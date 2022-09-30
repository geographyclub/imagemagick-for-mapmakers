# ImageMagick for Mapmakers

This is a work in progress compiling my most used ImageMagick commands that extend and enhance the mapmaking process.

<img src="images/newyork.jpg"/>

Use `gdalwarp` to convert geotiff to regular tiff for use with imagemagick.
 
```gdalwarp -overwrite -dstalpha --config GDAL_PAM_ENABLED NO -co PROFILE=BASELINE layer_geo.tif layer_regular.tif```

Crop image.

```convert largeprint_kdp-page077.png -gravity center -crop 1940x600+0+0 +repage largeprint_kdp-page077_1940x600.png```

Crop image while maintaining aspect ratio with `geometry` option.

```convert largeprint_kdp-page077.png -gravity center -geometry 1940x600^ -crop 1940x600+0+0 +repage largeprint_kdp-page077_1940x600.png```

Resize all png files and save as jpg.

```ls *.png | while read file; do convert -resize 25% ${file} ${file%.*}.jpg; done```

Adjust levels to enhance colors of Natural Earth's hypsometric image.

```convert layer0.png -level 50%,100% layer0_levels.png```

<img src="images/layer0.jpg"/>
<img src="images/layer0_levels.jpg"/>

Composite cloud cover image over Natural Earth image.

```convert layer0.png layer1.png -gravity center -compose over -composite frame.png```

Composite and adjust levels of each image in one command.

```convert -size 1920x1080 xc:none \( layer0.png -level 50%,100% \) -gravity center -compose over -composite \( layer1.png -level 50%,100% \) -gravity center -compose over -composite frame.png```

<img src="images/frame.jpg"/>

Add a sketch effect with a canny edge detection layer.

```convert layer0.png -level 50%,100% \( +clone -modulate 200 -canny 0x1+10%+20% -negate \) -compose multiply -composite layer0_canny.png```

<img src="images/layer0_canny.jpg"/>

Make a gif from a folder of files.

```convert -delay 60 $PWD/*.png $(basename $PWD).gif```

TO DO: more effects and animation
