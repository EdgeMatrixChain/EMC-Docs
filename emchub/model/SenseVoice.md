# SenseVoice API - Beta
This API allows you to convert an audio file (e.g., .mp3) into transcribed text using the SenseVoice speech recognition engine. It is suitable for voice assistants, transcription services, and speech-based applications.


| Param          | Value                                                                             |
|----------------|-----------------------------------------------------------------------------------|
| API URL        | apply for an Static Api url or dynamic direct connection url obtained from EMCHub |
| EMCHub API Key | apply for an API key obtained from EMCHub                                         |

* [How to obtain  dynamic direct connection url]()

## Invoke The API
Once you have obtained an API key, you can access the SenseVoice API using the following example scripts. 
### curl
````
curl -X POST <API URL> \
  -H 'Authorization: Bearer <EMCHub API Key>' \
  -H 'accept: application/json'   -F 'file=@test.mp3'
````
