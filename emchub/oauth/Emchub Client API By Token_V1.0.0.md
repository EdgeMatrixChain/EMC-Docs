# Client API By Token Documentation

## Overview

These endpoints allow you to query and modify user information using an access token for authentication.

## Endpoints

### 1. Query User Information

- **Endpoint**: https://oauth.emchub.ai/emchub/api/resource/user/selectByUser
- **Method**: GET
- **Description**: Retrieve the authenticated user's information.

#### Request Headers

| Header        | Type   | Required | Description                                       |
| ------------- | ------ | -------- | ------------------------------------------------- |
| Authorization | string | Yes      | Bearer token. Example: `Bearer your_access_token` |

#### Response

| Field                                  | Type    | Description                                                  |
| -------------------------------------- | ------- | ------------------------------------------------------------ |
| `_result`                              | integer | Result code, typically `0` for success and non-zero for errors. |
| `_desc`                                | string  | Description of the result, e.g., "success".                  |
| `_sid`                                 | string  | meaningless, default null                                    |
| `_login`                               | boolean | meaningless , typically `false`.                             |
| `data`                                 | object  | Container for user-specific data.                            |
| `data.userId`                          | integer | Unique ID of the user.                                       |
| `data.userCode`                        | string  | Code of the user, if applicable, otherwise `null`.           |
| `data.username`                        | string  | Username of the user.                                        |
| `data.email`                           | string  | Email address of the user.                                   |
| `data.userImage`                       | string  | URL of the user's profile image.                             |
| `data.status`                          | integer | Status of the user, e.g., `0` for active.2 for delete        |
| `data.type`                            | integer | Type of user, e.g., `0` for user.Reserved fields default to 0 |
| `data.createTime`                      | date    | utc time                                                     |
| `data.description`                     | string  | Description of the user, if any, otherwise `null`.           |
| `data.sub`                             | string  | Deprecated.unique field for google.                          |
| `data.emailVerified`                   | boolean | Indicates if the user's email is verified.                   |
| `data.signInProvider`                  | string  | Provider used for signing in, e.g., "google.com","twitter.com","web3wallet","github.com". |
| `data.userThirdAuths`                  | array   | List of third-party authentication details associated with the user. |
| `data.userThirdAuths[].id`             | integer | Unique ID for the third-party authentication record.         |
| `data.userThirdAuths[].userId`         | integer | ID of the user associated with this third-party authentication. |
| `data.userThirdAuths[].thirdUid`       | string  | Unique ID provided by the third-party provider.              |
| `data.userThirdAuths[].signInProvider` | string  | Provider used for signing in, e.g., "google.com".            |
| `data.userThirdAuths[].identities`     | string  | JSON string containing third-party identities associated with the user. |
| `data.userThirdAuths[].name`           | string  | Name of the user as provided by the third-party provider.    |
| `data.userThirdAuths[].picture`        | string  | URL of the user's profile picture as provided by the third-party provider. |
| `data.userThirdAuths[].email`          | string  | Email address of the user as provided by the third-party provider. |
| `data.userThirdAuths[].emailVerified`  | boolean | Indicates if the user's email is verified by the third-party provider. |
| `data.userThirdAuths[].status`         | integer | Status of the third-party authentication record.Default: 0 normal, 2 delete |
| `data.userThirdAuths[].createTime`     | date    | Date utc time                                                |

#### Response Example

```
{
    "_result": 0,
    "_desc": "success",
    "_sid": null,
    "_login": false,
    "data": {
        "userId": 1,
        "userCode": null,
        "username": "",
        "email": "",
        "userImage": "",
        "status": 0,
        "type": 0,
        "createTime": ,
        "description": null,
        "sub": "",
        "emailVerified": true,
        "signInProvider": "google.com",
        "userThirdAuths": [
            {
                
            }
        ]
    }
}
```

#### Example Request

```
GET /emchub/api/resource/user/selectByUser HTTP/1.1
Host: https://oauth.emchub.ai
Authorization: Bearer your_access_token
```

### 2. Query User Blance 

- **Endpoint**: https://oauth.emchub.ai/emchub/api/resource/user/queryBlanceByUserId
- **Method**: GET
- **Description**: Retrieve the authenticated user's blance.

#### Request Headers

| Header        | Type   | Required | Description                                       |
| ------------- | ------ | -------- | ------------------------------------------------- |
| Authorization | string | Yes      | Bearer token. Example: `Bearer your_access_token` |

#### Response

| Field     | Type    | Description                                                  |
| --------- | ------- | ------------------------------------------------------------ |
| `_result` | integer | Result code, typically `0` for success and non-zero for errors. |
| `_desc`   | string  | Description of the result, e.g., "success".                  |
| `_sid`    | string  | Deprecated.default null.                                     |
| `_login`  | boolean | Deprecated.default `false`.                                  |
| `data`    | number  | The balance amount returned, as a numeric value.             |

### Example Response

```
{
    "_result": 0,
    "_desc": "success",
    "_sid": null,
    "_login": false,
    "data": 1
}
```

#### Example Request

```
GET /emchub/api/resource/user/queryBlanceByUserId HTTP/1.1
Host: https://oauth.emchub.ai
Authorization: Bearer your_access_token
```

## Error Responses

### 401 Unauthorized

Indicates that the access token is missing, invalid, or expired.

#### Response Example

```
{
  "error": "unauthorized",
  "error_description": "Full authentication is required to access this resource"
}
```

### 403 Forbidden

Indicates that the user does not have permission to perform the requested operation.

#### Response Example

```
{
  "error": "forbidden",
  "error_description": "You do not have permission to access this resource"
}
```

### 400 Bad Request

Indicates that the request body contains invalid data.

#### Response Example

```
{
  "error": "invalid_request",
  "error_description": "The request is missing required parameters or contains invalid parameters"
}
```