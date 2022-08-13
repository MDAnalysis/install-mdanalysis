# install-mdanalysis
Action to install MDAnalysis

## Basic usage

See the action.yaml for all inputs.


Examples:

The below workflow installs the develop version of MDAnalysis and MDAnalysisTests, using pip.
```yaml
steps:
- uses: actions/checkout@v3

- uses: actions/setup-python@v4
  with:
    python-version: 3.9

- uses: MDAnalysis/install-mdanalysis@main
  id: install-mdanalysis
  with:
    version: develop
    install-tests: true
    installer: pip
    shell: bash

- name: Check MDAnalysis version
  shell: bash
  run: |
    echo "MDAnalysis version: ${{ steps.install-mdanalysis.outputs.installed-version }}

```

Alternatively, you could use conda to install version 2.1.0:

```yaml
steps:
- uses: actions/checkout@v3

- name: Install conda Python 3.9
  uses: conda-incubator/setup-miniconda@v2
  with:
  python-version: 3.9
  add-pip-as-python-dependency: true
  architecture: x64
  mamba-version: "*"
  channels: conda-forge, defaults
  auto-update-conda: true
  show-channel-urls: true

- uses: MDAnalysis/install-mdanalysis@main
  id: install-mdanalysis
  with:
    version: "2.1.0"
    install-tests: true
    installer: conda  # or mamba
    shell: bash -l {0}

- name: Check MDAnalysis version
  shell: bash
  run: |
    echo "MDAnalysis version: ${{ steps.install-mdanalysis.outputs.installed-version }}

```


### Options

* version: this can be
  * "develop": this will pull the develop branch from GitHub
  * "latest": this will install the most recent release
  * "2.0.0" or a similar release number
* install-tests: `true` or `false`. Whether or not to install MDAnalysisTests
* installer: this can be "conda", "mamba", or "pip". The conda environments and Python should already be set up prior to installing.
* shell: We highly recommend using the login shell `"bash -l {0}"` if you are using a conda environment, and `"bash"` otherwise.


### Notes:

* Cycling this action while using `pip` and `conda` to repeatedly uninstall or reinstall MDAnalysis may lead to undefined behaviour. It is highly encouraged to keep Python environments separate between jobs to avoid this kind of issue.