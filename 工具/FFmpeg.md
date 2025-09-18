```sh
视频剪切
ffmpeg -i input.mp4 -ss 00:01:00 -to 00:02:30 -c copy output.mp4
-ss：开始时间，-to：结束时间
-c copy：直接复制流（不重新编码，速度极快）
```