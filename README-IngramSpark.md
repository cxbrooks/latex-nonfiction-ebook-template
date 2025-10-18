# Notes about using LaTeX to generate a pdf for IngramSpark

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
*FIXME: not sure if `height=!` is necessary.

##Checking image size##

In the non-free version of Adobe Acrobat:
1. Open the pdf file, all tools -> View More -> Use print production
2. Use "List page objects, grouped by type of object"


# Previewing

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

1. Open the pdf file, all tools -> View More -> Use print production
2. Under PDF/X, select  `Convert to PDF/X-1a (Coated FOGRA39) Selected this one`