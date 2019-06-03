# Reveal Slides for Github Pages via Pandoc

slides available at https://display-lab.github.io/slides-template

## Making a new presentation
Clone and rename this repository.

```sh
git clone --shallow-submodules --recurse-submodules \
  https://github.com/display-lab/slides-template.git newname
```
## Writing slides

Create entire slide deck (index.html) from markdown files in slides/ directory.
```sh
pandoc -t revealjs --template=pandoc-templates/default.revealjs \
  -o index.html  --slide-level=2 \
  -V reveal.js=./reveal.js  -V theme=white slides/*.md
```

## Publishing to Github Pages

1. Create repo: https://github.com/new
1. Set repo as remote
    ```sh
    git remote add origin URL_TO_YOUR_REPO
    ```
1. Push up contents
    ```sh
    git push --set-upstream origin gh-pages
    ```
1. Change repository settings on github to serve Github Pages from gh-branch.
[more info](https://help.github.com/articles/configuring-a-publishing-source-for-github-pages/)


## Dependencies

### Pandoc
Required and used to generate the slides html.

Installation: https://pandoc.org/installing.html


### Jekyll
The underlying technology that powers github pages.
Not required to generate slides,
but useful for examining how the repo will be processed.

Installation: https://jekyllrb.com/docs/installation/



