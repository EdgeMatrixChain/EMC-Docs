# OAuth Token API Documentation

## Overview

This API is used to obtain an OAuth token. Clients need to provide the necessary authorization information to receive an access token.

## Request URL

```
POST https://oauth.emchub.ai/emchub/api/oauth/token
```

## Request Headers

### Headers

- `Content-Type: application/x-www-form-urlencoded`
- `Authorization: Basic <base64_encoded_client_id_and_secret>`

```
Note: The value of the Authorization header is the Base64 encoded result of client_id:client_secret.
```

## Request Parameters

### Body Parameters

| Parameter     | Type   | Required | Description                                                  |
| ------------- | ------ | -------- | ------------------------------------------------------------ |
| grant_type    | string | Yes      | The grant type. Typical values: `authorization_code`, `refresh_token` |
| code          | string | No       | Authorization code (for `authorization_code` grant type)     |
| redirect_uri  | string | No       | Redirect URI (for `authorization_code` grant type,The current version uses a fixed value for the redirect URL: https://emchub.ai)           |
| refresh_token | string | No       | Refresh token (for `refresh_token` grant type)               |

## Request Examples

### Obtaining Token with Authorization Code

```
POST /emchub/api/oauth/token HTTP/1.1
Host: oauth.emchub.ai
Content-Type: application/x-www-form-urlencoded
Authorization: Basic Y2xpZW50X2lkOmNsaWVudF9zZWNyZXQ=

grant_type=authorization_code&code=auth_code_received&redirect_uri=https://yourapp.com/callback
```

### Obtaining Token with Refresh Token

```
POST /emchub/api/oauth/token HTTP/1.1
Host: oauth.emchub.ai
Content-Type: application/x-www-form-urlencoded
Authorization: Basic Y2xpZW50X2lkOmNsaWVudF9zZWNyZXQ=

grant_type=refresh_token&refresh_token=your_refresh_token
```

## Response Parameters

### Successful Response

| Parameter     | Type   | Description                                                  |
| ------------- | ------ | ------------------------------------------------------------ |
| access_token  | string | Access token                                                 |
| token_type    | string | Token type (usually "bearer")                                |
| expires_in    | int    | Validity of the access token in seconds                      |
| refresh_token | string | Refresh token (if applicable)                                |
| scope         | String | A scope is a string value that specifies a particular permission(read,write,admin) |

### Response Example

```
{
  "access_token": "your_access_token",
  "token_type": "Bearer",
  "expires_in": 3600,
  "refresh_token": "your_refresh_token",
  "scope":"read"
}
```

### Error Response

| Parameter | Type   | Description                      |
| --------- | ------ | -------------------------------- |
| _result   | string | Error code ,non-zero for errors. |
| _desc     | string | Error description                |
| data      | string | description                      |

### Error Example

```
{
    "_result": 4000,
    "data": "",
    "_desc": "{\"error\":\"invalid_grant\",\"error_description\":\"Invalid authorization code: Yb6gyd\"}"
}
```

## Notes

- Ensure to use HTTPS protocol when transmitting sensitive information (such as `client_secret` ) to maintain security.
- Provide appropriate parameters based on the grant type being used.
- Check the `expires_in` field in the response to refresh the token or obtain a new access token before it expires.
