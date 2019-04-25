This site is built with [Lektor](https://www.getlektor.com/).

At time of writing there is no Lektor GUI client for Mac. Instead, install Lektor from the commandline with:

```sh
$ pip install Lektor
```

With Lektor installed, start the development server with:

```sh
$ lektor serve
```

It has a bunch of [dependencies](package.json), so do `npm install` and then `npm run build`.

`grunt` will watch for changes to your [SCSS files](assets/scss), and also [icons](assets/icons) (see [svgstore](https://github.com/FWeinb/grunt-svgstore)).

This repo is configured to deploy to Github pages when published.

`lektor deploy` will deploy to the gh-pages branch, putting changes live, if you have deploy permissions.
