# docker-compose setup for owncast

This is a minimal setup for a docker-hosted owncast instance, adding support for
HTTP authentication.

## Instructions

To run, configure owncast.yaml (including setting the streaming key) to your
liking and run:

```
docker-compose -f docker-compose.yml up owncast
```

## HTTP Authentication

To add HTTP authentication, first create a htpasswd file using the `htpasswd`
command:

```
htpasswd -b htpasswd USERNAME PASSWORD
```

Then uncomment the two lines starting with `auth_basic` in nginx.conf and the
line mounting the htpasswd file into the nginx instance in docker-compose.yml.

## Streaming video

To stream a video file to the owncast instance, you can use ffmpeg as follows:

```
fmpeg -i VIDEO.mp4 -c:v libx264 -preset veryfast -b:v 1984k -maxrate 1984k \
  -bufsize 3968k -vf "format=yuv420p,realtime" -g 60 -c:a aac -b:a 128k \
  -ar 44100 -af arealtime -f flv \
  rtmp://OWNCAST-HOST/live/STREAMING-KEY
```

To add subtitles, append `,subtitles=SUBTITLEFILE.srt` to the `-vf` string.
