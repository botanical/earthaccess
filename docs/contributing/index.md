# Contributing

When contributing to this repository, please first discuss the change you wish to make via issue,
email, or any other method with the owners of this repository before making a change.

Please note that we have a [code of conduct](./CODE_OF_CONDUCT.md). Please follow it in all of your interactions with the project.

## Development environment

1. Fork [nsidc/earthaccess](https://github.com/nsidc/earthaccess)
1. Clone your fork (`git clone git@github.com:{my-username}/earthaccess`)

`earthaccess` uses Poetry to build and publish the package to PyPI, the defacto Python repository. In order to develop new features or fix bugs etc. we need to set up a virtual environment and install the library locally. We can accomplish this with Poetry and/or Conda.

### Using Conda

If we have `mamba` (or `conda`) installed, we can use the environment file included in the `ci` folder. This will install all the libraries we need (including Poetry) to start developing `earthaccess`:

```bash
mamba env update -f ci/environment-dev.yml
mamba activate earthaccess-dev
poetry install
```

After activating our environment and installing the library with Poetry we can run Jupyter lab and start testing the local distribution or we can use `make` to run the tests and lint the code.
Now we can create a feature branch and push those changes to our fork!

### Using Poetry

If we want to use Poetry, first we need to [install it](https://python-poetry.org/docs/#installation). After installing Poetry we can use the same workflow we used for Conda, first we install the library locally:

```bash
poetry install
```

and now we can run the local Jupyter Lab and run the scripts etc. using Poetry:

```bash
poetry run jupyter lab
```

!!! note

    You may need to use `poetry run make ...` to run commands in the environment.

### Managing Dependencies

If you need to add a dependency, you should do the following:

- Run `poetry add <package>` for a required (non-development) dependency
- Run `poetry add --group=dev <package>` for a development dependency, such
  as a testing or code analysis dependency

Both commands will add an entry to `pyproject.toml` with a version that is
compatible with the rest of the dependencies.  However, `poetry` pins versions
with a caret (`^`), which is not what we want.  Therefore, you must locate the
new entry in `pyproject.toml` and change the `^` to `>=`.  (See
[poetry-relax](https://github.com/zanieb/poetry-relax) for the reasoning behind
this.)

In addition, you must also add a corresponding entry to
`ci/environment-mindeps.yaml`.  You'll notice in that file that required
dependencies should be pinned exactly to the versions specified in
`pyproject.toml` (after changing `^` to `>=` there), and that development
dependencies should be left unpinned.

Finally, for _development dependencies only_, you must add an entry to
`ci/environment-dev.yaml` with the same version constraint as in
`pyproject.toml`.

## First Steps to contribute

- Read the documentation
- Fork this repo (see "Development environment" section above for more)
- Install environment (see "Development environment" section above for more)
- Run the unit tests successfully in `main` branch:
    - `make test`

From here, you might want to fix and issue or bug, or add a new feature.  Jump to the
relevant section to proceed.

### ...to fix an issue or bug

- Create a GitHub issue with a
  [minimal reproducible example](https://en.wikipedia.org/wiki/Minimal_reproducible_example),
  a.k.a Minimal Complete Verifiable Example (MCVE), Minimal Working Example (MWE),
  SSCCE (Short, Self Contained, Complete Example), or "reprex".
- Create a branch to resolve your issue
- Run the unit tests successfully in your branch
- Create one or more new tests to demonstrate the bug and observe them fail
- Update the relevant code to fix the issue
- Successfully run your new unit tests

Once you've completed these steps, you are ready to submit your contribution.

### ...to contribute a new feature

- Create an issue and discuss the feature's scope and its fit for this package with the team
- Create a branch for your new feature in your fork
- Run the unit tests successfully in your branch
- Write the code to implement your new feature in a backwards compatible manner
- Create at least one test that exercises your feature and run the test suite as you go

Once you've completed these steps, you are ready to submit your contribution.

## Submitting your contribution

- Run all unit tests successfully in your branch
- Lint and format your code.  See below.
- Update the documentation and CHANGELOG.md
- Submit the fix to the problem as a pull request
- Include an explanation of what you did and why in the pull request

### Please format and lint as you go

```bash
make format lint
```

We attempt to provide comprehensive type annotations within this repository.  If
you do not provide fully annotated functions or methods, the `lint` command will
fail.  Over time, we plan to increase type-checking strictness in order to
ensure more precise, beneficial type annotations.

We have included type stubs for the untyped `python-cmr` library, which we
intend to eventually upstream.  Since `python-cmr` exposes the `cmr` package,
the stubs appear under `stubs/cmr`.

### Requirements to merge code (Pull Request Process)

- you must include test coverage
- you must update the documentation
- you must format and lint

## Pull Request process

1. Ensure you include test coverage for all changes
1. Ensure your code is formatted properly following this document
1. Update the documentation and the `README.md` with details of changes to the
   interface, this includes new environment variables, function names,
   decorators, etc.
1. Update `CHANGELOG.md` with details about your change in a section titled
   `Unreleased`. If one does not exist, please create one.
1. You may merge the Pull Request once you have the sign-off of another
   developer, or if you do not have permission to do that, you may request the
   reviewer to merge it for you.

## Release process

> :memo: The versioning scheme we use is [SemVer](http://semver.org/). Note that until
> we agree we're ready for v1.0.0, we will not increment the major version.

1. Ensure all desired features are merged to `main` branch and `CHANGELOG.md` is updated.
1. Use `bump-my-version` to increase the version number in all needed places, e.g. to
   increase the minor version (`1.2.3` to `1.3.0`):

   ```plain
   bump-my-version bump minor
   ```

1. Push a tag on the new commit containing the version number, prefixed with `v`, e.g.
   `v1.3.0`.
1. [Create a new GitHub Release](https://github.com/nsidc/earthaccess/releases/new). We
   hand-curate our release notes to be valuable to humans. Please do not auto-generate
   release notes and aim for consistency with the GitHub Release descriptions from other
   releases.

> :gear: After the GitHub release is published, multiple automations will trigger:
>
> - Zenodo will create a new DOI.
> - GitHub Actions will publish a PyPI release.

> :memo: `earthaccess` is published to conda-forge through the
> [earthdata-feedstock](https://github.com/conda-forge/earthdata-feedstock), as this
> project was renamed early in its life. The conda package is named `earthaccess`.
