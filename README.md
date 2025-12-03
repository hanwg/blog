[![Deploy Hugo site to Pages](https://github.com/hanwg/blog/actions/workflows/hugo.yaml/badge.svg)](https://github.com/hanwg/blog/actions/workflows/hugo.yaml)

# Hugo Blog

This is the source for [Han Wuguang's Tech Blog](https://hanwg.top).

## Customizations

Some minor customizations were done on the PaperMod theme:
- `assets`
- `layouts`
- `static`

## Installed Modules

1) [hugo-admonitions](https://github.com/KKKZOZ/hugo-admonitions) - Adds callouts.
2) [PaperMod](https://github.com/adityatelange/hugo-PaperMod) - The site's theme. Supports dark mode and search.

---

## Guides

### How-To: Build the site locally for testing

Pre-requisites: [Install Hugo](https://gohugo.io/installation/windows/)

1) Run `.\hugo.exe server --buildDrafts --buildFuture`.
2) The local site is available at: https://localhost:1313/

### How-To: Add a new post

1) Create a new folder under `content/posts/YYYY/MM/TITLE`. This will be the [page bundle](https://gohugo.io/content-management/page-bundles/) for the new post.
2) Create a new `index.md` file in the page bundle. This file contains the content of your new post.
3) Build the site locally and test.

### How-To: Update Hugo and modules

To update Hugo,
1) Go to https://github.com/gohugoio/hugo/releases to determine the target version.
2) Edit the `.github/workflows/hugo.yaml` file and replace the value of the build environment variable `HUGO_VERSION` with the version number (exclude the `v` prefix) identified in the previous step.

To update modules,
1) Go to the module: `cd themes/MODULE_NAME`.
2) Run `git pull` to update to the latest version.

### How-To: Install new modules

1) Run `git submobule add --depth=-1 MODULE_GITHUB_URL themes/MODULE_NAME`.
2) Go to the module: `cd themes/MODULE_NAME`.
3) Checkout the version that you want to use.
4) Edit `config.yml` and add the module to the `theme` section.
5) Run `.\hugo.exe server` to test it out locally.
