---
author: eric-urban
ms.service: cognitive-services
ms.topic: include
ms.date: 06/08/2022
ms.author: eur
---

[!INCLUDE [Header](../../common/python.md)]

[!INCLUDE [Introduction](intro.md)]

## Prerequisites

[!INCLUDE [Prerequisites](../../common/azure-prerequisites.md)]

> [!div class="nextstepaction"]
> <a href="https://microsoft.qualtrics.com/jfe/form/SV_0Cl5zkG3CnDjq6O?PLanguage=PYTHON&Pillar=Speech&Product=Speech-translation&Page=quickstart&Section=Prerequisites" target="_target">I ran into an issue</a>

## Set up the environment

The Speech SDK for Python is available as a [Python Package Index (PyPI) module](https://pypi.org/project/azure-cognitiveservices-speech/). The Speech SDK for Python is compatible with Windows, Linux, and macOS. 
- You must install the [Microsoft Visual C++ Redistributable for Visual Studio 2015, 2017, 2019, or 2022](/cpp/windows/latest-supported-vc-redist?view=msvc-170&preserve-view=true) for your platform. Installing this package for the first time might require a restart.
- On Linux, you must use the x64 target architecture.

Install a version of [Python from 3.7 to 3.10](https://www.python.org/downloads/). First check the [SDK installation guide](../../../quickstarts/setup-platform.md?pivots=programming-language-python) for any more requirements 

> [!div class="nextstepaction"]
> <a href="https://microsoft.qualtrics.com/jfe/form/SV_0Cl5zkG3CnDjq6O?PLanguage=PYTHON&Pillar=Speech&Product=Speech-translation&Page=quickstart&Section=Set-up-the-environment" target="_target">I ran into an issue</a>

## Translate speech from a microphone

Follow these steps to create a new console application.

1. Open a command prompt where you want the new project, and create a new file named `speech-recognition.py`.
1. Run this command to install the Speech SDK:  
    ```console
    pip install azure-cognitiveservices-speech
    ```
1. Copy the following code into `speech_translation.py`: 

    ```Python
    import azure.cognitiveservices.speech as speechsdk
    
    def recognize_from_microphone():
        speech_translation_config = speechsdk.translation.SpeechTranslationConfig(subscription="YourSubscriptionKey", region="YourServiceRegion")
        speech_translation_config.speech_recognition_language="en-US"
    
        target_language="it"
        speech_translation_config.add_target_language(target_language)
    
        audio_config = speechsdk.audio.AudioConfig(use_default_microphone=True)
        translation_recognizer = speechsdk.translation.TranslationRecognizer(translation_config=speech_translation_config, audio_config=audio_config)
    
        print("Speak into your microphone.")
        translation_recognition_result = translation_recognizer.recognize_once_async().get()
    
        if translation_recognition_result.reason == speechsdk.ResultReason.TranslatedSpeech:
            print("Recognized: {}".format(translation_recognition_result.text))
            print("""Translated into '{}': {}""".format(
                target_language, 
                translation_recognition_result.translations[target_language]))
        elif translation_recognition_result.reason == speechsdk.ResultReason.NoMatch:
            print("No speech could be recognized: {}".format(translation_recognition_result.no_match_details))
        elif translation_recognition_result.reason == speechsdk.ResultReason.Canceled:
            cancellation_details = translation_recognition_result.cancellation_details
            print("Speech Recognition canceled: {}".format(cancellation_details.reason))
            if cancellation_details.reason == speechsdk.CancellationReason.Error:
                print("Error details: {}".format(cancellation_details.error_details))
                print("Did you set the speech resource key and region values?")
    
    recognize_from_microphone()
    ```

1. In `speech_translation.py`, replace `YourSubscriptionKey` with your Speech resource key, and replace `YourServiceRegion` with your Speech resource region.
    > [!IMPORTANT]
    > Remember to remove the key from your code when you're done, and never post it publicly. For production, use a secure way of storing and accessing your credentials like [Azure Key Vault](../../../../../key-vault/general/overview.md). See the Cognitive Services [security](../../../../cognitive-services-security.md) article for more information.
1. To change the speech recognition language, replace `en-US` with another [supported language](~/articles/cognitive-services/speech-service/supported-languages.md#speech-to-text). Specify the full locale with a dash (`-`) separator. For example, `es-ES` for Spanish (Spain). The default language is `en-US` if you don't specify a language. For details about how to identify one of multiple languages that might be spoken, see [language identification](~/articles/cognitive-services/speech-service/language-identification.md).
1. To change the translation target language, replace `it` with another [supported language](~/articles/cognitive-services/speech-service/supported-languages.md#speech-translation). With few exceptions you only specify the language code that precedes the locale dash (`-`) separator. For example, use `es` for Spanish (Spain) instead of `es-ES`. The default language is `en` if you don't specify a language.

Run your new console application to start speech recognition from a microphone:

```console
python speech_translation.py
```

Speak into your microphone when prompted. What you speak should be output as translated text in the target language: 

```console
Speak into your microphone.
Recognized: I'm excited to try speech translation.
Translated into 'it': Sono entusiasta di provare la traduzione vocale.
```

> [!div class="nextstepaction"]
> <a href="https://microsoft.qualtrics.com/jfe/form/SV_0Cl5zkG3CnDjq6O?PLanguage=PYTHON&Pillar=Speech&Product=Speech-translation&Page=quickstart&Section=Translate-speech-from-a-microphone" target="_target">I ran into an issue</a>

## Remarks
Now that you've completed the quickstart, here are some additional considerations:

- This example uses the `recognize_once_async` operation to transcribe utterances of up to 30 seconds, or until silence is detected. For information about continuous recognition for longer audio, including multi-lingual conversations, see [How to translate speech](~/articles/cognitive-services/speech-service/how-to-translate-speech.md).
- To recognize speech from an audio file, use `filename` instead of `use_default_microphone`:
    ```python
    audio_config = speechsdk.audio.AudioConfig(filename="YourAudioFile.wav")
    ```
- For compressed audio files such as MP4, install GStreamer and use `PullAudioInputStream` or `PushAudioInputStream`. For more information, see [How to use compressed input audio](~/articles/cognitive-services/speech-service/how-to-use-codec-compressed-audio-input-streams.md).

## Clean up resources

[!INCLUDE [Delete resource](../../common/delete-resource.md)]
