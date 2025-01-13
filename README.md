# ✏️WIP: AI on-the-edge

Learnings on containerizing (Azure) AI models and running them locally/hybrid.

Examples:

* Containerizing Azure Speech
* *note to self:* do I need some kind of POC or code that showcases how it works?

## Containerizing Azure Speech: lessons learned

*Lessons learned from ASD 09/01/2025.*

Original POC (on the cloud) used Azure Speech to do language detection + speech-to-text. This was quick to setup and worked well but we noticed some latency in the translations, which was not ideal for our use case.

So we tried [containerizing Azure Speech](https://learn.microsoft.com/en-us/azure/ai-services/speech-service/speech-container-howto) to see whether the latency would improve.

The Azure Speech container version is different than the cloud version. For starters, it requires two services instead of one to achieve the same POC:

* Azure Language (containerized) for language detection
* Azure Speech (containerized) for speech-to-text

Secondly, for input detection it recognized *only US english (en-US)*. Even after removing all autodetect functionality from our code it is unable to detect any other language.

The container is quite large: about 8+ GB. This means the deployment of the container takes quite some time. Depending on your use case, this could be a blocker.
That being said, there are some things you can try to decrease the size, such as selecting which languages it should detect up front (instead of loading all languages).

It is unclear how it works when multiple languages are being spoken in a conversation. Unable to test yet, as the input detection was en-US only.

In this example we did not use GSpeech for audio input.

### Documentation used

* [Install and run speech container](https://learn.microsoft.com/en-us/azure/ai-services/speech-service/speech-container-howto)
* [Speech-to-text container](https://learn.microsoft.com/en-us/azure/ai-services/speech-service/speech-container-stt?tabs=container&pivots=programming-language-python)
