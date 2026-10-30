A list marker after a LineBreak inside emphasis or a span is escaped,
as it is after a LineBreak at the top level of a paragraph.

```
% pandoc -f html -t markdown
<p><strong>Lead:<br>- alpha</strong></p>
<p><strong>a<em>Lead:<br>3. alpha</em></strong></p>
<p><span class="x">Lead:<br>+ b</span></p>
^D
**Lead:\
\- alpha**

**a*Lead:\
3\. alpha***

[Lead:\
\+ b]{.x}
```

Inside a list item, an unescaped marker would be read back as a sublist.

```
% pandoc -f html -t markdown
<ul><li><strong>Lead:<br>- alpha</strong></li></ul>
^D
- **Lead:\
  \- alpha**
```

```
% pandoc -f markdown -t native
- **Lead:\
  \- alpha**
^D
[ BulletList
    [ [ Plain
          [ Strong
              [ Str "Lead:"
              , LineBreak
              , Str "-"
              , Space
              , Str "alpha"
              ]
          ]
      ]
    ]
]
```
