# ffmpeg-scripts

Collection of snippets using ffmpeg to create presnation and mix videos

# Video Rotation

https://stackoverflow.com/a/9570992/1279318

    ffmpeg -i input.mp3 -vf "transpose=1" output.mp4

* 0 = 90CounterCLockwise and Vertical Flip (default)
* 1 = 90Clockwise
* 2 = 90CounterClockwise
* 3 = 90Clockwise and Vertical Flip
* -vf "transpose=2,transpose=2" for 180 degrees
