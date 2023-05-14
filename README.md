# This repository has been moved

In an effort to build a community to maintain and advance Deepnest, this repo has been moved to the 
[deepnest-io](https://github.com/deepnest-io/Deepnest) organization.  Please join the 
community and help advance Deepnest!

## **Deepnest**

<img src="https://deepnest.io/img/logo-large.png" alt="Deepnest" width="250">

A fast, robust nesting tool for laser cutters and other CNC tools

Deepnest is a desktop application originally based on [SVGNest](https://github.com/Jack000/SVGnest)

- New nesting engine with speed critical code written in C
- Merges common lines for laser cuts
- Support for DXF files (via conversion)
- New path approximation feature for highly complex parts

No further development will occur on this fork.

## About this fork

This is a fork of [Dogthemachine's fork](https://github.com/Dogthemachine/Deepnest) of [Jack000's
Deepnest](https://github.com/Jack000/Deepnest), with changes made to the make it able to be built
again (only Windows has tested, but work was done in the `binding.gyp` file to improve
multi-platform building, and the dependency on a local path to boost has been removed).

No attempts have been made to upgrade to the latest package versions, and as such this software depends on
old packages.  You must pay close attention to the [prerequisites](#prerequisites) to get a
successful build.  

## Support, issues and pull requests

My primary goal has been to get Deepnest buildable again, though I have made a few small improvments
(see [Code changes in this fork](#code-changes-in-this-fork)). I'm not in a position to support this
fork (I don't know the code and don't have the time), so do not expect any assistance on
improvements or solving problems!  It would be wonderful if somebody (or a team) wanted to bring
this project back to life, update it with latest dependencies and start adding features / fixing
issues.  This fork may be a starting point, or at least perhaps provide guidance on building.  Good
luck!

## Prerequisites

- You must have Visual Studio with C++ extensions installed.  
- Clone this fork with: `git clone --recurse-submodules --remote-submodules
  https://github.com/cmidgley/Deepnest`
- Use [Node.js](https://nodejs.org) version 8 (10 generates lots of warnings, and 12+ fails due to node-gyp).  Recommend using the Node version manager.
  [nvm-windows](https://github.com/coreybutler/nvm-windows/releases) to download Node and change
  versions.
- Ensure you are on Python 2.7 (Python 3 does not work).  Recommend using Python version manager
  [pyenv-win](https://github.com/pyenv-win/pyenv-win) to download and change versions.  Make sure to
  close all command shells (including VSCode) after doing this, to get the latest environment
  variables.  Check with `python --version`.
  - NOTE: If you are running Windows 10 1905 or newer, you might need to disable the built-in Python
  launcher via Start > "Manage App Execution Aliases" and turning off the "App Installer" aliases
  for Python"
- `node-gyp` (the Node to C++ binding environment) requires Visual Studio with C++ extensions.  See
  [this
  page](https://nodejs.github.io/node-addon-examples/getting-started/tools/#:~:text=It%20is%20not%20necessary%20to,that%20has%20everything%20you%20need.) for a simple to install package that may work (not tested, as my system already has it already
  installed).

## Building

Your first build should do the following:

- `npm install`
- `npm run w:build`

If you change the electron-related files (web files, javascript), a build with 
`npm run w:build` is all that is needed.  If you change the the Minkowski files (the `.cc` or `.h` files),
where the NFP (Non-Fit Polygon) logic and background threading is handled, you must rebuild using
`node-gyp` to recompile the files using:

- `npm run w:fullbuild`

## Running

Unless you want to create a [distribution build](#create-a-distribution-build) (a separate set of
executable files that can be run without dependency on the build environment), you can run Deepnest with:

- `npm run w:start`

## Clean builds

Two clean options:
- For regular clean of build artifacts, use `npm run w:clean` and then `npm run w:build`.
- To remove everything, including `node_modules` use `npm run w:fullclean`, then [build](#building) again.

## Create a distribution build

To build a distribution set of files, run:

- `npm run w:dist`

The resulting files will be located in `.\deepnest-win32-x64`.  All files need to be distributed,
meaning a ZIP file or writing a simple installer would be needed to avoid handling a larger number
of files.

## Browser dev tools

If the environment variable "deepnest_debug" has a value of "1", Deepnest will open the browser
dev tools (debugger/inspector).

## Code changes in this fork

Aside from improving the ability to build (mostly in `binding.gyp`, `package.json`, and
`README.md`), the following changes have also been made:

- Cloned the `plus.svg` file into `add_sheet.svg` (it was missing) for the add-sheet icon.
- Added environment variable `deepnest_debug` to open the browser dev tools (see [Browser dev
  tools](#browser-dev-tools)).
- Checked for thrown errors on a couple paths that would cause expections when closing the
  application while the nest was still running.  A better solution would be to cleanly shutdown the
  workers when the app is closed, but this works for now (but could hide important exceptions).
  The two places are marked with a `// todo:` comment in `main.js`.
- Removed a bunch of unused source files.  There are likely more to be discovered.  There also
  appears to be code that is unused within the source files.
- Eliminated the dependency on a manually downloaded instance of boost (completion of work from
  prior fork, using the `polygon` sub-repo).
- Removed all build artifacts.  This means there is no pre-built binaries in this fork and a build
  is required in order to use this.  Eventually a cleaner release solution, perhaps with ZIP or an installer,
  should be added along with making releases offical in GitHub.
- Bumped the version number to 1.0.6cm (the 'cm' to indicate this has come from the `cmidgley`
  fork).
- Eliminated the dependency on the `c:\nest` directory pre-existing, instead using the os-specific temp directory
  (`os.tmpdir()`) and creating it if it does not exist.  
- Clarified the license is MIT.  Removed an old reference to GPL and added the `license` property to
  `package.json`.  Moved the `LICENSE.txt` file to the root and renamed to `LICENSE`.

## Comments about updating to modern packages

See [Updating Deepnest to use modern Node.js, Electron,
etc.](https://github.com/cmidgley/Deepnest/issues/1) for information about my failed effort to bring
Deepnest up to current packages, and thoughts about what it might take.
