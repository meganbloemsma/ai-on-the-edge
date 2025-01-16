# Azure Speech container learnings

Original POC (on the cloud) used Azure Speech to do language detection + speech-to-text. This was [quick to setup](src/azure-speech-quickstart.py) and worked well but we noticed some latency in the translations, which was not ideal for our use case.

So we tried [containerizing Azure Speech](https://learn.microsoft.com/en-us/azure/ai-services/speech-service/speech-container-howto) to see whether the latency would improve.

The Azure Speech container version is different than the cloud version. For starters, it requires two services instead of one to achieve the same POC:

* Azure Language (containerized) for language detection
* Azure Speech (containerized) for speech-to-text

Secondly, for input detection it recognized *only US english (en-US)*. Even after removing all autodetect functionality from our code it is unable to detect any other language.

The container is quite large: about 8+ GB. This means the deployment of the container takes quite some time. Depending on your use case, this could be a blocker.
That being said, there are some things you can try to decrease the size, such as selecting which languages it should detect up front (instead of loading all languages).

It is unclear how it works when multiple languages are being spoken in a conversation. We have been unable to test yet, because of the time it took to figure out the input language detection issue.

## Documentation used

* [Install and run speech container](https://learn.microsoft.com/en-us/azure/ai-services/speech-service/speech-container-howto)
* [Speech-to-text container](https://learn.microsoft.com/en-us/azure/ai-services/speech-service/speech-container-stt?tabs=container&pivots=programming-language-python)

## Extra

If you're looking for a fun example of speech-to-text, check out my [send message project](https://github.com/meganbloemsma/send-message). Based on the 'Sending' spell in Dungeons and Dragons,  it records your message real-time and transcribes it to text - capping it at 25 words.