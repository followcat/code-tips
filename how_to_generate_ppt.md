# How to generate ppt

## FengLiji

### 2015-4-ForthWeek


# Why generate

- Simple
- Fast
- Generalduty
- GEEK

# To generate on Linux（Unix）

## Tools

    - pandoc 
    - reveal.js

## Install pandoc

    ➜ cabal install cabal-install
    ➜ ~/.cabal/bin/cabal update
    ➜ ~/.cabal/bin/cabal install pandoc

## Output html ppt

    ➜ cd ~/.cabal/bin
    ➜ ./pandoc -s -t revealjs source.md -o output.html -V theme=default  --self-contained

## Some params

### -t formats

    Output formats: 
        asciidoc, beamer, context, docbook, docx, dokuwiki, dzslides,
        epub, epub3, fb2, haddock, html, html5, icml, json, latex, man,
        markdown, markdown_github, markdown_mmd, markdown_phpextra,
        markdown_strict, mediawiki, native, odt, opendocument, opml,
        org, pdf*, plain, revealjs, rst, rtf, s5, slideous, slidy,
        texinfo, textile

### -V theme=

    Select theme. Can be black, league, night, simple, sky, white, beige, blood, moon, serif, solarized in reveal.js
    Can create theme.

### -i

    Show steps by steps.

### -V transition=

    You can select from different transitions, like: 
    None - Fade - Slide - Convex - Concave - Zoom</section></section><section><section id="end" class="titleslide slide level1">

# End

## This PPT's markdown code
```
> % How to generate ppt
>     % FengLiji
>     % 2015-4-ForthWeek> 
>     # Why generate> 
>     
>     ## Simple
>     ## Fast
>     ## Generalduty
>     ## GEEK> 
>     
>     # To generate on Linux（Unix
>     
>     ## Tools
>         + pandoc 
>         + reveal.js
>     
>     ## Install pandoc
>     
>         ➜ cabal install cabal-install
>         ➜ ~/.cabal/bin/cabal update
>         ➜ ~/.cabal/bin/cabal install pandoc
>     
>     ## Output html ppt
>         ➜ cd ~/.cabal/bin
>         ➜ ./pandoc -s -t revealjs source.md -o output.html -V theme=default --self-contained> 
>     
>     # Some params> 
>     
>     ## -t formats
>         Output formats: 
>             asciidoc, beamer, context, docbook, docx, dokuwiki, dzslides,
>             epub, epub3, fb2, haddock, html, html5, icml, json, latex, man,
>             markdown, markdown_github, markdown_mmd, markdown_phpextra,
>             markdown_strict, mediawiki, native, odt, opendocument, opml,
>             org, pdf*, plain, revealjs, rst, rtf, s5, slideous, slidy,
>             texinfo, textile
>     
>     ## -V theme=
>         Select theme. Can be black, league, night, simple, sky, white, beige, blood, moon, serif, solarized in reveal.js
>         Can create theme.
>     
>     ## -i
>         Show steps by steps.
>     
>     ## -V transition=
>         You can select from different transitions, like: 
>         None - Fade - Slide - Convex - Concave - Zoom> 
>     
>     # End
>     
>         ## This PPT's markdown code
>     
>     ## This is an online Editor
>     ![Slides]<http://slides.com></section><section id="this-is-an-online-editor" class="slide level2">
```

# This is an online Editor
[Slides][http://slides.com](http://slides.com)