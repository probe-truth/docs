# Upload Media

The `POST /v1/media` endpoint allows you to upload video, audio, or image files for deepfake forensic inspection. This is the first step in the ProbeTruth detection pipeline.

You can upload **videos**, **audio**, or **images** for forensic evaluation.

---

## Endpoint

`POST https://api.probetruth.ai/v1/media`

---

## Authentication

This endpoint requires a valid API key. Include it in the `Authorization` header. Tokens are valid for 24 hours.:

`Authorization: Bearer YOUR_API_KEY`

API is accessible from whitelisted IPs. Ensure your IP is registered with ProbeTruth. SecurityScorecard tools are used to validate service security.

---

## Supported Media Types

- — Video `.mp4` `.mkv` `.avi` `.mov` `.wmv` `.flv` `.webm` 
- — Audio `.mp3` `.wav` `.aac` `.flac` 
- — Image `.jpg` `.png` `.bmp` `.tiff` 
---

## Headers

| Key            | Value                        |
|----------------|------------------------------|
| Authorization  | Bearer `<your_jwt_token_here>` |
| Content-Type   | multipart/form-data          |

---

## Request Body

| Field        | Type     | Required | Description                            |
|--------------|----------|----------|----------------------------------------|
| file         | file     | Yes      | The media file to be analyzed          |
| filename     | string   | No       | Optional custom name for the upload    |

---

## Example: Upload Media

```
curl -X POST https://api.probetruth.ai/v1/media \
  -H "Authorization: Bearer <your_jwt_token_here>" \
  -F "file=@/path/to/your/video.mp4" \
```

**Response**
```
{
  "upload_id": "b91c0fe3-523d-4d3a-9e0e-17cc81c513ab",
  "status": "uploaded",
  "message": "Media successfully uploaded and queued for analysis."
}
```

---

## Error Responses
All endpoints return these common error responses:

400 Bad Request
```
{
  "error": "Invalid file type",
  "allowed_types": ["mp4", "mp3", ...]
}
```
401 Unauthorized

```
{
  "error": "Invalid authentication token"
}
```
413 Payload Too Large
```
{
  "error": "File size exceeds 500MB limit"
}
```
---

## Next Step

Once uploaded, use the returned `upload_id` to perform media inspection:

```
POST /v1/inspection
```

## Developer Notes

- Use unique filenames to avoid collisions in downstream processing.
- Media larger than 500MB may fail to upload depending on server-side configuration.
- Accepted media types are validated on the backend. If you're unsure, validate the file type before uploading.
- After uploading, you will receive a media_id used in subsequent API calls (inspect, generate report, etc.).