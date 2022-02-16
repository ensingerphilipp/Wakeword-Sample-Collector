
**Wakeword-Sample-Collector** 

## Using Wakeword-Sample-Collector to train a custom wakeword for Project-Friday

<p align="center">
  <img src="/resources/Wakeword-Sample-Collector.jpg" width="500" title="Wakeword-Sample-Collector-In-Action">
</p>

## Overview

Procedure:

1. Agree to use the data and use the microphone.
2. Press the record button
3. Record the words displayed for ~ 1.5 Seconds before the recording starts
4. When the button turns RED the recording has started. --> Speak the givven word
5. When the button turns grey again the recording has stopped 
6. Review the words and then upload them via the upload button.

## Installation instructions

1. Create a GCloud Console account
2. Install GCloud instance according to Google instructions & add account --> see https://cloud.google.com/sdk/docs/quickstart
3. Download Wakeword-Collector from this Git Repo
4. Add desired words and Filler-words in \static\scripts\app.js  
5. Customise texts in \static\templates
6. Create a project in the GCloud Console (browser website) and create a storage bucket with the desired properties
7. Select the storage bucket under "Cloud Storage" and select "Interoperability" to create an access key for the user account.
8. In app.yaml, specify the storage bucket name and secret of the created key  
9. Open terminal in folder and execute "gcloud app deploy"

## Prepare recorded Samples for Wakeword-Training

The recorded samples are in the ogg container with various internal audio
internal audio formats -\> therefore, before using them for the training
all samples must be converted to WAV 16000hz Mono.

  - Open terminal and download all samples from gem GCloud Bucket by using the command 

<!-- end list -->

    gsutil -m cp gs://samplecollection_project_friday/* ./

"samplecollection\_project\_friday" is an example name of the bucket used.

  - Then ffmpeg is used in the CMD to convert all files.
    Command for Windows-based machines:

<!-- end list -->

    FOR /F "tokens=*" %G IN ('dir /b *.ogg') DO ffmpeg -i "%G" -acodec pcm_s16le -ac 1 -ar 16000 ".\wav_16khz_mono\%~G.wav"

  - Unfortunately, it turned out that the accuracy of the recordinglength of the samples is different across browsers and some samples are 
    960ms or 1020ms long instead of 1000ms. Therefore all samples have to be stretched or squashed to 1000ms.

The standalone tool "Rubberband" in the CMD is used for this:

    FOR /F "tokens=*" %G IN ('dir /b *.wav') DO ..\rubberband-2.0.2-gpl-executable-windows\rubberband.exe -D 1 "%G" "..\wav_16khz_mono_stretched\%~G.wav"


## General Running
Please follow the directions provided by Pete Warden located here (https://github.com/petewarden/open-speech-recording/edit/master/README.md)

## Credits

**Wakeword-Sample-Collector** is small web app to collect speech samples and upload them to Google cloud storage.  

This is a small Flask app that runs on Google App Engine. It serves up a client-side Javascript app that prompts for words to be uttered, recorded, and posted to a Google cloud storage bucket.  This app uses the microphone and require HTML5 support on the client device.  

For use in collecting speech recordings from friends and family to detect the keyphrase 'Friday*', I modified Pete Wardens original work in following ways:

1. Modified text/message in all templates (German) (original: https://github.com/petewarden/open-speech-recording/blob/master/templates)
2. Modified var wantedWords and var fillerWords in app.js (original: https://github.com/petewarden/open-speech-recording/blob/master/static/scripts/app.js)
3. Modified waiting Times and logic of Recording

Thanks to Pete Warden for the Open Speech Recording application (https://github.com/petewarden/open-speech-recording)

Pete Warden also credited:
'Thanks to the Mozilla team for the Web Dictaphone sample application that I used as a starting point, Sole for the oscilloscope, and the Flask team for a lovely Python microframework!'
