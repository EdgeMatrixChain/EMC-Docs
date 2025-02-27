# API Documentation: Query API URL

## Endpoint

### `GET https://emchub.ai/emchub/api/client/thirdOpenApi/queryApiAddress`

## Description

This API is used to query the API address based on the given `appOrigin` and `apiPath`. It returns both static URL and dynamic direct URLs for accessing the API.

## Request Parameters

| Parameter   | Type   | Required | Default Value | Description                                                             |
| ----------- | ------ | -------- | ------------- | ----------------------------------------------------------------------- |
| `appOrigin` | String | Yes       | ""            | The origin of the application. If empty, an error response is returned. |
| `apiPath`   | String | Yes       | ""            | The API path to query. If empty, an error response is returned.         |

## Response Format

### Success Response

#### HTTP Status Code: `200 OK`

```json
{
    "_result": 0,
    "_desc": "Success",
    "data": {
        "dynamicURL": [
            "http://relay_host:relay_proxy_port/edge/{nodeId}/{port}{apiPath}",
            ...
        ],
        "staticURL": "https://openapi.emchub.ai/emchub/api/openapi/task/executeTaskByUser/{appOrigin}/{apiPath}"
    }
}
```

### Error Responses

#### Case 1: `appOrigin` is empty

```json
{
    "_result": 1,
    "_desc": "AppOrigin is empty"
}
```

#### Case 2: `apiPath` is empty

```json
{
    "_result": 1,
    "_desc": "ApiPath is empty"
}
```

#### Case 3: No available nodes for the model

```json
{
    "_result": 1,
    "_desc": "The current image has not been deployed yet, so there is no available address for now"
}
```

## Example Request

### Request URL

```
GET https://emchub.ai/emchub/api/client/thirdOpenApi/queryApiAddress?appOrigin=myApp&apiPath=/test
```

### Example Response

```json
{
    "_result": 0,
    "_desc": "Success",
    "data": {
        "dynamicURL": [
            "http://192.168.1.1:8080/edge/1234/8000/test",
            "http://192.168.1.2:8080/edge/5678/8001/test"
        ],
        "staticURL": "https://openapi.emchub.ai/emchub/api/openapi/task/executeTaskByUser/myApp/test"
    }
}
```

## Notes

- If `apiPath` does not start with `/`, the system will automatically prepend it with `/`.
- The dynamic URL list is generated randomly from available node configurations.
- The static URL follows a fixed pattern and is based on `appOrigin` and `apiPath`.


