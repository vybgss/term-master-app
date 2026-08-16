# term-master-app

The landing page for [Term Master](https://github.com/vybgss/term-master), served
by GitHub Pages, and the place the built app is downloaded from.

| Path | What lives there |
| --- | --- |
| `index.html` | The whole page — markup, styles and script in one file, no dependencies |
| `assets/` | The app icon, used as the hero image and the favicon |
| `downloads/Term-Master.zip` | The current build. A stable name, so the page can link to it without JavaScript |
| `downloads/archive/` | Earlier builds, only when published with `--keep` |
| `releases.json` | Version, build, size, checksum and date of what is in `downloads/` |

Everything published here is notarised and stapled — `publish.sh` refuses to
copy in a build that is not, because macOS refuses to open one.

## Publishing a build

From the app repository next door:

```sh
Scripts/publish.sh
```

It builds a signed, notarised release, zips it, copies it in here, writes
`releases.json`, then commits and pushes. `Scripts/publish.sh --help` lists the
options.

Nothing here is generated at build time: the page is static and `releases.json`
is the only thing that changes between releases. Dropping a zip into
`downloads/` by hand works too — update `releases.json` beside it, or the page
will keep quoting the old version.

The download link in the markup is a plain `href` to
`downloads/Term-Master.zip`. `releases.json` is fetched to fill in the version,
the size and the checksum; if that fetch fails the page says nothing has been
published yet rather than offering a link to nothing.

## Looking at it locally

Opening `index.html` from the filesystem works, but `fetch` of `releases.json`
is blocked on `file://`, so the version line stays empty. Serve the directory
instead:

```sh
python3 -m http.server -d . 8000
```

## Notes

- `.nojekyll` keeps GitHub Pages from running the files through Jekyll.
- Every published build stays in the git history, so the repository grows by the
  size of a zip each release. `--keep` also leaves the previous one in the
  working tree, under `downloads/archive/`.
