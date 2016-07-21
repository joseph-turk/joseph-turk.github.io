---
title: ImageMagick Guide Part 2&#58; Extracting Images from PDFs
layout: post
---

With ImageMagick and Ghostscript, you can extract each page from a PDF document as a separate image. This post is the part of a series that shows you how to use ImageMagick from the command line.

This guide was originally written to help non-technical members of the business development team at my company work with ImageMagick from the command line. The instructions in this guide are focused on installing and ImageMagick in a Windows environment and running it from the command line to process local images. However, these instructions can be easily adapted for running from a terminal to process local images in Linux or macOS as well.

This guide will not help you if you're looking to use ImageMagick to integrate image processing capabilities into an application you are developing.

You can find the other parts of this guide here:

- [ImageMagick Guide Part 1&#58; Setting Up ImageMagick](../../../07/14/setting-up-imagemagick)

## Extracting Images from a PDF
For this section, you'll need a PDF document to work with. If you want to follow along exactly with the document that I'm using, you can download the zip directory with sample files from Adobe at [this website](http://partners.adobe.com/public/developer/xml/topic_samples2.html).

1. Open a command prompt and navigate to the directory you have saved your sample PDF in. I saved my sample in a folder on my desktop called sample, so I used the command `cd .\Desktop\sample` to navigate to it.
![Command prompt screenshot](../../../../img/blog/processing-images-with-imagemagick/command-cd-desktop.png "Command prompt screenshot")
2. To verify that you're in the correct directory and that your sample PDF is saved in it, type `ls` and press **Enter**. You should see something that looks like the output below.
![Command prompt screenshot](../../../../img/blog/processing-images-with-imagemagick/command-ls-1.png "Command prompt screenshot")
**Note:** If you are using the standard Windows command prompt (cmd.exe) rather than PowerShell, you will need to type `dir` rather than `ls`.
3. To convert the PDF to PNG (one image per page), type `magick convert GrantApplication.pdf grant.png` and press **Enter**.
![Command prompt screenshot](../../../../img/blog/processing-images-with-imagemagick/command-convert-pdf-to-png-1.png "Command prompt screenshot")
In this command, `convert` tells ImageMagick which tool to use. Because we're working with just one PDF, we can use the Convert tool. `GrantApplication.pdf` tells Convert what file you want to convert, and `grant.png` tells it where to save the output. Because we gave our output file an extension of .png, Convert will automatically extract each page of the PDF as its own PNG image.
4. If you open your sample folder, you'll see that there are now three files in it: the original PDF, and two PNGs for the two pages of the PDF.
   ![Explorer window showing files](../../../../img/blog/processing-images-with-imagemagick/explorer-files.png "Explorer window showing files")  
5. Open one of the files.
   ![End result of first extraction](../../../../img/blog/processing-images-with-imagemagick/result-1.png "End result of first extraction")  
   You'll notice it doesn't look very good. The text is extremely pixelated, and it's pretty much illegible.

## Tweaking Your Extraction Settings
Fortunately, ImageMagick gives you lots of options for how to process images. Let's try extracting the pages from our sample PDF again with different settings.

1. Open your command prompt again and navigate back to your sample folder.
2. Type `magick convert -density 192 -quality 100 GrantApplication.pdf grant-fixed.png` and press **Enter**.
   ![Command prompt screenshot](../../../../img/blog/processing-images-with-imagemagick/command-convert-pdf-to-png-2.png "Command prompt screenshot")  
3. Open one of the new files.
  ![End result of second extraction](../../../../img/blog/processing-images-with-imagemagick/result-2.png "End result of second extraction")  
  Much better!

## Wrapping Up
In this post, we covered how to extract images from PDFs while maintaining legibility. There is, however, a tradeoff for higher quality extracts: file size. The PNGs we extracted with the first command had a total file size of about 85 KB, around half of the original PDF. The second, higher quality extraction, has a total file size of about 289 KB, more than 100 KB larger than the original PDF.

In the next post in this series, we'll look at ways to optimize images to strike a balance between file size and quality.
