# Reveal Slides for Github Pages via Pandoc

## Writing slides

Create entire slide deck (index.html) from markdown files in slides/ directory.
```sh
pandoc -t revealjs --template=pandoc-templates/default.revealjs \
  -o docs/index.html  --slide-level=2 \
  -V reveal.js=./reveal.js  -V theme=white slides/*.m
```

## Created gh-pages


