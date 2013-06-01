# Heroku Buildpack: CHICKEN Scheme

This is a [Buildpack][] for deploying [CHICKEN Scheme][chicken] apps
on Heroku's [Cedar][] stack.

It comes with CHICKEN [4.8.0][releases] and uses the [egg][] packaging
infrastructure to manage dependencies.

## Usage

    $ git ls-files
    Procfile
    deploy.meta
    run.scm

    $ cat deploy.meta
    ((description "An example CHICKEN app for Heroku")
     (depends awful))

    $ cat run.scm
    (use awful)
    (define-page (main-page-path)
      (lambda () "Hello World!"))

    $ cat Procfile
    web: awful --port=$PORT run.scm

    $ heroku create --buildpack git://github.com/evhan/heroku-buildpack-chicken.git
    ...

    $ git push heroku master
    ...
    -----> Heroku receiving push
    -----> Fetching custom buildpack... done
    -----> CHICKEN app detected
    -----> Installing CHICKEN
    -----> Installing eggs
    ...
    -----> Copying CHICKEN onto slug
    -----> Discovering process types
           Procfile declares types -> web
    -----> Compiled slug size is 5.4MB
    -----> Launching... done, v1
           http://deep-journey-2786.herokuapp.com deployed to Heroku

To be recognized as a CHICKEN application, your project must have a `run.scm`,
`deploy.meta` or `deploy.setup` file in its root. If no [`Procfile`][procfile]
is included in the app, `run.scm` will be run as the default `web` process.

If a `deploy.meta` file is present, dependencies listed therein will be
installed onto your slug during the deploy. This file should follow
CHICKEN's [metafile][] format.

If a `deploy.setup` file is present, it will be run by `chicken-install` during
the deploy. This file should follow CHICKEN's [setupfile][] format.

The vendored CHICKEN and installed eggs are cached between deploys. If a
`deploy.setup` file is present, however, it is *always* rerun.

A specific version of CHICKEN to install can be defined in the metafile:

    $ cat deploy.meta
    ((description "An example CHICKEN app for Heroku")
     (depends awful)
     (chicken 4.8.0))

Currently, CHICKEN [4.8.0][releases], [4.7.0][releases] and [4.7.0.5-st][st]
are available in this way.

[buildpack]: https://devcenter.heroku.com/articles/buildpacks
[chicken]: http://call-cc.org/
[cedar]: https://devcenter.heroku.com/articles/cedar
[releases]: http://code.call-cc.org/releases
[egg]: http://wiki.call-cc.org/eggs
[procfile]: http://devcenter.heroku.com/articles/procfile
[metafile]: http://wiki.call-cc.org/eggs%20tutorial#the-meta-file
[setupfile]: http://wiki.call-cc.org/eggs%20tutorial#the-setup-file
[st]: http://wiki.call-cc.org/stability
