# ffmpeg
Tips for converting and ripping

#### Copy audio, video + subtitles:
```
ffmpeg \
-i video.mkv \
-sub_charenc CP1250 \
-i sub.srt \
-map 0:0 \
-map 0:1 \
-map 1:0 \
-c:a copy -async 1 \
-c:v copy \
-c:s srt -metadata:s:s:0 language=ces \
-y output.mkv
```


#### VOB:
```
cat VTS_01_1.VOB VTS_01_2.VOB VTS_01_3.VOB VTS_01_4.VOB VTS_01_5.VOB | ffmpeg -i - \
-c:v libx264 \
-map 0:1 \
-b:v 2100k \
-minrate 2100k \
-maxrate 6100k \
-bufsize 1835k \
-c:a copy -map 0:4 -metadata:s:a:0 language=eng \
-c:s copy -map 0:6 -metadata:s:s:0 language=czech \
output.mp4
```

#### Deinterlace:
```
-vf "yadif=0:-1:0"
```

### TS stream h264 native:
```
ffmpeg -i input.ts \
-c:v copy \
-c:a copy \
-map 0:0 \
-map 0:1 -metadata:s:a:0 language=eng \
output.mp4
```

#### example 2:
```
ffmpeg -i inputs.ts \
map 0:0 -c:v copy \
-map 0:1 -metadata:s:a:0 language=cze \
-c:a copy
output.mp4
```
### TS stream h264 native 1080p scale to 720p
```
ffmpeg -i input.ts \
-c:v h264 \
-b:v 2100k \
-minrate 2100k \
-maxrate 6100k \
-bufsize 1835k \
-vf scale=-1:720 \
-c:a copy \
-map 0:0 \
-map 0:1 	 \
output.mp4
```

### TS stream crf 20
```
ffmpeg -i input.ts \
-c:v libx264 \
-preset slow \
-crf 21 \
-tune animation \
-profile:v auto -level 4.2 \
-vf scale=-1:720 \
-c:a copy \
-map 0:0 -map 0:1 \
output.mp4 
```
