# ImageMagick for Mapmakers

This is an ongoing project to compile the choicest ImageMagick commands I've used over the years to extend and enhance my mapmaking.

<img src="images/newyork.jpg"/>

## The Magick

Adjust levels to enhance colors of Natural Earth's hypsometric image. This will be our example image.  
```convert HYP_HR_SR_OB_DR.png -level 50%,100% layer0.png```

<img src="images/HYP_HR_SR_OB_DR.jpg"/>
<img src="images/layer0.jpg"/>

Resize all png files by 25% and save as jpg. These will be our example thumbnails.  
```ls *.png | while read file; do convert -resize 25% ${file} ${file%.*}.jpg; done```

Crop image.  
```convert layer0.png -gravity center -crop 960x540+0+0 +repage layer0_crop.png```

Crop image by percentage and save parts.  
```convert layer0.png -crop 100%x50% +repage layer0_part.png```

Append images in reverse order.  
```convert $(ls -r layer0_part-*.png) -append layer0_append.png```

<img src="images/layer0_appended.jpg"/>

Composite cloud cover image over Natural Earth image.  
```convert layer0.png layer1.png -gravity center -compose over -composite frame.png```

Composite and adjust levels of each image in one command.  
```convert -size 1920x1080 xc:none \( layer0.png -level 50%,100% \) -gravity center -compose over -composite \( layer1.png -level 50%,100% \) -gravity center -compose over -composite frame.png```

<img src="images/frame.jpg"/>

Add a sketch effect with a canny edge detection layer.  
```convert layer0.png \( +clone -modulate 200 -canny 0x1+10%+20% -negate \) -compose multiply -composite layer0_canny.png```

<img src="images/layer0_canny.jpg"/>

Change the prime meridian 180*.  
```convert layer0.png -roll +960 layer0_roll.png```

<img src="images/layer0_roll.jpg"/>

Make a gif from a folder of files.  
```convert -delay 60 $PWD/*.png $(basename $PWD).gif```

