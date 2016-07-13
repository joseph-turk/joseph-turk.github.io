---
title: ImageMagick Guide Part 3&#58; Optimizing Images
layout: post
---

ImageMagick is a powerful tool for processing images. This post is the part of a series that shows you how to use ImageMagick from the command line.

This guide was originally written to help non-technical members of the business development team at my company work with ImageMagick from the command line. The instructions in this guide are focused on installing and ImageMagick in a Windows environment and running it from the command line to process local images. However, these instructions can be easily adapted for running from a terminal to process local images in Linux or macOS as well.

This guide will not help you if you're looking to use ImageMagick to integrate image processing capabilities into an application you are developing.

You can find the other parts of this guide here:

- [ImageMagick Guide Part 1 - Setting Up ImageMagick](../../../07/13/setting-up-imagemagick)
- [ImageMagick Guide Part 2 - Extracting Images from PDFs](../../../07/13/extracting-images-from-pdfs)

## Processing Images
In spite of their often interchangeable use online, different image formats in fact excel in different areas. This website provides a good introductory overview of which image format to use for which purpose.

There are two commands for ImageMagick that we will use to convert images from one format to another: `convert` and `mogrify`. These two commands are similar, with the primary difference being that `mogrify` can be used for batch processing (editing multiple images at one time).

We will use these two commands to perform two common processing actions for images: converting PNG files to JPG (and vice versa) and resizing images.

### Changing PNG Images to JPG (and Vice Versa)
You can change image formats with `convert` using the following format:
```
magick convert [original-image].[extension] [processed-image].[extension]
```



#### Changing Image Formats Using Convert

#### Changing Image Formats Using Mogrify

### Resizing Images

#### Resizing Images Using Convert

#### Resizing Images Using Mogrify

### Additional Processing Options
There are many more actions you can perform with ImageMagick, including cropping, color correction, adjusting compression level, and rotating.


## How to Learn More
