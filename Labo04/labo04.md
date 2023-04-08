
# ADS Lab 04 - Scripting
### Authors: Pablo Urizar and Vincent Peer  
### Date: March 30, 2023  

---  




password for lab5 session: fBAW3g&jnH8&



## Task 2: Create thumbnails  

### Show dimensions script
>Write a script called show_dimensions that loops through all the picture files
and shows for each its name and its dimensions. For the loop use the for .. in
.. do .. done control structure.  

```  
#!/bin/bash
#
# Description : Print the name of each picture in the lab04_raw_files directory, followed by its dimensions. Picture extension should be .png or -jpg. Dimensions are width and height.
#
# Authors : Pablo Urizar, Vincent Peer
#
# Date : 06.04.2023


path=lab04_raw_files
cd $path

for file in *.png *.jpg
do
        echo -e $(identify -format '%f width: %w, height: %h' $file)
done
```  

### Rename pictures script
>Write a script called rename_pictures that produces picture files that have
the dimensions in their name. For example if a picture is called building.jpg
and has a width of 1024 and a height of 768 pixels the script should create a
file building_1024_768.jpg . The script should not modify the original files,
but create new ones.

```
#!/bin/bash
#
# Description : Renames each picture with png or jpg extention. The resulting name is the  name followed by the width and the height of this picture.
#
# Authors : Pablo Urizar, Vincent Peer
#
# Date : 06.04.2023



rm -rf renamed_pictures
mkdir renamed_pictures

cd lab04_raw_files

for picture in *.png *.jpg
do
        picture_ext=${picture##*.}
        name=${picture%%[.]*}
        dimension=$(identify -format '%w_%h' $picture)
        filename=${name}_${dimension}.${picture_ext}

        echo "new name : $filename"
        cp $picture ../renamed_pictures/${filename}
done
```

### Make thumbnails script
```
convert -geometry 300#!/bin/bash
#
# Description :
#
#Authors : Pablo Urizar, Vincent Peer
#
# Date : 06.04.2023


path=lab04_raw_files
cd $path

for file in *.png *.jpg
do
        ext=${file##*.}
        name=${file%%.*}
        if [ ${name##*_} = "thumb" ]
        then
                echo "$name is already a thumbnail"
        else
                convert -geometry 300 $file ${name}_thumb.${ext}
        fi
done
```


## Task 3: Generate HTML file

## Task 4: Use SSH Tunneling

