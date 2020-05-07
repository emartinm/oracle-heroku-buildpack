# Heroku Buildpack for Oracle

Heroku buildpack for setting up Oracle Instant Client and the `LD_LIBRARY_PATH` so that the `cx_Oracle`  Python library can find all the files.

# Usage

## Add a detect hook (required)

In order for this buildpack to execute, it will look for a `.oracle.yml` file in your app's root.  The file can be empty but it must exist.

```
$ touch .oracle.yml
```

## Add Buildpack

You'll need to use multiple buildpacks. Heroku natively supports multiple buildpacks, so it's easy:

    $ heroku buildpacks:clear
    $ heroku buildpacks:add heroku/python
    $ heroku buildpacks:add --index 1 https://github.com/emartinm/oracle-heroku-buildpack
    $ heroku buildpacks:add --index 1 heroku-community/apt

The result must be:

    $ heroku buildpacks
    1. heroku-community/apt
    2. https://github.com/emartinm/oracle-heroku-buildpack
    3. heroku/python

The buildpack `heroku-community/apt` will install using `apt` all the packages appearing in the file `Aptfile`. We need to install `libaio1` so that `cx_Oracle` could work. The contents of this `Aptfile` file would be one line:

    libaio1

For some reason, `libaio1` is installed in the unexpected directory `/app/.apt/lib/x86_64-linux-gnu` that is not in added in `LD_LIBRARY_PATH` by the `heroku-community/apt` buildpack. For simplicity, the `oracle-heroku-buildpack` will do this for you (probably it shouldn't, but it makes things easier).

# Changes from the original version
* Ported from Ruby to Bash, as encouraged at https://devcenter.heroku.com/articles/buildpack-api#buildpack-api
* Install the latest version of Oracle Instant Client instead of the fixed version 11.2.0.3.0.
* Removed support of any configuration in the `.oracle.yml` file, so `.ORA` files will not be created.
