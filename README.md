Heroku buildpack: stunnel
=========================

This is a [Heroku buildpack][buildpack] that supplies stunnel and is meant to be used as a [multi-buildpack][multi]. It uses [stunnel][stunnel], and was extracted from the [pgbouncer][pgbouncer] buildpack.

[buildpack]: http://devcenter.heroku.com/articles/buildpacks
[multi]: https://github.com/ddollar/heroku-buildpack-multi
[stunnel]: http://stunnel.org/
[pgbouncer]: https://github.com/gregburek/heroku-buildpack-pgbouncer

Usage
-----

Example usage:

    $ ls -a
    Procfile  package.json  web.js  .buildpacks

    $ heroku config:add BUILDPACK_URL=https://github.com/ddollar/heroku-buildpack-multi.git

    $ cat .buildpacks
    https://github.com/timshadel/heroku-buildpack-stunnel.git
    https://github.com/heroku/heroku-buildpack-nodejs.git

    $ cat conf/stunnel.conf
    // your stunnel config file

    $ cat Procfile
    web: node web.js

    $ git push heroku master
    ...
    -----> Heroku receiving push
    -----> Fetching custom buildpack
    -----> Multipack app detected
    =====> Downloading Buildpack: https://github.com/timshadel/heroku-buildpack-stunnel.git
    =====> Detected Framework: stunnel
           Using stunnel version: 4.56
    -----> Fetching and vendoring stunnel into slug
    -----> Generating the startup script
           stunnel will use config at /app/conf/stunnel.conf at boot
    -----> stunnel done
    =====> Downloading Buildpack: https://github.com/heroku/heroku-buildpack-nodejs
    =====> Detected Framework: Node.js
    -----> Node.js app detected
    -----> Vendoring node 0.10.6
    -----> Installing dependencies with npm 1.2.21
           express@3.2.4 ./node_modules/express
           ├── mime@1.2.2
           ├── qs@0.3.1
           └── connect@1.6.2
           Dependencies installed

The buildpack will install and configure stunnel to start at dyno boot reading the config from `$HOME/conf/stunnnel.conf`.

If you don't want a startup file generated

    heroku config:add STUNNEL_STARTUP_FILE=NO
    heroku labs:enable user-env-compile

For more info, see [CONTRIBUTING.md](CONTRIBUTING.md)
