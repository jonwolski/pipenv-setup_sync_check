# sync vs check bug

## Steps to reproduce

1. Check out this project
2. `pipenv install --dev`
3. Ensure the `setup.py` is up to date by running the sync command
   ```
   pipenv run pipenv-setup sync --pipfile
   ```
4. Confirm the sync worked by running a check
   ```
   pipenv run pipenv-setup check
   ```

## Expected behavior

The `... check` command succeeds with exit code 0 and the following message:

> No version conflict or missing packages/dependencies found in setup.py!

## Actual behavior

The `... check` command fails with exit code 1 and the following message:

> package 'foo_f' in pipfile but not in install_requires

## Details

I expected that I could run `pipenv-setup sync --pipfile` and that would
ensure that a subsequent `pipenv-setup check` would succeed. However, if I have
an underscore (`_`) in a package name in my Pipfile, the `sync` command will
automatically translate the `_` to `-`. The `check` command does no such
translation, so it fails to find a match for the package in the Pipfile.

I am not sure if this is a bug. A bug is literally a discrepancy between
expected behavior and actual behavior, but it is unclear if my expectations
are correct. It is unusual to have an underscore in a package name, because
installing via the command line (`pipenv install --dev foo_f`) will handle the
translation _before_ saving to the Pipfile. In that hypothetical example,
`pipenv sync ... && ... check` would have succceeded.


