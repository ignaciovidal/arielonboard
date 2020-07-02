# Hall Frontend
----

Hall frontend makes use of the following primary technologies, but a lot
of additional components and plugs not documented here:

* [AngularJS](https://angularjs.org) - Frontend JS Framework. The core
  of the project.
* [Node.js](http://nodejs.org) - Javascript runtime used for a lot of
  the under the hood stuff to manage the app.
* [gulp.js](http://gulpjs.com) - A build system we use for pipelining
  the js, html, css; watching the filesystem for changes; runing the
  karma server, etc...
* [Haml](http://haml.info) - A clean templating engine. HTML templates
  are generated from the Haml files.
* [Sass](http://sass-lang.com) - A `CSS` extension language we use for
  our stylesheets.


## Systems Acquisition, Development and Maintenance Policy

Before you begin to work with this code base, please review the [Systems Acquisition, Development and Maintenance Policy](https://github.com/Hall-and-Partners/hall-and-partners/blob/master/ISMS-C_DOC_14.1.pdf) so you understand the requirements around systems changes for this project.


## Building a development environment
----

Prerequisite packages installed on your machine:

- Node.js
- Bundler (Ruby)

We recommend using [nvm](https://github.com/creationix/nvm) for managing multiple node versions. We also recommend that things like `gulp` be installed locally (something the `npm install`) command will do.  We're currently relying on Node LTS v10.x.

Begin installation with Node dependencies:

    $ npm install

Install all Ruby dependencies:

    $ bundle install

Then simply run gulp:

    $ gulp dev:setup

If you didn't install `gulp` globally, but instead locally (recommended) then you can invoke the command using node scripts:

    $ npm run gulp dev:setup


## Running the development server
----

Once you've got everything installed, you can run the development web
server and the watch server, which rebuilds the pipeline of css, js, and
html:

    $ npm run gulp develop


## Running the frontend only
----

The frontend repository can be run in development mode without the
presence of the backend server by utilizing a staging server's
backend. To do this you need to run your development server under either port
8003 for staging or port 8004 for develop:

    $ npm run gulp develop -- --port=8003

    $ npm run gulp develop -- --port=8004


## Git Hooks
----

Connecting this will automatically ensure that `git commit` branch commits will have the branch number
prepended to the commit message in the format of `[#1234] message`.

    $ git config core.hooksPath ./git_hooks/


## Staging and production releases
----

The staging and production release process is automated using gulp.

To deploy to the main staging enviroment ([https://staging.thehubinsights.com](https://staging.thehubinsights.com)):

    $ npm run gulp -- release --staging

To deploy to the secondary staging enviroment ([https://develop.thehubinsights.com](https://develop.thehubinsights.com))

    $ npm run gulp -- release --develop

To deploy to production:

    $ npm run gulp -- release --production

## Sass compilation
----

Sass is complied with [gulp-sass](https://www.npmjs.com/package/gulp-sass). All Sass files are explicitly imported via the entry point `./src/_init/style.sass`.

## Known issues
----

On OSX you might hit the error `EMFILE: Too many opened files` when running `gulp develop`. `EMFILE` means it is trying to open more file descriptors than your system allows. The default on OSX is 256 and it is really low. You can temporarily increase the limit in your session by running:

    $ ulimit -n 10240

