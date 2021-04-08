# ReTypeTron

> Minimalistic React + TypeScript + Electron Boilerplate. Nothing more.

Uses a minimal webpack config to bundle everything together, and includes
just what you need to build your electron app. Tiny enough so that you keep
a good overview of everything, simple enough for you to add what you need
and still complete enough to start working on production projects.

Features:

- Completely typed, not only your application code, but also the electron
  main process code, the webpack configuration files (editor suggestions
  in those files make extensions really easy!), and in json configuration
  files via json schemas.
- No built-in frontend libraries like `react-router` or `redux`. They
  are dead-simple to integrate manually, this boilerplate does not make
  assumptions about your tech stack.
- `electron-builder` included for bundling the app for Windows, Linux
  and Mac. Easily adjustable configurations allows bundling installers or
  zipped portable packages.
- `prettier` included for formatting.

## Preparing releases

The CI pipeline is setup to automatically release new versions for 
pushed tags. Make sure to bump the version in the ``package.json``
file, and tag the commit with the new version (e.g. ``v1.0.0``).

The following manual steps have to be completed initially to setup
releases:
* Set a GitHub Actions secret with the name ``GH_SECRET`` that has repo access
* Set a suitable ``appId`` and ``productName`` in ``electron-builder.json``
* Set suitable ``name``, ``description``, ``author``, ``repository`` values in ``package.json``
* Overwrite ``resources/icon.png`` with a suitable logo image. Note that, for the Mac OS build to work, it must be at least 512x512 

## Removing stuff

Utilities like `electron-builder` and `prettier` are included for
convenience, but can easily be removed if you don't want them.

### Removing `electron-builder`

- Remove the fields `build:unpacked`, `build:packed`, `build` from
  `package.json:scripts`.
- Remove the dependency `electron-builder`.
- Remove the file `electron-builder.json`.

### Removing `prettier`

- Remove the fields `prettier:check` and `prettier:write` from
  `package.json:scripts`
- Remove the dependency `prettier`.
- Remove the file `prettierrc.json`.

## FAQ

- Where does the code land once built?
  - The built files `index.html`, `electron-main.js` and
    `js/main.js` (render logic) are placed in the `app/`
    folder, which is loaded into the root of the
    `resources/app.asar` archive once built into an
    distributable package.
- How can I access the location of the built code?
  - e.g. `path.join(app.getAppPath(), '/app/index.html')`
- How can I add other files to be included into that archive?
  - Add the file paths as glob to the `files`-array in
    `electron-builder.json`.
- How can I import images/css/scss/other custom things in
  TypeScript?
  - Add the relevant loaders in `webpack-renderer.config.ts`
    if you want to load those files in the render process, or
    in `webpack-electron.config.ts` for the main process.
- How can I change the icon?
  - `resources/icon.png`
- I don't want to use yarn.
  - Remove the file `yarn.lock`, change `yarn` to `npm run`
    in the scripts inside of `package.json`.
- I want a more comprehensive boilerplate that includes more features.
  - Look into https://github.com/electron-react-boilerplate/electron-react-boilerplate
- Using async ``fs``-method do not return and block indefinitely.
  - https://github.com/electron/electron/issues/22119
  - TLDR: Set ``app.allowRendererProcessReuse = false;`` in ``src/electron-main.ts:37``
