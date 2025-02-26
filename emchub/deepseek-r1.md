# DeepSeeK API Call - Beta
The DeepSeek API uses an API format compatible with OpenAI. By modifying the configuration, you can use the OpenAI SDK or softwares compatible with the OpenAI API to access the DeepSeek API.


| Param          | Value                                                                             |
|----------------|-----------------------------------------------------------------------------------|
| api_url        | apply for an Static Api url or dynamic direct connection url obtained from EMCHub |
| EMCHub API Key | apply for an API key obtained from EMCHub                                         |
* The deepseek-chat model has been upgraded to DeepSeek-V3. The API remains unchanged. You can invoke DeepSeek-V3 by specifying model='deepseek-chat'.

* deepseek-reasoner is the latest reasoning model, DeepSeek-R1, released by DeepSeek. You can invoke DeepSeek-R1 by specifying model='deepseek-reasoner'.

* [How to obtain  dynamic direct connection url]()

## Invoke The Chat API
Once you have obtained an API key, you can access the DeepSeek API using the following example scripts. This is a non-stream example, you can set the stream parameter to true to get stream response.

### curl
````
curl <api_url> \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer <api_key>" \
  -d '{
        "model": "deepseek-chat",
        "messages": [
          {"role": "system", "content": "You are a helpful assistant."},
          {"role": "user", "content": "Hello!"}
        ],
        "stream": false
      }'
````
