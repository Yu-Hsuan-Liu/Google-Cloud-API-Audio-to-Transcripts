# Google Cloud API: Audio to Transcripts

A worked example of transcribing long audio recordings (for instance, research interviews) with the Google Cloud Speech-to-Text API, using asynchronous long-running recognition with speaker diarization enabled. The repository includes a sample English speech recording and the transcript the notebook produced from it.

## What the code does

`Google_Longaudio_API_speaker_diarization.ipynb` walks through the full pipeline:

1. Converts the source MP3 to FLAC with ffmpeg, the lossless format the API accepts directly.
2. The FLAC file is then uploaded to a Google Cloud Storage bucket (done in the Cloud console; long-running recognition reads audio from a `gs://` URI rather than a local file).
3. Sets the `GOOGLE_APPLICATION_CREDENTIALS` environment variable to point at a service account key file.
4. Calls `long_running_recognize` from `google.cloud.speech_v1p1beta1` with a configuration that sets the sample rate, language code, audio channel count, word time offsets, and speaker diarization (speaker count adjustable).
5. Waits for the operation to finish, then writes each result segment to a text file, one line per segment, prefixed with a speaker label.

A small helper, `display_time()`, converts word end times in seconds into a readable `hours:minutes:seconds` form.

## Requirements

- Python 3
- ffmpeg on the system path
- Packages: `google-cloud-speech`, `google-cloud-storage`, `pydub`
- A Google Cloud project with the Speech-to-Text API enabled, a Cloud Storage bucket, and a service account key (JSON). Keep the key file out of version control; the included `.gitignore` excludes `*.json` for this reason.

## Usage

1. Convert your recording to FLAC and upload it to your Cloud Storage bucket.
2. In the notebook, point `GOOGLE_APPLICATION_CREDENTIALS` to your service account JSON file and set `storage_uri` to your `gs://bucket/file.flac` URI.
3. Adjust the configuration to match your audio: `sample_rate_hertz`, `audio_channel_count`, `diarization_speaker_count`, and the language code (`en-US`, `zh-TW`, and others).
4. Run `sample_long_running_recognize(storage_uri, filename, language)`. The transcript is printed and saved as `<filename>.txt`.

## Sample files

- `Stay_Hungry_Stay_Foolish.mp3`: sample English speech audio used as the demo input.
- `Stay_Hungry_Stay_Foolish.txt`: the transcript generated from that file by this notebook.

## Repository structure

```
Google_Longaudio_API_speaker_diarization.ipynb   Main notebook: conversion, API call, transcript output
Stay_Hungry_Stay_Foolish.mp3                     Sample input audio
Stay_Hungry_Stay_Foolish.txt                     Sample output transcript
README.md                                        This file
LICENSE                                          MIT license
```

## Notes

Speech-to-Text is a paid service billed by audio duration; check current Google Cloud pricing before transcribing long recordings. If the API rejects the audio, the error message usually states the expected sample rate or channel count, and the configuration should be edited to match.
