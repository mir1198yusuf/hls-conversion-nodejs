# HLS video conversion using FFMPEG in node.js

- I created this to convert existing s3 mp4 file to HLS-compatible video segments
- This script will download the s3 mp4 file locally, convert it to 3 resolutions i.e. 320x180, 854x480 and 1280x720, then create a master m3u8 playlist file & upload all playlists and their segments back to s3. It does not delete original s3 mp4 file.
- It will also cleanup all local files generated at end of script.

### How to use ?

- Download `ffmpeg` on your machine as per your OS.
- Upload a mp4 file to s3 in a directory `mp4`
- Clone this repo and do `npm install`
- `cp .env.sample .env`. Populate the s3 credentials.
- Do `mkdir hls` in repo root.
- Create `hls` directory in s3 on same level as `mp4` directory.
- In `app.js`, update `const mp4FileName = 'video1.mp4';` with your video file name. 
- In `app.js`, update `ffmpeg.setFfmpegPath('/opt/local/bin/ffmpeg');` with your ffmpeg installation path.
- Run `node app.js` to start the script.

### Points to understand

- This has been tested on Mac, but should work on linux as well.
- Currently script is not much flexible in allowing other resolutions, changing folder names, etc. Later if I get time, I will modify the script to allow passing these values from `.env` so modifying the script `app.js` will not be needed.

### How to test ?

- I have included a small html file which uses video.js as client library to play hls-compatible videos. Html file loads external scripts for video.js so make sure to verify all external scripts loaded and proceed at your own risk. Create your own html file if you want to load from other sources or use a different video library.
- After the main script `app.js` is run, find in s3 the file ending with `master.m3u8`. Get the url of that file from s3 or cdn if you have cdn infront of your s3 bucket.
- Replace the url as `master_m3u8_file_url` in `index.html` file.
- Open the `index.html` file in your browser. Better to use incognito to test the adaptive bitrate algorithm as sometimes browsers cache resources and actual behaviour of algorithm does not get discovered properly.

### Runtime values

- As per my testing on mac, for an mp4 video of 900 MB, entire script completed in 26 minutes with no high-ram usage detected. The total size of all generated file of all 3 resolutions was around 580 MB.

_Feel free to open an issue in case you find a bug_