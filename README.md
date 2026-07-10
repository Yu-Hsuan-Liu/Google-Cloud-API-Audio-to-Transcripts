# Google Cloud API: Audio to Transcripts

Transcribes long audio recordings (research interviews, lectures) with the Google Cloud Speech-to-Text API, using async long-running recognition with speaker diarization. The notebook walks through the whole pipeline: convert the MP3 to FLAC with ffmpeg, upload it to a Cloud Storage bucket, call `long_running_recognize`, and write the transcript to a text file with one speaker-labeled line per segment.

`Stay_Hungry_Stay_Foolish.mp3` and its `.txt` transcript are included as a working example.

You need a Google Cloud project with the Speech-to-Text API enabled, a storage bucket, and a service account key (the `.gitignore` keeps `*.json` keys out of the repo). Point `GOOGLE_APPLICATION_CREDENTIALS` at your key, set `storage_uri` to your `gs://` path, match `sample_rate_hertz` and the channel count to your audio, and run. The service is billed by audio duration, so check pricing before feeding it hours of recordings.
