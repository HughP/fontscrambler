# fontscrambler



#### Table of Contents

1. [Overview](#overview)
2. [Module Description - What the module does and why it is useful](#module-description)
3. [Usage - Configuration options and additional functionality](#usage)



##Overview

This tool was created to demonstrate a simple idea to mitigate mass digital surveillance and big data analytics by altering the character encoding process on such way, which remains compatible with major online and offline formats, but increases the costs of computer analysis by forcing algorithms to process visual information.



##Module Description

####`generate` script

The generate script is written in python and uses the FontForge library to manipulate typefaces. The following arguments are accepted:

* -i FILENAME: Non-scrambled TrueType font file to be scrambled.
* --multiglyph: Assignes multiple glyphs to one character to avoid the frequency analysis attack. For demonstration purposes a 1:10 assignment were implemented, which has to be further randomized for effective protection.
* --shake FACTOR: Implements the glyph hash protection algorithm. All points of all glyphs are moved by a random distance, which is proportional to the FACTOR multiplied with the width/height of the glyph. Then any remaining overlap is removed.
* --components: Changes the number of points in the glyph to prevent prediction by components. This is done by calling the simplify function of FontForge after shaking, which reduces the number points in the glyph.
* --no-scramble: Disables the glyph scrambling function to allow testing features individual.
* --seed SEED: Sets the seed of the pseudo-random generator to SEED. This allows the recreation of specific scrambling tables.



####`scramble` script

The scramble script encodes text from the standard input according to the scrambling table specified in the argument.



####`gocr`, `ocrad`, and `tesseract` folders

Folders to measure the decoding effort of popular OCR softwares. Just run the `runme_ascii` script for a single, specific test, or the `runme` script to run multiple tests with different shaking factors, DPI, and random seeds.



##Usage

To scramble the `Andika-R.ttf` font (and set the random seed generator to 12345) use:

```bash
./generate -i Andika-R.ttf --seed 12345
```

This will create the scrabled `Andika-0-12345.ttf` font:

IMG

and the `Andika-0-12345.txt` scrambler table for text encoding:

```text
0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ.
hHcQs9wqC.yYtjXfomTbIuFL5rO3UBnpaM6dDAGxiRJzgW4SP12e0vlZEkN78KV
```

To encode a text for display with the scrambled `Andika-0-12345.ttf` font, use:

```bash
echo "My example sentence." |./scramble Andika-0-12345.txt
```

which creates the following scrambled text:

```text
P6 XMyFruX UXLBXLtXV
```

Use this text with the scrambled font to display surveilance and analytics safe messages:



####HTML example

```html
<!DOCTYPE html>
<html><head>
<style> 
@font-face {
    font-family: myFirstFont;
    src: url(https://example.com/Andika-0-12345.ttf);
}
div {
    font-family: myFirstFont;
}
</style></head>
<body>
<div>P6 XMyFruX UXLBXLtXV</div>
</body></html>
```



####Latex example

```tex
%!TEX program = xelatex

\documentclass{article}[12pt]
\usepackage{fontspec}

\newfontfamily\ScrambledFont[
  Path = ./,
  UprightFont = Andika-0-12345.ttf,
  ItalicFont = Andika-0-12345.ttf,
  BoldFont = Andika-0-12345.ttf,
  BoldItalicFont = Andika-0-12345.ttf
]{Andika-0-12345.ttf}

\begin{document}
\ScrambledFont{P6 XMyFruX UXLBXLtXV}
\end{document}
```
