
# Generate Forensic Report

After media inspection is complete (see [Media Inspection Endpoints](inspect_media.md)), a detailed forensic report can be generated. This report includes classification results (e.g., real, fake) along with the model’s confidence score, and can be downloaded as a PDF or JSON for documentation purposes.

---

## Endpoint

POST https://api.probetruth.ai/v1/reports


---

## Authentication

This endpoint requires a valid API key. Include it in the `Authorization` header. Tokens are valid for 24 hours.:

`Authorization: Bearer YOUR_API_KEY`

API is accessible from whitelisted IPs. Ensure your IP is registered with ProbeTruth. SecurityScorecard tools are used to validate service security.

---

## Headers

| Key             | Value                                    |
|-----------------|------------------------------------------|
| `Authorization` | `Bearer <your_jwt_token_here>`           |
| `Content-Type`  | `application/json`                       |
| `Accept`        | `application/pdf` or `application/json`  |

**Note:** The media type (PDF/JSON) is inferred from the Accept header. No need to specify this in the body.

---

## Request Body

| Field      | Type     | Required | Description                                  |
|------------|----------|----------|----------------------------------------------|
| `media_id` | `string` | Yes      | The unique ID of the media file to generate the report for. |
| `case_id`  | `string` | Yes      | Case number to associate the report with legal tracking.    |


#### Example Request

```bash
curl -X POST https://api.probetruth.ai/v1/reports \
  -H "Authorization: Bearer <your_jwt_token_here>" \
  -H "Content-Type: application/json" \
  -H "Accept: application/pdf" \
  -d '{
        "media_id": "b91c0fe3-523d-4d3a-9e0e-17cc81c513ab",
        "case_id": "CASE-2025-0001"
      }'
```

-----

## Response

### Success: PDF Report (200 OK) returns a binary stream of the PDF:

```
%PDF-1.7
1 0 obj
...binary PDF data...
%%EOF
```
The PDF file will be directly streamed in the response body with the `Content-Type: application/pdf` header.

### Success: JSON Report (200 OK) returns the URL of the JSON:
```
{
  "media_id": "b91c0fe3-523d-4d3a-9e0e-17cc81c513ab",
  "case_id": "CASE-2025-0001",
  "status": "suspicious",
  "confidence": 0.78,
  "report_url": "https://cdn.probetruth.ai/reports/CASE-2025-0001.json"
}
```

**Error Responses**

| HTTP Status Code | Response Body (JSON)                                     | Description                                                                 |
|------------------|----------------------------------------------------------|-----------------------------------------------------------------------------|
| `401 Unauthorized` | ` json { "error": "Unauthorized", "message": "Invalid or missing token." }  ` | Authentication failed. Ensure a valid JWT token is provided.              |
| `400 Bad Request`  | ` json { "error": "Bad Request", "message": "The 'media_id' field is required." }  ` | The `media_id` field is missing in the request body.                      |
| `404 Not Found`    | ` json { "error": "Not Found", "message": "Media with ID 'invalid-id' not found or analysis not completed." }  ` | The specified `media_id` does not exist or the analysis for it is not yet complete. |
| `500 Internal Server Error` | ` json { "error": "Internal Server Error", "message": "Failed to generate report." }  ` | An unexpected error occurred on the server while generating the report.    |

-----

## Request Example (Python)

```python

import requests

token = 'your_jwt_token_here'
media_id = 'b91c0fe3-523d-4d3a-9e0e-17cc81c513ab'
case_id = 'CASE-2025-0001'

headers = {
    'Authorization': f'Bearer {token}',
    'Content-Type': 'application/json',
    'Accept': 'application/pdf'
}

data = {
    'media_id': media_id,
    'case_id': case_id
}

response = requests.post('https://api.probetruth.ai/v1/reports', headers=headers, json=data, stream=True)

if response.status_code == 200:
    with open(f'report_{case_id}.pdf', 'wb') as f:
        for chunk in response.iter_content(chunk_size=8192):
            f.write(chunk)
    print("Report downloaded successfully.")
else:
    print(f"Failed to generate report: {response.status_code} - {response.text}")

```

-----

## Developer Notes
- Media type (PDF/JSON) is handled via the `Accept` header.
- IP whitelisting is enforced at the firewall level.
- All uploaded files are deleted after inspection; only SHA256 hashes and forensic reports are stored.
- `status` (`real`, `fake`) and confidence score are returned in the report.
- All endpoints are tested with [SecurityScorecard](https://securityscorecard.com) for cybersecurity compliance.
