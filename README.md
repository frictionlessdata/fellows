## Contributing
We're excited you want to contribute! The project follows the Open Knowledge Foundation [coding standards](https://github.com/okfn/coding-standards).

### Contribute via a Pull Request (PR):
- Clone this repository
- Make changes locally on a new branch
- Open a PR with a clear list of the changes you have made
- Request review of your PR from another contributor (e.g. @lwinfree)
- The reviewer will approve (or ask for changes) and then merge the PR. Please do not merge your own PRs.
- Read more about [PRs here](http://help.github.com/pull-requests/)

### Open an Issue
Please [open an issue](https://github.com/frictionlessdata/fellows/issues) if you see something wrong or want to discuss ideas for improvement/edits.

### Editing fellows.frictionlessdata.io

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
