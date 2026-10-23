The untitled section the EPUB writer adds before the first heading
must not take an identifier a heading already has, or links to that
heading resolve to the wrong chapter.

```
% pandoc -f native -t epub3 --split-level=1 --metadata title=1 -o - | pandoc -f epub -t html
[ Para [ Str "Front" ]
, Header 1 ( "section" , [] , [] ) [ Str "1" ]
, Para
    [ Str "Se"
    , Space
    , Link
        ( "" , [] , [] )
        [ Str "kapittel" , Space , Str "1" ]
        ( "#section" , "" )
    , Str "."
    ]
]
^D
<p><span id="title_page.xhtml"></span></p>
<p><span id="ch001.xhtml"></span></p>
<section id="ch001.xhtml_section-1" class="unnumbered level1">
<h1 class="unnumbered">1</h1>
<p>Front</p>
</section>
<p><span id="ch002.xhtml"></span></p>
<section id="ch002.xhtml_section" class="level1">
<h1>1</h1>
<p>Se <a href="#ch002.xhtml_section">kapittel 1</a>.</p>
</section>
```
