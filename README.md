# PNPM should prioritize linking local (private) packages in repo instead of installing external packages

## Steps to reproduce

1. Clone this repo.
2. Run `pnpm recursive install`.
3. Run `node index.js`.

## Expected behavior

After step 2, `pnpm` should link `/tools/foo` to `/tools/bar/node_modules`.

After step 3, `/index.js` should calls `/tools/bar/index.js` which in turn calls `/tools/foo/index.js` which prints `'foo'`.

## Actual behavior

After step 2, `pnpm` installs an actual [`foo` package](https://www.npmjs.com/package/foo) from npm registry which is not what I needed.

## Other details

<details>
  <summary>Repo Overview</summary>

  * This is a multi-packages repo.

  * Directory [`/tools`] contains packages that are used as tools in dev environment (e.g. to assist testing, or to validate packages before publishing, etc.) and therefore have `"private": true` in their `package.json`.

  * Package `/tools/bar` depends on `foo` (`/tools/foo`).


**Folder structure:**

```
  .
  ├── index.js
  ├── pnpm-workspace.yaml
  ├── README.md
  └── tools
      ├── bar
      │   ├── index.js
      │   └── package.json
      └── foo
          ├── index.js
          └── package.json

```
</details>

<details>
  <summary>Why do I want this?</summary>

  * It is nicer to call package by their names instead of write out full paths to their files.
  * It allows me to isolate their dependencies from production packages' dependencies.
  * It makes it easier for me to know whether a dependency is still neccessary.
</details>

<details>
  <summary>Why don't I just rename to package or increase version to really high?</summary>

  * It doesn't work in a long term.
</details>
