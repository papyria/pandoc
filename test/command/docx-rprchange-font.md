A run's font comes from its own `w:rPr/w:rFonts`. The `w:rFonts` inside
a `w:rPrChange` records the font before a tracked formatting change, so
it must not make a run read as Symbol text, and it must not hide the
run's own Symbol font.

```
% pandoc command/docx-rprchange-font.docx -t html
^D
<p>abc</p>
<p>αβχ</p>
```
