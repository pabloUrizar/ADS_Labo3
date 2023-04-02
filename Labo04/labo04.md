
# ADS Lab 04 - Scripting  
## author: Pablo Urizar, Vincent Peer  
## date: March 30, 2023  


password for lab5: fBAW3g&jnH8&



## Task 2: Create thumbnails  
### Image dimensions  
With the following command:
> identify -format 'width: %w, height: %h, size: %B bytes\n' image.jpg  

We obtain these results for cours.jpg, mur.jpg and yparc.png:
```
width: 2500, height: 1667, size: 286786B
width: 2501, height: 1667, size: 4076944 bytes
width: 1200, height: 800, size: 918440 bytes
```  

### Show dimensions script
```  
#!/bin/bash

path=lab04_raw_files
cd $path

for file in *.png *.jpg
do
        echo -e $(identify -format '%f width: %w, height: %h' $file)
done
```  

### Rename pictures script
```
#!/bin/bash


rm -rf renamed_pictures
mkdir renamed_pictures

cd lab04_raw_files

for picture in *.png *.jpg
do
        picture_ext=${picture##*.}
        basename=${picture%%[.]*}
        dimension=$(identify -format '%w_%h' $picture)
        filename=${basename}_${dimension}.${picture_ext}

        echo "new name : $filename"
        cp $picture ../renamed_pictures/${filename}
done
```

## Task 3: Generate HTML file

## Task 4: Use SSH Tunneling

