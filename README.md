# Heroku Buildpack for Oracle

Heroku buildpack for setting up Oracle Instant Client and the `LD_LIBRARY_PATH` so that the `cx_Oracle`  Python library can find all the files (with the exception of `libaio1`, as discussed later).

# Usage

## Add a detect hook (required)

In order for this buildpack to execute, it will look for a `.oracle.yml` file in your app's root.  The file can be empty but it must exist.

```
$ touch .oracle.yml
```

## Add Buildpack

You'll need to use multiple buildpacks. Heroku natively also support's multiple buildpacks, so it's easy:

    $ heroku buildpacks:clear
    $ heroku buildpacks:add heroku/python
    $ heroku buildpacks:add --index 1 https://github.com/emartinm/oracle-heroku-buildpack
    $ heroku buildpacks:add --index 1 heroku-community/apt

The result must be:

    $ heroku buildpacks
    1. heroku-community/apt
    2. https://github.com/emartinm/oracle-heroku-buildpack
    3. heroku/python

# Changes from the original version
* Ported from Ruby to Bash, as encouraged at https://devcenter.heroku.com/articles/buildpack-api#buildpack-api
* Install the latest version of Oracle Instant Client instead of the fixed version 11.2.0.3.0.
* Removed support of any configuration in the `.oracle.yml` file, so `.ORA` files will not be created.
