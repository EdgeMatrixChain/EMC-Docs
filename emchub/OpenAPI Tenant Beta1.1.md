# OpenAPI Tenant Beta1.1

## Request Introduction

```
 Tenants can use this api for free.
```

1.Domain name:

```
https://openapi.emchub.ai
```

2.Request Method:

POST, Content-Type is application/json.

3.Character Encoding:

UTF-8.

4.Interface Parameter Description:

| Parameter   | Type   | Required | Description                                             |
| :---------- | ------ | -------- | ------------------------------------------------------- |
| appid       | String | Yes      | Fixed value issued by the open platform, e.g., cat      |
| nonce       | String | Yes      | Random numeric string                                   |
| action      | String | Yes      | Request interface name, action = specific business name |
| sign        | String | Yes      | Signature                                               |
| requestBody | String | Yes      | Specific business request parameters                    |


The requestBody parameter format is a JSON string, for example: {"name":"create"}. If there are no parameters in the requestBody parameter for the request interface, pass an empty JSON string, for example: {}.

### Signature Generation Steps

Step 1: Concatenate the interface parameters appid, nonce, action, and requestBody into a string called stringA using the URL key-value pair format (i.e., key1=value1&key2=value2...).

Step 2: Append the secret to the end of stringA to obtain the stringSignTemp string. Calculate the SHA1 hash of stringSignTemp to obtain the sign value, signValue.

Assuming the following parameters are being sent:

~~~
appid: cat_shark
action: walletCreate
nonce: 1226202735
requestBody:{"phone":"13900001111","wallet_type":0}
~~~

Step 1: Generate StringA by formatting the parameters in the key=value format.

~~~
stringA="appid=cat_shark&nonce=1226202735&action=walletCreate&requestBody={"phone":"13900001111","wallet_type":0}"
~~~


Step 2: Concatenate the API secret key.

For example, if the secret key provided by the open platform is "ef149163-276e-11ed-8589-b8599f24f354".

~~~
stringSignTemp=stringA+"&secret=ef149163-276e-11ed-8589-b8599f24f354"
sign=SHA1(stringSignTemp)="376e0de35aade4117fc00c69a2c5b25421a8e083"
~~~

    When using SHA1 encryption, use the UTF-8 character encoding.

### Random Numeric String Generation (Nonce)

It is recommended to generate a random numeric string using a random function and then convert it to a string.


### Interface and Specific Business Parameters

#### SyncTaskTenant

API Description:

This API is used to submit a synchronous request to the server for syncTaskTenant. Upon receiving the request, the server will forward the request and wait for the request response.

API PATH :/emchub/api/openapi/task/syncTaskTenant

action: syncTaskTenant

| requestBody Parameter | require | type        | description                                                  |
| --------------------- | ------- | ----------- | ------------------------------------------------------------ |
| apiPath               | Yes     | String      | apiPath                                                      |
| apiMethod             | Yes     | String      | apiMethod                                                    |
| modelHash             | NO      | String      | modelHash                                                    |
| appOrigin             | Yes     | String      | appOrigin                                                    |
| taskType              | No      | Integer     | type value : default 4 generic                               |
|                       |         |             | 0 TEXTTOIMAGE                                                |
|                       |         |             | 2 llm                                                        |
|                       |         |             | 4 generic                                                    |
| generativeParameters  | Yes     | Json String | parameter                                                    |
| callbackUrl           | No      | String      | Async callback notification address when the text-to-image task is completed; the callback interface uses POST requests uniformly, with Content-Type in application/json format; when the json object returned by the callback interface contains the _result field of 0, it indicates a successful callback, and other cases indicate a callback failure, and a callback notification will be issued again; |

generativeParameters : 

{

"eg":"11"

}



Response Example:

{

  "_result": 0,   // 0 :success , other failed

  "_desc": "success",

 "_taskSn": "",

  "responseBody":   //  Content returned by the access party interface 

}

#### AsyncTaskTenant

API Description:

This API is used to submit a asynchronous request to the server for asyncTaskTenant. Upon receiving the request, the service immediately returns the forwarding success or failure, and the access party uses the response tasksSn to actively query the task execution results.

API PATH :/emchub/api/openapi/task/asyncTaskTenant

action: asyncTaskTenant

| requestBody Parameter | require | type        | description                                                  |
| --------------------- | ------- | ----------- | ------------------------------------------------------------ |
| apiPath               | Yes     | String      | apiPath                                                      |
| apiMethod             | Yes     | String      | apiMethod                                                    |
| modelHash             | NO      | String      | modelHash                                                    |
| appOrigin             | Yes     | String      | appOrigin                                                    |
| taskType              | No      | Integer     | type value : default 4 generic                               |
|                       |         |             | 0 TEXTTOIMAGE                                                |
|                       |         |             | 2 llm                                                        |
|                       |         |             | 4 generic                                                    |
| generativeParameters  | Yes     | Json String | parameter                                                    |
| callbackUrl           | No      | String      | Async callback notification address when the text-to-image task is completed; the callback interface uses POST requests uniformly, with Content-Type in application/json format; when the json object returned by the callback interface contains the _result field of 0, it indicates a successful callback, and other cases indicate a callback failure, and a callback notification will be issued again; |

generativeParameters : 

{

"eg":"11"

}



Response Example:

{

  "_result": 0,   // 0 :success , other failed

  "_desc": "success",

 "_taskSn": "",  // sn

  "responseBody":   //  null 

}

#### Query Task Result

API Description:

Query task details based on the task SN.

API PATH : /emchub/api/openapi/task/queryTaskBySn

Action: queryTaskBySn

| requestBody Parameter | Required | Type   | Description               |
| --------------------- | -------- | ------ | ------------------------- |
| taskSn                | Yes      | String | Unique task serial number |

Response Example:

{

  "_result": 0,

  "_desc": "success",

  "_sid": **null**,

  "_login": **false**,

  "data": {} // task object returned

}

| Task Object Name     | Type    | Description                                                  |
| -------------------- | ------- | ------------------------------------------------------------ |
| id                   | Integer | Primary key                                                  |
| taskSn               | String  | Task serial number                                           |
| appId                | String  | Application ID                                               |
| modelHash            | String  | Model hash                                                   |
| nodeId               | String  | Node ID                                                      |
| generativeParameters | String  | Generative parameters                                        |
| taskType             | Integer | Task type (0: Text-to-Image)                                 |
| fileUrl              | String  | File path                                                    |
| createTime           | Date    | Creation time                                                |
| status               | Integer | Status: 0 - Pending, 1 - In Progress, 2 - Completed, 3 - Failed, 4 - Timeout, 5 - Deleted |
| requestTimes         | Integer | Number of task execution attempts (if exceeded, the generative process fails) |
| finishTime           | Date    | Task completion time                                         |
| callbackTime         | Date    | Callback time                                                |
| callbackStatus       | Integer | Callback status: 0 - Waiting for completion callback, 1 - Callback in progress, 2 - Callback successful, 3 - Callback failed, 4 - Callback timeout |
| callbackUrl          | String  | Callback URL                                                 |
| callbackType         | String  | Callback strategy (used for retrying failed callbacks)       |
|                      |         |                                                              |
