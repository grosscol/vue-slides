# Reveal Slides for Github Pages via Pandoc

## Writing slides

Create entire slide deck as single markdown
```sh
pandoc -s -t revealjs -o index.html \
 --slide-level=2 -V reveal.js=./reveal.js \
 -V theme=solarized slides/all-in-one.md
```

## Created gh-pages


