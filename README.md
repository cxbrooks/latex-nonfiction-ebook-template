Latex Technical Nonfiction Book
===============================

This is a template for creating the various formats of a technical non-fiction
book using content written in LaTeX. Modern books need to be formatted for
printing, as PDFs, as websites, and as eBooks (for Apple iBooks, KOBO, Amazon
Kindle and other electronic publishers). This template provides a model for
creating all of these versions, using the same LaTeX source files. You will 
need to annotate your LaTeX files using commands supplied in the `zzInit.tex`,
commands file in order to build all your book formats successfully.

Note that these tools are specifically for use by independent self-publishers
or small publishers, and not for authors who are using one of the
[big four publishers](https://en.wikipedia.org/wiki/Big_Four_(book_publishing)).
Larger publishers have their own requirements for book formatting that are 
entirely different than these.

[This is the print version from book.tex as a PDF](book.pdf)

[This is the eBook version from eBook.tex as PDF](eBook.pdf)

This template is for a fully publishable book, rather than a toy example, so the it contains:

1. **Front matter**
  * A Half Title page (just the book title)
  * A Frontispiece/Epigraph - image and/or quote
  * A title page with an image
  * A copyright and publication data page
  * A summary table of contents
  * A detailed table of contents
  * A List of Figures
  * An optional List of Tables
  * A Preface
2. **Main matter**
  * An introductory chapter
  * As many body chapters as you have
3. **Back matter**
  * Notes grouped by chapter
  * Bibliography
  * A List of Authors cited
  * A permuted index
  * A configurable list of abbreviations and terms used

Many of these sections are optional, and there are variations to the formatting when eBooks are generated.

To use this project you'll need to have some familiarity with LaTeX. To be really successful, you'll need to become comfortable editing and creating new commands, but this should give you a big head start.

## Structure

`BookParameters.tex` is the LaTeX file containing defines about your book. 
You edit this while constructing your book. All LaTeX root files for the same
publication include this file.

`book.tex` is the LaTeX root file for the Print on Demand
([CreateSpace](https://www.createspace.com) or 
[Ingram Spark](http://www.ingramspark.com/)), distribution PDF, and 
ASCII plain text draft formats for your book.

The `book.tex` will used to generate a greyscale version for printing and
a color version for a distribution PDF (A PDF that looks like the print
version, but has live hypelinks and images in color).

`eBook.tex` is the LaTeX root file for hyperlinked formats (EPUB (for KOBO
and iBooks, MOBI for Amazon's Kindle (KDP), and html websites).

The `./tex` sub-directory contains all of your book chapters, as will as
the initialization macros (./tex/zzInit.tex), your abbreviations and special
terms definitions (./tex/zzAbbr.tex), your title page ((./tex/zzTitlepage.tex),
and your copyright page (./tex/zzCopyright.tex).

The `./images` sub-directory contains all images and figures used in 
the book. You should create **_BW** greyscale versions for the print
formated version or you may get a nasty surprise from your printer.

`eBook.cfg` is used to add metadata to the `OEBPS/Content.opf` section of your eBooks. You'll need to manually edit this as described in the eBook format section below.


## Building

You can run the provided script:

```
    ./bin/makebook --all
```

This will build all versions of the book to subdirectories. You can build
individual variants using options described in the utilities section. You
should run this daily while writing to avoid having subtle problems in your
text that creates errors in some formats and not others. Its much easier to
debug a document incrementally than to try to do it after you have many
conflicting issues in it.

While working on a book, I usually just build from within TeXstudio, but
since there are a number of steps, its easy to leave one out and get an
improperly formatted book (incomplete or incorrext index, missing bibliographic
references, bad table of contents, etc.)

## Paperback Book Cover

Createspace allows you to create a quickie cover using their on-line tool, 
but these are not terribly professional looking and cannot be used with
other printers. 
I use my 
[Github latex-createspace-bookcover](https://github.com/jfogarty/latex-createspace-bookcover)
project. The best way to use this is to clone that project and merge its
`cover`, `bin`, and `images` directories into this one.

When you build the book PDF with `./bin/makebook`, it runs `setBookTotalPageCount`
against the LaTeX log to set the page count in `BookParameters.tex` for
the proper spine width of your book's cover.

## BookParameters.tex Values

You will need to manually edit these values, but rarely.

- TotalPageCount: This is the number of pages in the printed form of the book. It is essential for calculating the thickness of the spine in the cover. **makebook** computes this from the PdfLaTeX log whenever that format is built and changes it in this file.
- PaperWidthPt: {6in} The 6" by 9" book format is the most common one used for high quality non-fiction paperbacks. This is the default used by CreateSpace but you can change it as needed.
- PaperHeightPt: {9in}
- TheMainTitle: {The Ultimate Book}
- TheSubTitle: {Hey! Why Not Read This One?}
- TheBookSeries: {Books That Are Really Odd Series}
- TheAuthor: {Wilson J. Schmeggles}
- TheAuthorLNF: {Schmeggles, Wilson, J.}
- TheCopyrightYear: {2017}
- TheEdition: {First Edition}
- ThePublisherName: {Your Press} Just the short form name
- ThePublisherLineA: {Your} Just one part of the name
- ThePublisherLineB: {Press} Just one part of the name
- ThePublisher{\ThePublisherName}  Add Ltd, Inc, LLC etc here
- ThePublisherAddrA: {1234 Xyzzy Plugh Ln}
- ThePublisherAddrB: {North Wyodaka, QM 45678}
- ThePublisherCity: {Wyodaka}
- ThePublisherState: {QM}
- ThePrinter: {CreateSpace, An Amazon.com Company}
- TheSubjectArea: {Yada, Yada, Yada}
- TheKeywords: {Yadas, Books, Odd, Wierd}
- PrintISBN: The 13 digit ISBN. Get from Bowker. 
- PrintISBNShort: The 10 digit ISBN. Get from Bowker. 
- EbookISBN: The ebook ISBN, if you need one.
- EbookISBNShort: The short version of the above.
- TheCIPSubjectHeadings: Library of Congress Catalog Subject Headings. Get from QualityBooks
- TheLCCN: Library of Congress classification number (also Control Number)
- TheDDSN: Dewey Decimal System classification number
- TheLCPCN: Library of Congress’ Preassigned Control Number (PCN)
- TheCopyrightKeywords: Obtain from QualityBooks
- ShortDescription: Normally restricted to 300 t0 350 characters depending on publisher.
- BackDescription: Typically max 2000-4000 characters
- AuthorBio : A short description of the author for the back cover

## Important Command Macros

The various versions of your book are controlled by **\Boolean**
parameters specified in the root of `book.tex` or `eBook.tex`. When
`makebook` creates a subdirectory for each version, it also updates
some of these Boolean parameters so that the book is typeset correctly.

You need to assist this by using command macros that adapt to the version.
These are located in `zzInit.tex`, and you should become familiar with
the ones shown in this section.

#### Boolean Parameters

These **\Boolean** parameters are in the top section of your root 
LaTeX files `book.tex` and `eBook.tex`. You can set them manually,
although others are automatically set by **makebook**.

- FINALFORM: Ready for publication. Set by makebook -f. Removes DRAFT marks and throws errors on missing image files.
- EDITMODE: Editing mode. Enable double spacing for text that you sent to human editors for manual markup.
- WATERMARK: Only valid for eBook. Allows you to set a background watermark (very slow to typeset).
- TOHTML: Format for Tex4HT conversion. Set by makebook for eBooks and HTML output.
- TOMOBI: Extra Formatting for Kindle .MOBI: Set by makebook. Testable in your text for pubication type specifics.
- TOEPUB: Extra Formatting for IDPF .EPUB. Set by makebook. Testable in your text for pubication type specifics.
- RAWHTML: Allow /HCode directly in text [no PDF].
- TOPRESS: Format for paper printing. Set by makebook. Hyperlinks include the url in the text.
- NOCOLOR: Format for monochrome output. Set by makebook. Controls whether \Image will look for **BW** variant files.
- SUBCAPTIONS: Allow subcaptioned figures. These are nice but is not supported by html (ebook) formats. For those you need to build a new image that contains the two (or more) images that you wanted, with their supcaptions.
- PARENTIMAGES: Images are in a parent directory. This is set by makebook depending on whether the image subdirectory needs to be copied to the target.
- CHAPTERNOTES: Place endnotes at end of each chapter
- NOBODY: Incude only the intro [debugging]
- NOLISTS: Do not generate lists of figures and tables
- NOFIGLIST: Do not generate list of figures
- NOTABLIST: Do not generate list of tables
- NOINDEX: Do not generate index entries. Useful in debugging.
- NOAUTHORS: Do not include the Authors section.
- NOHYPERREF: Disables hyperref - href linking.
- NOABBR: Suppress abbreviations is mostly used in debugging.
- NOIMAGES: Disable Graphics inclusion which is useful when you are doing lots of text editing, and image typesetting slows down rebuilds to the point you're annoyed.

#### Images

Your image files should be referenced with the \Image or \IMAGE
command forms. 

```tex
    \Image[options]{filebase}{type}
```
The [options] are just passed to **\includegraphics**.

{filebase} is the filename without the extension. You should create a {filebase}_BW.{type} variations for every image rather than sending color images to your printer.

{type} should be **jpg** or **png** to be compatible with all eBook forms.
Do not use **gif** or multi-media image formats.

```tex
    \IMAGE[options]{filename}
```

Like **\Image**, but the entire filename is provided. All image files must be in the `../images` subdirectory, but you may add further subdirectories within it.

#### Nomenclature and Abbreviations

Technical books will usually define or use a number of terms and
abbreviations. Use the `zzAbbr.tex` file to add the ones that are
relevant for your book.  The standard definition macro will also
create a \ prefixed command. Use this in your text instead of the
raw name in order to get standardized formatting and indexing of the
term usage.  

When an abbreviation contains non alphabetic characters, or already 
exists as a LaTeX command, you'll need to use the alternate * form.

I've include a couple of example macros for defining AI and Neuroscience terms, but you'll need to make equivalent ones that a applicable to your topics. The **\PrintAcronyms** command must be edited to format this appendix.

#### Indexing

Indexing is usually a tedious process, but you can assist this by
using semantic macros in your text. These insert the appropriate
**\Ix** macros as needed.

    - \BookName
    - \StoryName
    - \ArticleName
    - \FilmName
    - \IdeaName
    - \ConditionName
    - \OrganismName
    - \AnimalName
    - \AnatomyName
    - \ComputerName
    - \PersonName
    - \CompanyName
    - \ProjectName
    - \InstitutionName
    - \UniversityName

When you have a specific item in your text to index that doesn't fit
into one of the semantic macros, you should either add an appropriate
semantic macro, or use one of the following inline in your text.

```tex
    \Index{text to index}
    \Inx{text to index and display}
    \PInx{category name}{text to display and index}
    \PInx[text to index]{category name}{text to display}  
```

**\Index** will just add an index entry but otherwise does nothing. The
second **\Ix** form both typesets the text and adds it to the index under a
specific category name. The third form both displays and indexes the text,
but allows you to index by a name different that than displayed. This
is most useful when you have variant forms of the text that should
all index to the same entry such as nicknames, abbreviations, etc.

# Book Formats

The book formats are:

- Print on Demand Grayscale
- Distribution PDF
- ASCII Plain Text Draft
- ePub eBook
- mobi kindle eBook
- html single page site

### Format: Print on Demand Grayscale

The Print on Demand Grayscale format has exact images of every page that
will be between the covers of your book. Where hyperlinks were used
in your book, reasonable accomodations for the printed version need to be
made.

This format is based on the `book.tex` root which uses KOMA-Script Book Formatting.
The results are better for printed books than the LaTeX book class, but unfortunately
not all packages required for eBooks are compatible with KOMA-Script so we
use two different root files. This also limits the number of confusing
conditionals you would have to maintain.

### Format: Distribution PDF

A distribution PDF should not be confused with an eBook format, although
they can be hosted on most eReaders. A PDF renders the actual page formatting
of the print version of the book including headers, footers, page numbers,
and so-on. An eBook uses flow layouts to reformat the text for the geometry
of the reader and are better suited to a wide range of devices including 
smart phones and tablets.

### Format: ASCII Plain Text Draft and Publishers Data

The ASCII text draft format is what Quality Books damands for you to
obtain a **Publisher's Cataloging-in-Publication Data** section for placement
on your copyright page. All libraries, and most bookstores and textbook
stores require this information.

You should go to [Quality Books Publisher's Cataloging-in-Publication](http://www.quality-books.com/pcip.htm) to get started with this. You'll need to send them $100
and the text draft of your book. Don't do this until you have the book at
least half done in order to get a meaningful and complete classification.

### Format: ePub ebook (Applie iBooks, KOBO, Barnes and Noble)

The .epub is the most complicated format since it includes many different
hyperlinks, and while largely derived from html, does not have all the 
flexibility of a set of web pages. The LaTeX is converted to html for
eBooks, by the aptly named [tex4ebook]https://github.com/michal-h21/tex4ebook)
program which is included with the TexLive 2016 distribution.

You **must** manually edit the `eBook.cfg` file which defines
some metadata about your book, including its description and where to find
the eBook cover image (which is **not** the same as the cover of your paperback
version and is **not** necessarily the cover image inserted in the front of
the eBook. This file looks like:

```xml
\Preamble{xhtml}
  \Configure{CoverMimeType}{image/jpeg}
  \CoverMetadata{images/ebookcover.jpg}
  \Configure{OpfScheme}{uuid}
  \Configure{UniqueIdentifier}{92cb3522-cf8b-4f58-b066-6aee78b2bee4}
  \Configure{OpfMetadata}{\EndP\HCode{<dc:subject>Non-Fiction</dc:subject>}}
  \Configure{OpfMetadata}{\EndP\HCode{<dc:subject>Science</dc:subject>}}
  \Configure{OpfMetadata}{\EndP
    \HCode{<dc:description>}
      <div><h3>Book Description</h3>[your HTML description of your book]</div>
    \HCode{</dc:description>}}
\begin{document}
\EndPreamble
```

Some useful links: 
[ePub Secrets](http://epubsecrets.com/where-do-you-start-an-epub-and-what-is-the-guide-section-of-the-opf-file.php), 
[Authorship Tag](https://www.safaribooksonline.com/blog/2009/11/27/practical-epub-metadata-authorship/)

Note that **there will be problems with your epub** file and you will need
to edit it with a good editor such as the [Calibre](http://calibre-ebook.com/)
eBook editor which you should have installed. There are almost always some
issues of metadata and table of content links which you will need to manually
resolve using the editor. Sorry, but that's how it is at this point.

### Format: mobi eBook (Amazon Kindle Direct Publishing)

A .mobi can be generated directly, but this is just useful as a draft.
You need to edit the .epub file with a good ePub editor such as [Calibre](http://calibre-ebook.com/) which can then be used to generate the .mobi.


### Format: Html Single Page Website

The single page website is also the most convenient way to do a quick
evaluation of your eBook formatting. Open the **eBook.html** in your browser
and look especially carefully at equations, images, table of content and
index references. Validate all of the hyperlinks.

To copy the Html site to a webserver, you will need to copy the **./images**
directory, **eBook.html**, **eBook.css**, and all **eBook\*.png** files. 


## ISBN Numbers

You should have ISBN numbers for each verson of your book. 
In the United States these must be either purchased directly from
[Bowker](http://www.bowker.com/) at [MyIdentifiers.com](https://www.myidentifiers.com/get-your-isbn-now). I recommend buying a block of 10 ISBNs for $250. Alternatively, you can
get a 'free' ISBN from [CreateSpace](https://www.createspace.com) if you are
publishing your print version ony with them.
There are issues with doing this, that may make it difficult
to use other printers, but you may decide to save the bucks.

Once you have your ISBNs, modify the `BookParameters.tex` fields, **PrintISBN**,
**PrintISBNShort**, **EbookISBN**, and **EbookISBNShort**. 13 digit ISBN numbers
can be converted to 10 digit an vice versa using [this tool]().  


# Utility Programs

## `makebook` book formatter

**makebook** can build any or all versions of your book.
These are built in their own sub-directories, and the root 
LaTeX file's \Boolean properties are modified to reflect the
format of the book being built.

```
Usage: makebook [version(s)] [options]
Build some or all versions of a book

versions (more than one may be selected):
  --book   : grayscale PDF formatted for a paperback book
  --pdf    : color PDF with hyperlinks formatted for reading
  --epub   : eBook .epub archive with epubcheck validation
  --html   : single html file webpage of the book
  --ascii  : draft ASCII txt file for Publisher CIP data
  --mobi   : draft Kindle .mobi file
  --cover  : paperback book cover PDF generation
  --all    : alternative to using -a

options:
  -a             Generate ALL versions
  -f             Generate FINAL (non-draft) book versions
  -t             trial run: commands are shown but not executed
  -e             Deep trial run: sub-commands are shown but not executed
  -v             verbose output during command execution
  -q             quiet output during command execution
```

## `texclean` Utility

Running `pdflatex` generates a lot of trash intermediate files. You can
delete these with this `./bin/texclean` utility.

```
Usage: texclean [texfilename]
Clean up intermediate and trash files for a LaTeX .tex file.

-a             clean for all .tex files in the directory.
-t             trial run -- commands are shown but not executed.
-q             quiet output during command execution.
-v             verbose output during command execution.

All the various intermediate files created during LaTex processing
of a file are deleted.
```

## `bookinfo` Utility

The BookParameters.tex and book.tex or eBook.tex defines and boolean
values can be displayed as a group.

```
Usage: bookinfo [options] [bookname]
Display parameters and boolean values about a book.tex file.

Options:
  -v             verbose output during command execution.
  -q             quiet mode. Disables all console output.
  -t             trial run -- commands are shown but not executed.
```

# Installation 

Just copy the contents to your directories and use as is. You will need a
complete LaTeX environment, which includes `pdflatex`. This project was
developed on **Linux Mint**. All of the required tools are available for
both **Windows** and Mac **OS X**, but I have not tested it on those platforms.
Some fiddling will no doubt be required. This section describes the tools
that you need on your system and how to get them.

## Prerequisite tools

These tools expect a **bash** commmand line environment as found on most
Linux distributions. Windows and Mac users will may need to do additional
configuration for these.

**perl**, **java**, **sed**, **stat**, **mkdir**, **rm**, **cp** 
must all be available from the command line and work as expected.

#### LaTeX TexLive 2016

 I **strongly** recommend
that you install [TexLive 2016](https://www.tug.org/texlive/doc/texlive-en/texlive-en.html).
Almost all of the tools required by this project are already part of the 
TexLive 2016 distribution. If you try to use ad-hoc versions of LaTeX 
and packages, you will spend a very long time debugging your builds.

#### TeXstudio LaTeX editor

I use [TeXstudio](http://www.texstudio.org/) for interactive editing of all my LaTeX files.

#### Calibre eBook Manager

This is the essential tool for editing and managing eBooks. It lets you load
the book, edit metadata and content, and set up a server for connecting to
eReaders such as iPads, iPhones, Kindles, etc. to download books.

Get if from [Calibre download](http://calibre-ebook.com/download)

#### ePubCheck

You must create fully validated .epub files before attempting to publish
them. This means you must run ePubCheck since it is the same tool the pubisher
will run before accepting your book. *ePubCheck* is a java tool. 

The `./bin/epubcheck` script expects that the jar file for epubcheck is
already located at **$HOME/LaTeX/epubcheck/target/epubcheck.jar**. If you
don't have it, then you can get it via:

```
    git clone https://github.com/IDPF/epubcheck.git
    cd epubcheck
    mvn install    
```
Make sure to read its README.md file, since this also requires it own set 
of tools such as Maven.


#### Undefined control sequence - pgfsys@svg@newline -- Hnewline.fix

When building html, epub, and mobi files, you may encounter a **lot** of
error messages in during the tex4ht conversion. The log file will contain
messages that look like:

```
	! Undefined control sequence.
	\pgfsys@svg@newline ->\Hnewline 
``` 

To fix these I edited my
**/usr/local/texlive/2016/texmf-dist/tex/generic/pgf/systemlayer/pgfsys-tex4ht.def** file. 
At Line 83 in pgfsys-tex4ht.def I changed

```
	\def\pgfsys@svg@newline{\Hnewline}
```
to
```
	% --- jfogarty 8sep2016 Patch for tex4ht
	%\def\pgfsys@svg@newline{\Hnewline}% FROM https://bugzilla.redhat.com/attachment.cgi?id=978709
	\def\pgfsys@svg@newline{{?nl}}
```
Which fixed the huge number of undefined control sequence messages.

#### [Improving Poor Resolution Equations](http://tex.stackexchange.com/questions/43772/latex-xhtml-with-tex4ht-bad-quality-images-of-equations) in EPub Files/HTML

The **tex4ht** program by default converts equations into .png image files,
but at a fairly low resolution. Find your version of tex4ht with:

```
which tex4ht
```

on my system this is `/usr/local/texlive/2016/bin/x86_64-linux/tex4ht`.
The configuration file is called `tex4ht.env` which is in 
`/usr/local/texlive/2016/texmf-dist/tex4ht/base/unix` for my system.
There is also a `win32` version.  

Follow [these instructions](http://tex.stackexchange.com/questions/43772/latex-xhtml-with-tex4ht-bad-quality-images-of-equations) as needed for your system.

I used `dvipng` at a higher resolution which is part of the linux TeXlive
distribution.



# License

This code is licenced under the **MIT License** and the Free Public License 1.0.0.
You are free to choose the one that works for you. if you want to grab parts
for use in your own project then please remove my name and hack away.

