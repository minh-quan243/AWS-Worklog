---
title : "Thiết lập Lambda Functions"
date: "2000-01-01"
weight: 5
chapter : false
pre : " <b> 4.5. </b> "
---

Hàm Lambda này được kích hoạt tự động khi có một tệp âm thanh mới được đưa vào tiền tố `raw_audio/` của bucket S3 Âm thanh. Hàm này đọc một bảng băm (hash table) được lưu trữ trong cùng bucket để tra cứu tên công việc phiên âm đã được gán trước, sau đó khởi chạy một công việc AWS Transcribe và ghi kết quả đầu ra vào tiền tố `transcripts/`.

**Runtime (Môi trường thực thi):** Python 3.11  
**Trigger (Kích hoạt):** S3 — bucket `one4allthing`, tiền tố `raw_audio/`, loại sự kiện `PUT`

---

#### Bước 1 — Tạo Hàm Lambda

Điều hướng đến **Lambda** trong AWS Console. Nhấp vào **Create function** (Tạo hàm).

- **Function name (Tên hàm):** `audio2text`
- **Runtime:** Python 3.11
- **Architecture (Kiến trúc):** x86_64
- **Execution role (Vai trò thực thi):** Tạo một role mới với các quyền Lambda cơ bản (bạn sẽ thêm các quyền S3 và Transcribe ở bước tiếp theo)

Nhấp vào **Create function** (Tạo hàm).

![Tạo Hàm Lambda](/images/4-Workshop/Lambda/B1_Create_Lambda_audio2text.png)

---

#### Bước 2 — Mở Hàm và Thêm Mã Nguồn

Sau khi tạo, nhấp vào hàm `audio2text` để mở trang chi tiết của nó. Cuộn xuống phần **Code source** (Nguồn mã) và thay thế trình xử lý (handler) mặc định bằng mã bên dưới.

![Mở hàm audio2text](/images/4-Workshop/Lambda/B2_Paste_Code.png)

Dán đoạn mã sau vào `lambda_function.py`:

```python
import json
import urllib.parse
import boto3
from botocore.exceptions import ClientError

transcribe = boto3.client("transcribe", region_name="ap-southeast-1")
s3 = boto3.client("s3", region_name="ap-southeast-1")

OUTPUT_BUCKET = "one4allthing"
OUTPUT_PREFIX = "transcripts/"
HASH_TABLE_KEY = "hash_table.json"


def _hash(key, size=16):
    return sum(ord(c) for c in key) % size


def load_table(s3_client, bucket, key):
    try:
        resp = s3_client.get_object(Bucket=bucket, Key=key)
        return json.loads(resp["Body"].read().decode("utf-8"))
    except ClientError as e:
        print(f"Cannot load table s3://{bucket}/{key}: {e}")
        return None


def get_from_table(table, k: str):
    if not table:
        return None
    idx = _hash(k, len(table))
    for key, value in table[idx]:
        if key == k:
            return value
    return None


def find_job_name(table, s3_key: str):
    value = get_from_table(table, s3_key)
    if value:
        return value
    basename = s3_key.split("/")[-1]
    value = get_from_table(table, basename)
    if value:
        return value
    return None


def media_format_from_content_type(content_type: str):
    if not content_type:
        return None
    content_type = content_type.lower().strip()
    mapping = {
        "audio/wav": "wav", "audio/x-wav": "wav", "audio/wave": "wav",
        "audio/mpeg": "mp3", "audio/mp3": "mp3",
        "audio/flac": "flac", "audio/x-flac": "flac",
        "audio/mp4": "mp4", "audio/x-m4a": "m4a", "audio/m4a": "m4a",
        "audio/ogg": "ogg", "audio/webm": "webm", "audio/amr": "amr",
        "video/mp4": "mp4",
    }
    return mapping.get(content_type)


def lambda_handler(event, context):
    print("Lambda started")
    print(json.dumps(event))
    results = []

    try:
        first_bucket = event["Records"][0]["s3"]["bucket"]["name"]
    except Exception:
        return {"statusCode": 400, "body": json.dumps({"error": "Invalid S3 event"})}

    table = load_table(s3, first_bucket, HASH_TABLE_KEY)
    if table is None:
        raise ValueError(f"Cannot load hash table from s3://{first_bucket}/{HASH_TABLE_KEY}")

    for record in event.get("Records", []):
        try:
            bucket = record["s3"]["bucket"]["name"]
            key = urllib.parse.unquote_plus(record["s3"]["object"]["key"])
            print(f"Processing: s3://{bucket}/{key}")

            if not key.startswith("raw_audio/"):
                print(f"Skip non-raw-audio file: s3://{bucket}/{key}")
                continue

            job_name = find_job_name(table, key)
            if not job_name:
                raise ValueError(f"Key not found in hash table: {key}")

            head = s3.head_object(Bucket=bucket, Key=key)
            content_type = head.get("ContentType", "")
            media_format = media_format_from_content_type(content_type)

            if not media_format:
                raise ValueError(f"Unsupported ContentType for key: {key}")

            media_uri = f"s3://{bucket}/{key}"
            resp = transcribe.start_transcription_job(
                TranscriptionJobName=job_name,
                IdentifyLanguage=True,
                MediaFormat=media_format,
                Media={"MediaFileUri": media_uri},
                OutputBucketName=OUTPUT_BUCKET,
                OutputKey=f"{OUTPUT_PREFIX}{job_name}.json"
            )

            item = {
                "input_key": key,
                "job_name": job_name,
                "status": resp["TranscriptionJob"]["TranscriptionJobStatus"],
                "output_s3_uri": f"s3://{OUTPUT_BUCKET}/{OUTPUT_PREFIX}{job_name}.json"
            }
            print(json.dumps(item, ensure_ascii=False))
            results.append(item)

        except Exception as e:
            err = {
                "input_key": record.get("s3", {}).get("object", {}).get("key"),
                "error": str(e)
            }
            print(json.dumps(err, ensure_ascii=False))
            results.append(err)

    return {"statusCode": 200, "body": json.dumps(results, ensure_ascii=False)}
```

Nhấp vào **Deploy** (Triển khai) để lưu mã.

---

#### Bước 3 — Thêm S3 Trigger

Trong tab Configuration (Cấu hình) của hàm, nhấp vào Triggers → Add trigger (Trình kích hoạt → Thêm trình kích hoạt).

Cấu hình trigger:
- **Trigger type** (Loại trình kích hoạt): S3
- **Bucket**: `one4allthing`
- **Event types** (Loại sự kiện): `PUT` (Tất cả các sự kiện tạo object)
- **Prefix** (Tiền tố): `raw_audio/`
- Xác nhận cảnh báo gọi đệ quy (recursive invocation) và nhấp vào **Add** (Thêm).

![Thêm S3 Trigger](/images/4-Workshop/Lambda/B3_Add_S3_Trigger.png)

---

#### Thêm Quyền IAM vào Role của Lambda

Điều hướng đến tab **Configuration → Permissions** (Cấu hình → Quyền) của Lambda. Nhấp vào tên execution role để mở nó trong IAM. Thêm inline policy (chính sách nội tuyến) sau để hàm có thể đọc từ S3 và khởi chạy các công việc Transcribe:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:GetObject",
        "s3:HeadObject"
      ],
      "Resource": "arn:aws:s3:::one4allthing/*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "transcribe:StartTranscriptionJob"
      ],
      "Resource": "*"
    },
    {
      "Effect": "Allow",
      "Action": [
        "s3:PutObject"
      ],
      "Resource": "arn:aws:s3:::one4allthing/transcripts/*"
    }
  ]
}
```

---

{{% notice tip %}}
✅ The `audio2text` Lambda is deployed and will automatically start a Transcribe job each time an audio file is uploaded to `s3://one4allthing/raw_audio/`.
{{% /notice %}}