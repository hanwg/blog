[![Deploy Hugo site to Pages](https://github.com/hanwg/blog/actions/workflows/hugo.yaml/badge.svg)](https://github.com/hanwg/blog/actions/workflows/hugo.yaml)

---

# Building the site locally for testing

1) Run `.\hugo.exe server`.
2) The local site is available at: https://localhost:1313/

# Installed Modules

1) [hugo-admonitions](https://github.com/KKKZOZ/hugo-admonitions) - Adds callouts.
2) [PaperMod](https://github.com/adityatelange/hugo-PaperMod) - The site's theme.

# Updating Hugo and Modules

To update Hugo,
1) Go to https://github.com/gohugoio/hugo/releases to determine the target version.
2) Edit the `.github/workflows/hugo.yaml` file and replace the value of the build environment variable `HUGO_VERSION` with the version number (exclude the `v` prefix) identified in the previous step.

To update modules,
1) Go to the module: `cd themes/MODULE_NAME`.
2) Checkout the version that you want to use.

# Installing New Modules

1) Run `git submobule add --depth=-1 MODULE_GITHUB_URL themes/MODULE_NAME`.
2) Go to the module: `cd themes/MODULE_NAME`.
3) Checkout the version that you want to use.
4) Edit `config.yml` and add the module to the `theme` section.
5) Run `.\hugo.exe server` to test it out locally.
