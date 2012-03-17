# Heroku Buildpack: Chicken Scheme

This is a [Buildpack][] for deploying [Chicken Scheme][chicken] apps
on Heroku's [Cedar][] stack.

It comes with Chicken [4.7.0][] and uses the [egg][] packaging infrastructure
to manage dependencies.

## Usage

    $ git ls-files
    Procfile
    deploy.meta
    run.scm

    $ cat deploy.meta
    ((description "An example Chicken app for Heroku")
     (depends awful))

    $ cat run.scm
    (use awful)
    (define-page (main-page-path)
      (lambda () "Hello World!")

    $ cat Procfile
    web: awful --port=$PORT run.scm

    $ heroku create --stack cedar --buildpack git://github.com/evhan/heroku-buildpack-chicken.git
    ...

    $ git push heroku master
    ...
    -----> Heroku receiving push
    -----> Fetching custom buildpack... done
    -----> Chicken app detected
    -----> Installing Chicken
    -----> Installing eggs
    ...
    -----> Copying build onto slug
    -----> Discovering process types
           Procfile declares types -> web
    -----> Compiled slug size is 5.4MB
    -----> Launching... done, v1
           http://deep-journey-2786.herokuapp.com deployed to Heroku

To be recognized as a Chicken application, your project must have a `run.scm`,
`deploy.meta` or `deploy.setup` file in its root. If no [`Procfile`][procfile]
is included in the app, `run.scm` will be run as the default `web` process.

If a `deploy.meta` file is present, dependencies listed therein will be
installed onto your slug during the deploy. This file should follow Chicken's
[metafile][] format.

If a `deploy.setup` file is present, it will be run by `chicken-install` during
the deploy. This file should follow Chicken's [setupfile][] format.

The vendored Chicken and installed eggs are cached between deploys. If a
`deploy.setup` file is present, however, it is *always* rerun.

[buildpack]: https://devcenter.heroku.com/articles/buildpacks
[chicken]: http://call-cc.org/
[cedar]: https://devcenter.heroku.com/articles/cedar
[4.7.0]: http://code.call-cc.org/releases
[egg]: http://wiki.call-cc.org/eggs
[procfile]: http://devcenter.heroku.com/articles/procfile
[metafile]: http://wiki.call-cc.org/eggs%20tutorial#the-meta-file
[setupfile]: http://wiki.call-cc.org/eggs%20tutorial#the-setup-file
