[![Build Status](https://travis-ci.org/garris/BackstopJS.svg)](https://travis-ci.org/garris/BackstopJS)

# BackstopJS
**Anticipate CSS curve balls.**

BackstopJS automates CSS regression testing for your web UI by comparing screenshots of your Document Object Model (DOM) at varying viewport sizes.

**Features:** Versatile with multiple configuration files, simulates user interactions with CasperJS scripts, provides 'inline-Command Line Interface' reports quickly, detailed reports in your browser, Continuous Integration (CI) integration with junit reports, tests HTML5 elements such as web fonts and flexbox, friendly with source control.

## BackstopJS Workflow

### Set Up
  - Generate your configuration: Define URLs, screen sizes, DOM selectors, and other key settings.
  - Use BackstopJS to create a set of reference screenshots. This is considered your 'gold standard' which you can update whenever required.

### Repeat the Next Three Steps  
  1. **Make changes** to your CSS, add new components, perform a build, push to production, or wait for any display issues.
  2. **Trigger a test**. BackstopJS creates a set of test screenshots and compares them to the reference screenshots made during setup. Any unexpected changes are displayed in a detailed report.
  3. **Gain insights from your tests.** 🤑

## Learning Resources, Extensions, and More


- Check out the BackstopJS tutorial on [css-tricks.com](http://css-tricks.com/automating-css-regression-testing/).

- Read this informative article on [Making Visual Regression Useful](https://medium.com/@philgourley/making-visual-regression-useful-acfae27e5031#.y3mw9tnxt) by [Phillip Gourley](https://medium.com/@philgourley?source=post_header_lockup).

- Explore automated regression testing for AngularJS web-apps through this article on [DWB](http://davidwalsh.name/visual-regression-testing-angular-applications).

- Want to integrate BackstopJS with your existing gulp build? It’s simple – utilize [gulp-chug](https://github.com/robatron/gulp-chug). Learn how in this article by [Filip Bartos](http://blog.bartos.me/css-regression-testing/).

- If you're a Grunt fan, explore [grunt-backstop](https://github.com/ddluc/grunt-backstop) and this well-written article by [Joe Watkins](http://joe-watkins.io/css-visual-regression-testing-with-grunt-backstopjs/).

- Generate a BackstopJS configuration file from sitemap.xml with [BackstopJS Scenarios Constructor](https://github.com/enzosterro/bscm/) by [Enzo Sterro](https://github.com/enzosterro).

- Visit the BackstopJS brochure at [http://BackstopJS.org/](http://garris.github.io/BackstopJS/).

## Installation

### BackstopJS Package

Add BackstopJS from the root directory of any project.

```sh
$ npm install --save-dev backstopjs
```

### Development Version Installation

```sh
$ npm install garris/backstopjs#master
```

## Configuration

If you don't already have a BackstopJS config file, the following command will create a template config file which you can customize in your root directory. *Caution: this will overwrite any existing BackstopJS config file.*

From the `./node_modules/backstopjs` directory, run:

```sh
$ npm run genConfig
```

By default, `genConfig` will create `backstop.json` in the project root. A `backstop_data` directory will also be created here.

You can specify the locations of the `backstop.json` file and all resource directories. See [Setting the config file path](#setting-the-config-file-path-version-090) for more information.

**A comprehensive tutorial is also available at [css-tricks.com](http://css-tricks.com/automating-css-regression-testing/).**

Here's an example format for a configuration in `backstop.json`:

```json
{
  "viewports": [
    {
      "name": "phone",
      "width": 320,
      "height": 480
    }, 
    {
      "name": "tablet_v",
      "width": 568,
      "height": 1024
    }, 
    {
      "name": "tablet_h",
      "width": 1024,
      "height": 768
    }
  ],
  "scenarios": [
    {
      "label": "My Homepage",
      "url": "http://getbootstrap.com",
      "hideSelectors": [],
      "removeSelectors": [
        "#carbonads-container"
      ],
      "selectors": [
        "header",
        "main",
        "body .bs-docs-featurette:nth-of-type(1)",
        "body .bs-docs-featurette:nth-of-type(2)",
        "footer",
        "body"
      ],
      "readyEvent": null,
      "delay": 500,
      "misMatchThreshold" : 0.1,
      "onReadyScript": null,
      "onBeforeScript": null
    }
  ],
  "paths": {
    "bitmaps_reference": "../../backstop_data/bitmaps_reference",
    "bitmaps_test": "../../backstop_data/bitmaps_test",
    "compare_data": "../../backstop_data/bitmaps_test/compare.json",
    "casper_scripts": "../../backstop_data/casper_scripts"
  },
  "engine": "phantomjs",
  "report": ["browser", "CLI"],
  "debug": false,
  "port": 3001
}
```

**DEVELOPER NOTE:** BackstopJS will run in **Demo** mode and use the default configuration at `./node_modules/backstopjs/capture/config.default.json` if a valid configuration file is not present at the project root or at the specified path from your [Command Line Interface (CLI)](#setting-the-config-file-path-version-090).

## Usage Notes

Refer to the following guidelines when using BackstopJS:

### Generate (or Update) Reference Bitmaps

```sh
$ npm run reference
```

This task creates (or updates an existing) the `bitmaps_reference` directory with screen captures from the current project build.

### Generate Test Bitmaps

```sh
$ npm run test
```

This creates a new set of bitmaps in `bitmaps_test/<timestamp>/`.

After generating the test bitmaps, the program will run a report comparing the most recent test bitmaps against the current reference bitmaps and detect any significant disparities.
