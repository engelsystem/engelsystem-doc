# Engelsystem Documentation

The Engelsystem Docs Page is generated using [HUGO](https://gohugo.io/)
and uses the [Hugo Learn Theme](https://github.com/matcornic/hugo-theme-learn).
For installation instructions see the [Hugo install docs](https://gohugo.io/installation/)

## Clone repo
Use `git clone --recurse-submodules https://github.com/engelsystem/engelsystem-doc.git`
to clone this repository with the theme submodule.

## Local server
You can build and serve the docs locally using:
```bash
hugo server
```

## Build the page
```bash
hugo
```

## Reset theme submodule
```bash
git submodule deinit -f .
git submodule update --init
```

## Update theme
```bash
git submodule update --init --recursive
git submodule foreach git pull origin main
```
