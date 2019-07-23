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

# Concat Videos

https://stackoverflow.com/a/11175851/1279318

    rm list.txt
    for file_name in *.mp3; do
        echo "file '$file_name'" >> list.txt
    done
    ffmpeg -f concat -i list.txt output.mp4
    
# Video grid

https://stackoverflow.com/a/33764934/1279318

## 2 X 2 grid
    ffmpeg -i input0.mp4 -i input1.mp4 -i input2.mp4 -i input3.mp4 -filter_complex \
    "[0:v][1:v]hstack=inputs=2[top];[2:v][3:v]hstack=inputs=2[bottom];
    [top][bottom]vstack=inputs=2[v]" -map "[v]" output.mp4
    
## 3 X 3 Grid
    ffmpeg -i input0.mp4 -i input1.mp4 <...> -i input9.mp4 -to 00:00:55 -filter_complex \
    "[0:v]scale=640:640[v0];[1:v]scale=640:640[v1];[2:v]scale=640:640[v2];[v0][v1][v2]hstack=inputs=3[top];
    [3:v]scale=640:640[v3];[4:v]scale=640:640[v4];[5:v]scale=640:640[v5];[v3][v4][v5]hstack=inputs=3[middle];
    [6:v]scale=640:640[v6];[7:v]scale=640:640[v7];[8:v]scale=640:640[v8];[v6][v7][v8]hstack=inputs=3[bottom];
    [top][middle][bottom]vstack=inputs=3[v]" -map "[v]" output.mp4
    
# Resize vidoe 

## crop

     ffmpeg -i input.mp4  -vf "scale=(iw*sar)*max(864/(iw*sar)\,864/ih):ih*max(864/(iw*sar)\,864/ih), crop=864:864" output.mp4
     
## trim

    ffmpeg -i input.mp4  -ss HH:MM:SS.mmm -to HH:MM:SS.mmm output.mp4
