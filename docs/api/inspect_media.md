# Inspect Media
Once a media file is successfully uploaded via the [Upload Media Endpoint](upload.md), you can initiate a forensic deepfake analysis using the Media Inspection endpoint.

We only store the hash of the file and the forensic report. The media file is deleted after inspection.

Once a media file is successfully uploaded using the [Upload Media Endpoints](upload.md), you can initiate deepfake analysis by calling the dedicated inspection endpoint for the specific media type and providing the `upload_id` in the request body.

---
There are separate endpoints to trigger the inspection process based on the type of uploaded media.

### 1. Initiate Media Inspection

The `POST /v1/inspection` endpoint triggers the deepfake analysis pipeline for a previously uploaded media.

## Endpoint 

`POST https://api.probetruth.ai/v1/inspection`

#### Authentication

This endpoint requires a valid API key. Include it in the `Authorization` header. Tokens are valid for 24 hours.:

`Authorization: Bearer YOUR_API_KEY`

API is accessible from whitelisted IPs. Ensure your IP is registered with ProbeTruth. SecurityScorecard tools are used to validate service security.


#### Request Body

| Field       | Type   | Required | Description                                  |
|-------------|--------|----------|----------------------------------------------|
| `upload_id` | string | Yes      | The unique ID of the uploaded media file.    |
| `case_id`   | string | Yes      | Identifier for report tracking & generation  |

**Content-Type:** `application/json`

#### Example Request
```
curl -X POST https://api.probetruth.ai/v1/inspection \
  -H "Authorization: Bearer <your_jwt_token_here>" \
  -H "Content-Type: application/json" \
  -d '{
        "upload_id": "845f9ac7-9a71-4fcb-8e25-2b7234c6e3fc",
        "case_id": "CASE-001"
      }'

```

### Example Success Response (Inspection Initiated)
```
{
  "upload_id": "845f9ac7-9a71-4fcb-8e25-2b7234c6e3fc",
  "case_id": "CASE-001",
  "analysis_status": "queued",
  "message": "Media inspection initiated."
}
```

### Missing Upload ID or Case ID
```
{
  "error": "Bad Request",
  "message": "The 'upload_id' and 'case_id' fields are required."
}
```
### Invalid Upload ID
```
{
  "error": "Not Found",
  "message": "Media with upload ID 'invalid-id' not found."
}
```
### Unauthorized Token
```
{
  "error": "Unauthorized",
  "message": "Invalid or missing token."
}
```
### Incorrect Media Type (Auto-detected internally)
```
{
  "error": "Invalid Request",
  "message": "Upload ID 'xyz' does not correspond to a valid media file."
}
```

## 2. Get Media Inspection Status

### Endpoint

`GET https://api.probetruth.ai/v1/inspection/{upload_id}`

This returns the current analysis status and the final result if available.

### Success Response
```
{
  "upload_id": "845f9ac7-9a71-4fcb-8e25-2b7234c6e3fc",
  "case_id": "CASE-001",
  "status": "completed",
  "result": "fake",
  "confidence": 0.92,
  "hash": "SHA256:ae29cbf7d2...",
  "report_available": true
}
```
### Status Values
- queued
- processing
- completed
- failed

#### Security & Best Practices
- Tokens are valid for 24 hours  
- IP whitelisting enforced for access  
- Files are deleted post-inspection  
- Only SHA256 file hashes are stored  
- SecurityScorecard is used periodically to test API security  