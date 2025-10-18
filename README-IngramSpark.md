# Notes about using LaTeX to generate a pdf for IngramSpark

* [IngramSpark File Creation Guide]https://www.ingramspark.com/hubfs/downloads/file-creation-guide.pdf


# Images

Images must be 300 dpi.  It is best if the source images are quite large and are losslessly encoded (png or tiff).

One way to do this is to use `height=x.yin` to determine the proper size
```
\Image[scale=1,height=3.0in]{image.png}
```
Above, a 3" image was used, so the resulting 300 dpi image should be 900 pixels high.  The original image is `image.tiff`, so to resize:
```
magick image.tif -units PixelsPerInch -density 300x300 -resize x900 -depth 8  -colorspace Gray image.png
```


Then, update the tex to use:
```
\Image[scale=1,height=!]{image.png}
```
*FIXME: not sure if `height=!` is necessary.*


## Checking image size

In the non-free version of Adobe Acrobat:
1. Open the pdf file, all tools -> View More -> Use print production -> Preflight
2. Use "List page objects, grouped by type of object"


# Previewing images

IngramSpark seems to use inkjet to print the contents of most books.  In some cases, images may be much lighter than they appear on the screen or when printed using a laser printer.  Use the non-free full version of Adobe Acrobat to preview the file as follows

1. Open the pdf file, all tools -> View More -> Use print production -> Output Preview
2. Under Simulation Profile, select `U.S. Web Coated (SWOP) v2`

## Images are too light
* [Illustrations are too pale in the first proof of a book I am printing with a POD company](https://community.adobe.com/t5/indesign-discussions/illustrations-are-too-pale-in-the-first-proof-of-a-book-i-am-printing-with-a-pod-company/m-p/15276709/page/2)
* [CMYK vs RGB for IngramSpark and grayscale images](https://community.adobe.com/t5/indesign-discussions/cmyk-vs-rgb-for-ingramspark-and-grayscale-images/m-p/13041182
* [How To Fix Your Ingram Spark PDF – With Free PDF Fix Download](https://www.selfpublishingreview.com/2016/03/how-to-fix-your-ingram-spark-pdf-with-free-pdf-fix-download/)
** [Preflight Profile](https://www.selfpublishingreview.com/wp-content/uploads/2015/12/INGRAM-PRESET.kfp_1.zip)

Possible solutions

### Change the gamma
Changing the gamma can sometimes help. The gamma is usually 0.454545.  To check use ImageMagick's `identify`:

identify -verbose image.png | grep Gamma

For example
```
bash-3.2$ identify -verbose ../../Introduction/images/Introduction_BlackRockPoint1941_HenryLind_sc20019.png | grep Gamma
  Gamma: 0.454545
    icc:description: Grayscale - Gamma 2.2
bash-3.2%    
```

Gamma values to try are 0.35, 0.38 or values close to 0.4545.  ImageMagick can adjust the value with the `-gamma` flag:

```
magick image.tif -units PixelsPerInch -density 300x300 -resize x900 -depth 8  -colorspace Gray -gamma 0.35 image.png
```

For more information about Imagemagick's `-gamma` command line option, see [Imagemagick command line options](https://imagemagick.org/script/command-line-options.php#-gamma)

`-auto-gamma` might be worth a try.

### Adjust the black level

Adjusting the gamma does not work for all images.  Adjusting the black level might work:

```
magick image.tif -units PixelsPerInch -density 300x300 -resize x900 -depth 8  -colorspace Gray -level 70,100%  image.png
```

For more information about Imagemagick's `-level` command line option, see [Imagemagick command line options](https://imagemagick.org/script/command-line-options.php#-level)

`-auto-level` might be worth a try.

# Submitting to IngramSpark
See [How do I make my pdflatex .pdf compatible with Ingramspark (2018)](https://tex.stackexchange.com/questions/426706/how-do-i-make-my-pdflatex-pdf-compatible-with-ingramspark)

## too wide
Make sure that your text is not too wide by searching the log file for ``too wide``.  If a paragaph is too wide, then wrap it with
```
\begin{sloppypar}

blah blah blah

\end{sloppypar}
```

## PDF/X-1a
IngramSpark requires a PDF/X-1a file.  No free solution was found, the full version of Adobe Acrobat ($29/mo. in 2025) was required:

1. Open the pdf file, all tools -> View More -> Use print production -> Preflight
2. Under PDF/X, select  `Convert to PDF/X-1a (Coated FOGRA39) Selected this one`