# semantic-release-pypi
semantic-release plugin to publish a python package to PyPI

| Step | Description
| ---- | -----------
| ```verifyConditions``` | <ul><li>verify the environment variable ```PYPI_TOKEN```</li><li>verify ```PYPI_TOKEN``` is authorized to publish on the specified repository</li><li>verify that `version` is not set inside `setup.py` (**version will be set in `setup.cfg`**)</li><li>check if the packages `setuptools`, `wheel` and `twine` are installed</li></ul>
| ```prepare``` | Update the version in ```setup.cfg``` and create the distribution packages
| ```publish``` | Publish the python package to the specified repository (default: pypi)

# Configuration

## Environment variables

| Variable | Description | Required | Default
| -------- | ----------- | ----------- | -----------
| ```PYPI_TOKEN``` | [API token](https://test.pypi.org/help/#apitoken) for PyPI | true | 
| ```PYPI_USERNAME``` | Username for PyPI | false | ```__token__```
| ```PYPI_REPO_URL``` | Repo URL for PyPI | false | See [Options](#options)

## Usage

The plugin can be configured in the [**semantic-release** configuration file](https://github.com/semantic-release/semantic-release/blob/master/docs/usage/configuration.md#configuration). Here is a minimal example:

```json
{
  "plugins": [
    "@semantic-release/commit-analyzer",
    "@semantic-release/release-notes-generator",
    "semantic-release-pypi",
  ]
}
```

Note that this plugin modifies `setup.cfg` to point to the newly released version. While the plugin 
will work without any `setup.cfg` in the repository, if you want to save a hard-coded version in `setup.cfg`
then you will want to commit the change using the `@semantic-release/git` plugin:

```json
{
  "plugins": [
    "@semantic-release/commit-analyzer",
    "@semantic-release/release-notes-generator",
    "semantic-release-pypi",
    [
      "@semantic-release/git",
      {
          "message": "chore(release): ${nextRelease.version} [skip ci]\n\n${nextRelease.notes}",
          "assets": ["setup.cfg"]
      }
    ]
  ]
}
```

A full example using Github Actions can be found in the repo [semantic-release-pypi-example](https://github.com/abichinger/semantic-release-pypi-example).

## Options

| Option | Type | Default | Description
| ------ | ---- | ------- | -----------
| ```setupPy``` | str | ```./setup.py``` | location of ```setup.py```
| ```distDir``` | str | ```dist``` | directory to put the source distribution archive(s) in, relative to the directory of ```setup.py```
| ```repoUrl``` | str | ```https://upload.pypi.org/legacy/``` | The repository (package index) to upload the package to.
| ```pypiPublish``` | bool | ```true``` | Whether to publish the python package to the pypi registry. If false the package version will still be updated.
| ```gpgSign``` | bool | ```false``` | Whether to sign the package using GPG. A valid PGP key must already be installed and configured on the host.
| ```gpgIdentity``` | str | ```null``` | When ```gpgSign``` is true, set the GPG identify to use when signing files. Leave empty to use the default identity.

## Development

### Pre-requisites

- pyenv >= 2.1.0

```shell
source init.sh
```

### Contribute

- Fork from this repository
- Run `source init.sh`
- Make sure your code passes all unit tests by running `yarn test`
- Issue a PR