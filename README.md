# Knitter
[![forthebadge](https://forthebadge.com/images/badges/made-with-python.svg)](https://forthebadge.com) [![forthebadge](https://forthebadge.com/images/badges/60-percent-of-the-time-works-every-time.svg)](https://forthebadge.com)

**WARNING: Knitter is currently alpha software. Significant and breaking changes are expected from time to time. Use it at your own risk.**

Build static websites with fewer pains. Knitter was originally built for use in the development of the Dev8 website.

## Why Build Knitter?

We wanted to build a web page to share the results of our survey, [Dev8 Eastern Visayas Tech Scene Survey 2023](https://dev8community.github.io/surveys/evtss2023). We initially tried using vanilla HTML and CSS. However, as development of the page went on, we realized that this method was going to take us longer than we want. The developer experience will not be ideal as well since we were just using one CSS file for styling the entire web page. Things needed to change to allow us to boost our productivity.

We went with trying Vite at first. Webpack was never considered due to prior personal experiences of the author of Knitter. When we wanted to add routes for HTML pages through Vite, we weren't able to add routes in the way we wanted them to be (further details on this part has, sadly, already been lost to history). We felt like we were fighting against the tool itself. Vite seemed like it was focused on modern front-end development with JavaScript frameworks like React and Vue. We needed something that is more traditional, and we need one ASAP. So, we started writing `tool.py`.

`tool.py` was written overnight. We wrote it so that it can build us a static website with SCSS compilation support, routes, and live-reloading. We later added Jinja 2 support once there was a need for templating. We generally fared well with `tool.py`. We decided that it should only be in one file so that it won't be too mixed with our website's codebase. It supported us well during the development of the site for our survey results. Now that the site has been completed, we felt that it was time to move `tool.py` into its own repository so that it can be developed better.

During the time when we started writing `tool.py`, we liked the name, "Knitter". However, since it was just a single script inside the website codebase, we just called `tool.py` temporarily. When we started pulling `tool.py` outside of the Dev8 website codebase into its own repository, we started using the name "Knitter". The name felt right since the tool is there to "knit" together pieces of our code into a deployable static website.

### Why Not Jekyll (or Hugo, etc.)?
We actually forgot they existed. But, we argue that a custom tool will allow us to use a workflow that is best suited for us.

## Installation
Knitter requires a minimum Python version of 3.11, due to its utilization of the `tomllib` standard library that was only available starting with 3.11. Knitter also requires that [Dart Sass](https://github.com/sass/dart-sass) should be installed already in your system. It must also be callable from the terminal with just `sass`. Some package managers, such as [Snap](https://snapcraft.io/dart-sass), name the callable binary as something else. Aliasing the preprocessor to `sass` will be required for such cases. Installation of Knitter can still proceed without pre-installing Dart Sass. However, Knitter will complain if it is not installed yet.

Installing Knitter is easy. You just have to run:

```bash
$ pip install git+https://github.com/dev8community/knitter
```

At the present moment, Knitter is not available at [PyPI](https://pypi.org/).

## Uninstallation
To uninstall Knitter, simply run:

```bash
$ pip uninstall Knitter
```

## Usage
### Commands
Knitter only has two subcommands: `build` and `serve`. Running `knitter build` will make Knitter generate a static distributable website inside of a `dist/` folder. That `dist/` folder is the generated website, and can be copied for deployment.

On the other hand, running `knitter serve` will make Knitter first build the website, and start a live reloading development web server in `http://localhost:2016/`. Any changes to your project _may_ trigger a live reload. Currently, only changes to HTML files and files specified in the `[processed_files]` table in `knitter.toml` will trigger a live reload. Newly-added files, changes to JSON files, and other changes will requiring closing the `Knitter` development server and running it again by calling `knitter serve`.

### Configuration
Knitter will require a configuration file named, `knitter.toml`. It must be located in the root of the project, similar to Rust's `Cargo.toml`, and NodeJS's `package.json`.

`knitter.toml` should look like this:

```toml
[assets_folder]
folder = 'assets/'

[processed_files]
'surveys/evtss2023/css/index.css' = 'src/scss/surveys/evtss2023/index.scss'

[routes]
'/' = 'pages/index.html'
'/surveys/evtss2023' = 'pages/surveys/evtss2023/index.html'

[data]
primary_languages = 'data/surveys/evtss2023/primary-languages.json'
```

The `[assets_folder]` table must contain the required `folder` key, which must contain the path to the assets folder relative to the project root.

The `[processed_files]` table specifies the files that are processed from source files, such as an SCSS file. The processed filenames (keys) should be paths relative to the assets folder in the distributable folder. The source file should be relative to Knitter. For now, we only support processing SCSS files into CSS files.

The `[routes]` table specifies the routes that our static website will have. The keys are the routes while the values contain the path of the files that will be made available from the specified route in the deployed website. Currently, we only support specifying HTML files.

Lastly, the `[data]` table specifies the data that will be made available to templates. The key will be used as the name that is accessible from the templates, and the value will be the source data file. The data file is expected to be in JSON format currently. This may change at a later date.

## Contributing
We encourage outside contributions to Knitter. Not all may be included into the project, but we still appreciate them. Please feel free to fork this repository first, and then sending us a pull request into the `develop` branch.

## Authors and Contributors
Knitter was initially written by [Sean Ballais](https://seanballais.com).
