---
title: ImageMagick Guide Part 1&#58; Setting Up ImageMagick
layout: post
---

ImageMagick is a powerful tool for processing images. This post is the part of a series that shows you how to use ImageMagick from the command line.

This guide was originally written to help non-technical members of the business development team at my company work with ImageMagick from the command line. The instructions in this guide are focused on installing and ImageMagick in a Windows environment and running it from the command line to process local images. However, these instructions can be easily adapted for running from a terminal to process local images in Linux or macOS as well.

This guide will not help you if you're looking to use ImageMagick to integrate image processing capabilities into an application you are developing.

You can find the other parts of this guide here:

- [ImageMagick Guide Part 2 - Extracting Images from PDFs](../../../07/13/extracting-images-from-pdfs)

## Downloading and Installing ImageMagick
1. Go to the [ImageMagick downloads page](http://www.imagemagick.org/script/binary-releases.php#windows) and click the appropriate download link for your operating system. Assuming you're running a 64-bit version of Windows, the first two download links should both work for you.
![ImageMagick download page](../../../../img/blog/processing-images-with-imagemagick/download-page.png "ImageMagick download page")
2. When it finishes downloading, click **Run**.
3. When your computer prompts you whether to allow the installer to make changes on your computer, click **Yes**.
4. Follow the installation prompts accepting most of the defaults. When you get to the *Select Additional Tasks* screen, make sure you select the **Install legacy utilities (e.g. convert)** option.
![Select additional tasks screen](../../../../img/blog/processing-images-with-imagemagick/select-additional-tasks.png "Select additional tasks screen")
5. At the end of the installation process, you will receive a prompt to verify that ImageMagick was installed correctly. Follow the steps in the next section to perform this verification.

## Testing Your Installation
In this section, the screenshots you see will not be of the default Windows command prompt. I highly recommend Cmder as a console emulator for Windows. For information about how to install and set up Cmder, click here.

1. Open a command prompt.
2. Type the following command and press **Enter**.
```
magick wizard: wizard.jpg
```
![Command prompt screenshot](../../../../img/blog/processing-images-with-imagemagick/command-magick-wizard.png "Command prompt screenshot")
3. Type the following command and press **Enter**.
```
magick wizard.jpg win:
```
![Command prompt screenshot](../../../../img/blog/processing-images-with-imagemagick/command-wizard-win.png "Command prompt screenshot")
4. The wizard image opens in a new window. This confirms that you have installed ImageMagick correctly.
![Wizard window](../../../../img/blog/processing-images-with-imagemagick/wizard-window.png "Wizard window")

## Setting Up ImageMagick to Work with PDFs
ImageMagick requires a third-party tool called Ghostscript to process vector graphics such as PDFs.

1. Go to the [Ghostscript download page](http://www.ghostscript.com/download/).
2. Click the download link under *Postscript and PDF interpreter/renderer*.
![Ghostscript download page](../../../../img/blog/processing-images-with-imagemagick/ghostscript-download-page.png "Ghostscript download page")
3. On the *Ghostscript Downloads* page, select the appropriate link for your platform and usage of Ghostscript. Again, assuming you are using a 64-bit version of Windows, click one of the following links.
 ![Ghostscript download links](../../../../img/blog/processing-images-with-imagemagick/ghostscript-download-links.png "Ghostscript download links")
 4. After the download finishes, run the installer and accept all defaults.
