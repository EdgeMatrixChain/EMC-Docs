# Gemma API
We have deployed Gemma 3 27B locally using the high-performance vLLM framework. This setup allows us to expose a OpenAI-compatible API, making it easy to integrate with existing applications and tools.

| Param          | Value                                                                           |
|----------------|---------------------------------------------------------------------------------|
| API URL        | apply for a static Api url or dynamic direct url obtained from EMCHub |
| EMCHub API Key | apply for an API key obtained from EMCHub                                       |

* Using dynamic direct connection url to call APIs will result in faster response times, but because edge nodes in P2P networks cannot guarantee constant availability, you need to manage the availability of urls yourself.

[How to obtain  dynamic direct url](https://github.com/EdgeMatrixChain/EMC-Docs/blob/main/emchub/query%20dynamic%20url.md)

## Invoke The Chat API
Once you have obtained an API key, you can access the Gemma API using the following example scripts. This is a non-stream example, you can set the stream parameter to true to get stream response.

### curl
````
curl <API URL> \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer <EMCHub API Key>" \
  -d '{
        "messages": [
          {"role": "system", "content": "You are a helpful assistant."},
          {"role": "user", "content": "Hello!"}
        ],
        "stream": false
      }'
````
