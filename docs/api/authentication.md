## API Authentication

All requests to the ProbeTruth API must be authenticated using **secure, token-based authentication** provided by **Amazon Cognito**. This ensures robust, scalable access control to our deepfake detection services.

We no longer support static API keys for production access due to security and scalability limitations.

---

##  Overview of Authentication Flow

1. Clients **authenticate** using Amazon Cognito and obtain a **JWT access token**.
2. Clients include this token in the `Authorization` header when calling any protected API endpoint.
3. **API Gateway** validates the token before forwarding the request to the backend (e.g., Lambda, ECS, or SageMaker model).

---

## Getting Access

To gain access to the ProbeTruth API:

1. Contact us at [demo@probetruth.ai](mailto:demo@probetruth.ai) to request API access.
2. We’ll provision access to a **Cognito User Pool** and send you the necessary credentials (Client ID, Pool ID, Region).
3. Additionally, your IP address will be whitelisted for enhanced security.
4. You can authenticate via:
    - Hosted UI (Login page hosted by Cognito)
    - Directly using SDKs like AWS Amplify or Amazon Cognito Identity SDK.

---

## Token-Based Authentication

Once authenticated, your client will receive a **JWT access token** from Cognito. These tokens are valid for 24 hours.

Include this token in the `Authorization` header of all API requests:

Authorization: Bearer `<your_jwt_token_here>`

The token is automatically validated by our **API Gateway Authorizer** before any request is forwarded to the inference engine.

---

## Example (Python + JWT)
```
import requests

JWT_TOKEN = 'your_jwt_access_token_here'
API_URL = 'https://api.probetruth.ai/v1/inspection/{upload_id}'

headers = {
'Authorization': f'Bearer {JWT_TOKEN}'
}

response = requests.get(API_URL, headers=headers)

if response.ok:
print("Success:", response.json())
else:
print("Error:", response.status_code, response.text)
```
---
## Authenticating with Cognito

To get a token, you can authenticate with Cognito using:

### Option A: Hosted UI (OAuth2 Login)

You’ll be redirected to a Cognito-hosted login page. Upon login, Cognito redirects you back to your app with a token.

### Option B: Programmatic Login (for CLI or backend clients)

You can use the AWS Cognito Identity SDK:
```
import boto3

client = boto3.client('cognito-idp', region_name='your-region')

response = client.initiate_auth(
ClientId='your_cognito_client_id',
AuthFlow='USER_PASSWORD_AUTH',
AuthParameters={
'USERNAME': 'your_username',
'PASSWORD': 'your_password'
}
)

jwt_token = response['AuthenticationResult']['AccessToken']
```
---

## Security Best Practices

- **Never expose your token** in frontend JavaScript or public repositories.
- **Rotate credentials** periodically.
- Use third-party tools like **SecurityScorecard** to regularly assess the security posture of your application.

---

## Coming Soon

- OAuth2 scope-based permission control (e.g., `read:reports`, `submit:job`)
- Multi-tenant access via Cognito User Groups and Roles
- Temporary session tokens via AWS STS for advanced clients