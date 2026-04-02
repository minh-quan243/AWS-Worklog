---
title : "Mã Alert Dispatch"
date: "2000-01-01"
weight : 09
chapter : false
pre : " <b> 5.11.9. </b> "
---

```python

import os
import json
import logging
import urllib.request
import boto3
from botocore.exceptions import ClientError
import html

# --- Telegram ENV ---

# BOT_TOKEN = os.environ.get('BOT_TOKEN')
# CHAT_ID = os.environ.get('CHAT_ID')
# MESSAGE_THREAD_ID = os.environ.get('MESSAGE_THREAD_ID')

# --- Slack ENV ---

SLACK_WEBHOOK_URL = os.environ.get("SLACK_WEBHOOK_URL")

# --- SES ENV ---
SENDER_EMAIL = os.environ.get('SENDER_EMAIL')
RECIPIENT_EMAIL = os.environ.get('RECIPIENT_EMAIL') # Can now be "a@b.com, c@d.com"
AWS_REGION = os.environ.get('AWS_REGION', 'ap-southeast-1')

# --- Setup ---

# TELEGRAM_URL = f"https://api.telegram.org/bot{BOT_TOKEN}/sendMessage" if BOT_TOKEN else None

logger = logging.getLogger()
logger.setLevel(logging.INFO)

# Initialize SES Client
ses_client = boto3.client('ses', region_name=AWS_REGION)


# ====================================================================
# SEND TO TELEGRAM
# ====================================================================

# def send_to_telegram(finding, chat_id, thread_id):
#     logger.info("Formatting message for Telegram...")
# ... (Code commented out, keeping as is or translating if needed, but it is commented out so skipping detailed translation for brevity unless enabled)

# ====================================================================
# SEND TO SLACK
# ====================================================================

def send_to_slack(finding):
    if not SLACK_WEBHOOK_URL:
        logger.warning("Slack ENV missing. Skipping.")
        return

    severity_num = finding.get("severity", 0)
    title        = finding.get("title", "No Title")
    description  = finding.get("description", "No Description")
    region       = finding.get("region", "N/A")
    account_id   = finding.get("accountId", "N/A")
    finding_type = finding.get("type", "N/A")

    if severity_num >= 7:
        color = "#ff0000"
        sev = "🔴 CAO (HIGH)"
    elif severity_num >= 4:
        color = "#ffa500"
        sev = "🟠 TRUNG BÌNH (MEDIUM)"
    else:
        color = "#007bff"
        sev = "🔵 THẤP (LOW)"

    payload = {
        "text": f"🚨 {sev} – {title}",
        "attachments": [{
            "color": color,
            "blocks": [
                {"type": "header", "text": {"type": "plain_text", "text": f"🚨 GuardDuty Finding: {title}"}},
                {"type": "section", "fields": [
                    {"type": "mrkdwn", "text": f"*Mức độ (Severity):*\n{sev}"},
                    {"type": "mrkdwn", "text": f"*Khu vực (Region):*\n{region}"}
                ]},
                {"type": "section", "text": {"type": "mrkdwn", "text": f"*Mô tả (Description):*\n{description}"}},
                {"type": "divider"},
                {"type": "context", "elements": [
                    {"type": "mrkdwn", "text": f"*Tài khoản (Account):* `{account_id}`"},
                    {"type": "mrkdwn", "text": f"*Loại (Type):* `{finding_type}`"}
                ]}
            ]
        }]
    }

    try:
        req = urllib.request.Request(
            SLACK_WEBHOOK_URL,
            data=json.dumps(payload).encode("utf-8"),
            headers={"Content-Type": "application/json"}
        )
        with urllib.request.urlopen(req) as response:
            logger.info("Slack response: " + response.read().decode("utf-8"))
    except Exception as e:
        logger.error(f"SLACK FAILED: {e}")


# ====================================================================
# SEND TO SES EMAIL (UPDATED FOR MULTIPLE RECIPIENTS)
# ====================================================================
def send_to_ses(finding):
    if not SENDER_EMAIL or not RECIPIENT_EMAIL:
        logger.warning("SES Env vars missing. Skipping Email.")
        return

    logger.info("Formatting message for SES Email...")

    recipient_list = [email.strip() for email in RECIPIENT_EMAIL.split(',')]

    severity_num = finding.get("severity", 0)
    title        = finding.get("title", "No Title")
    description  = finding.get("description", "No Description")
    region       = finding.get("region", "N/A")
    account_id   = finding.get("accountId", "N/A")
    finding_type = finding.get("type", "N/A")
    finding_id   = finding.get("id", "N/A")

    if severity_num >= 7:
        color = "#ff0000"
        sev = "HIGH (CAO)"
    elif severity_num >= 4:
        color = "#ffa500"
        sev = "MEDIUM (TRUNG BÌNH)"
    else:
        color = "#007bff"
        sev = "LOW (THẤP)"

    html_body = f"""
    <html>
    <head>
        <style>
            body {{ font-family: Arial, sans-serif; line-height: 1.6; color: #333; }}
            .container {{ width: 100%; max-width: 600px; margin: 0 auto; border: 1px solid #ddd; border-radius: 8px; overflow: hidden; }}
            .header {{ background-color: {color}; color: white; padding: 15px; text-align: center; }}
            .content {{ padding: 20px; }}
            .footer {{ background-color: #f4f4f4; padding: 10px; text-align: center; font-size: 12px; color: #666; }}
            .label {{ font-weight: bold; color: #555; }}
        </style>
    </head>
    <body>
        <div class="container">
            <div class="header">
                <h2>🚨 Cảnh báo GuardDuty: {sev}</h2>
            </div>
            <div class="content">
                <h3>{title}</h3>
                <p>{description}</p>
                <hr>
                <p><span class="label">ID Tài khoản:</span> {account_id}</p>
                <p><span class="label">Khu vực:</span> {region}</p>
                <p><span class="label">Loại:</span> {finding_type}</p>
                <p><span class="label">ID Phát hiện:</span> {finding_id}</p>
            </div>
            <div class="footer">
                Được tạo bởi AWS Lambda Alert Dispatch
            </div>
        </div>
    </body>
    </html>
    """

    try:
        response = ses_client.send_email(
            Source=SENDER_EMAIL,
            Destination={'ToAddresses': recipient_list}, # Uses the list now
            Message={
                'Subject': {'Data': f"GuardDuty Alert [{sev}]: {title}", 'Charset': 'UTF-8'},
                'Body': {'Html': {'Data': html_body, 'Charset': 'UTF-8'}}
            }
        )
        logger.info(f"SES Email sent to {len(recipient_list)} recipients! MessageId: {response['MessageId']}")
    except ClientError as e:
        logger.error(f"SES FAILED: {e.response['Error']['Message']}")


# ====================================================================
# MAIN HANDLER
# ====================================================================
def lambda_handler(event, context):
    logger.info(f"Event received: {json.dumps(event)}")

    try:
        sns_message_raw = event["Records"][0]["Sns"]["Message"]
        message_data = json.loads(sns_message_raw)
        
        # Normalization Logic
        finding = {}
        if "detail-type" in message_data and message_data["detail-type"] == "GuardDuty Finding":
            detail = message_data["detail"]
            finding = {
                "severity": detail.get("severity", 0),
                "title": detail.get("title", "GuardDuty Finding"),
                "description": detail.get("description", "No description provided"),
                "accountId": detail.get("accountId", "N/A"),
                "region": detail.get("region", "N/A"),
                "type": detail.get("type", "N/A"),
                "id": detail.get("id", "N/A")
            }
        elif "AlarmName" in message_data:
            state = message_data.get("NewStateValue")
            severity = 8 if state == "ALARM" else 0
            finding = {
                "severity": severity,
                "title": f"CloudWatch Alarm: {message_data.get('AlarmName')}",
                "description": message_data.get("NewStateReason", "State change detected"),
                "accountId": message_data.get("AWSAccountId", "N/A"),
                "region": message_data.get("Region", "N/A"),
                "type": "CloudWatch Alarm",
                "id": "N/A"
            }
        else:
            finding = {
                "severity": 0,
                "title": "Unknown Alert",
                "description": f"Raw Payload: {json.dumps(message_data)}",
                "accountId": "N/A",
                "region": "N/A",
                "type": "Unknown",
                "id": "N/A"
            }
            
    except Exception as e:
        logger.error(f"FATAL: Could not parse incoming SNS event: {e}")
        return {"statusCode": 500}

    # --- Send Telegram ---
    # if BOT_TOKEN and CHAT_ID:
    #     send_to_telegram(finding, CHAT_ID, MESSAGE_THREAD_ID)

    # --- Send Slack ---
    if SLACK_WEBHOOK_URL:
        send_to_slack(finding)

    # --- Send SES Email ---
    if SENDER_EMAIL and RECIPIENT_EMAIL:
        send_to_ses(finding)

    return {"statusCode": 200, "body": "Dispatch complete"}
```