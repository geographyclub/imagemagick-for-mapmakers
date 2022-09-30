# ImageMagick for Mapmakers

Use ImageMagick to create, edit, compose or convert digital images. This is a growing list of my most used ImageMagick commands that extend and enhance mapmaking.

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

Split image by percentage and save parts.  
```convert layer0.png -crop 25%x100% +repage layer0_part.png```

Append split images in reverse order and flip horizontally.  
```convert $(ls -r layer0_part-*.png) -flop +append layer0_append.png```

<img src="images/layer0_append.jpg"/>

Composite cloud cover image over Natural Earth image.  
```convert layer0.png layer1.png -gravity center -compose over -composite frame.png```

Composite and adjust levels of each image in one command.  
```convert -size 1920x1080 xc:none \( layer0.png -level 50%,100% \) -gravity center -compose over -composite \( layer1.png -level 50%,100% \) -gravity center -compose over -composite frame.png```

<img src="images/frame.jpg"/>

Add a sketch effect with a canny edge detection layer.  
```convert layer0.png \( +clone -modulate 200 -canny 0x1+10%+20% -negate \) -gravity center -compose multiply -composite layer0_canny.png```

<img src="images/layer0_canny.jpg"/>

Change the prime meridian 180*.  
```convert layer0.png -roll +960 layer0_roll.png```

<img src="images/layer0_roll.jpg"/>

Apply a 45* arc projection.  
```convert -size 1920x1080 xc:none \( layer0.png -virtual-pixel none -resize 50% -distort Arc '45' \) -gravity center -compose over -composite layer0_arc_45.png```

<img src="images/layer0_arc_45.jpg"/>

Apply a 45* arc projection rotated 180*.  
```convert -size 1920x1080 xc:none \( layer0.png -virtual-pixel none -resize 50% -rotate 180 -distort Arc '45 180' \) -gravity center -compose over -composite layer0_arc_45_180.png```

<img src="images/layer0_arc_45_180.jpg"/>

Apply a 360* arc projection.  
```convert -size 1920x1080 xc:none \( layer0.png -virtual-pixel none -resize 50% -distort Arc '360' \) -gravity center -compose over -composite layer0_arc_360.png```

<img src="images/layer0_arc_360.jpg"/>

Apply a donut projection.  
```convert -size 1920x1080 xc:none \( layer0.png -virtual-pixel none -resize 50% -distort Polar '0' \) -gravity center -compose over -composite layer0_polar_0.png```

<img src="images/layer0_polar_0.jpg"/>


Make a gif from a folder of files.  
```convert -delay 60 $PWD/*.png $(basename $PWD).gif```

