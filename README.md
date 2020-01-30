# ffmpeg-scripts

Collection of snippets using ffmpeg to create presnation and mix videos

# Add sound to video

    ffmpeg -i input.mp4 -i audio2.mp3 -to 00:02:30 -c:v copy -c:a aac -strict experimental output.mp4

# Video Rotation

https://stackoverflow.com/a/9570992/1279318

    ffmpeg -i input.mp4 -vf "transpose=1" output.mp4

* 0 = 90CounterCLockwise and Vertical Flip (default)
* 1 = 90Clockwise
* 2 = 90CounterClockwise
* 3 = 90Clockwise and Vertical Flip
* `-vf "transpose=2,transpose=2"` for 180 degrees

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
https://www.bugcodemaster.com/article/crop-video-using-ffmpeg

     ffmpeg -i input.mp4  -vf "crop=1024:1024" output.mp4
     
* `crop=w:h:x:y`
* `in_w` and `in_h` are image variable for width and height
     
## trim

    ffmpeg -i input.mp4  -ss HH:MM:SS.mmm -to HH:MM:SS.mmm :v copy -c:a copy output.mp4

# Create video from images

https://video.stackexchange.com/a/13069

    ffmpeg -framerate 30 -i image-%03d.png -c:v libx264 -pix_fmt yuv420p -crf 23 output.mp4
    
* `-framerate` is how many frames there are in a second
* `image-%03d.png` is files in assending order from image-000.png and up. you cannot skip a number.

## Randomize images (python)

    import glob, random
    files = glob.glob('source/*.jpg')
    random.shuffle(files)
    import shutil
    for idx, fn in enumerate(files):
        shutil.copy(fn, 'image-%03d.jpg' % (idx))
        

# Rotation

## See orientaion attribute

    exiftool -Orientation -S source/

## Remove orientaion attribute

    exiftool -Orientation= source/

## Rotate image

https://github.com/xiumingzhang/cheatsheets/blob/master/ffmpeg-imagemagick.md#rotate-images-in-a-folder

    convert "$file" -rotate 90 "${file%.png}"_rotated.png

# Image manipulation

# Resize image
https://www.howtogeek.com/109369/how-to-quickly-resize-convert-modify-images-from-the-linux-terminal/
    
    convert example.png -resize 200x100 example.png

## Add background to resize images
https://unix.stackexchange.com/a/122592/279393

    mogrify -gravity Center -extent 1024x1024 -background white -colorspace RGB *jpg
    
## PDF to jpg

Include adding white background instead of transparent background

    convert -background white -flatten -colorspace sRGB -density 300 input.pdf output.jpg
