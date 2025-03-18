# Github Actions Intro

## Guide  
Intro steps are made in branches to develop a new idea, one at a time.

- 00: [Starting Branch](https://github.com/BlueBastion/DEV-github-actions-example/tree/00-start)
- 01: [Context Variables](https://github.com/BlueBastion/DEV-github-actions-example/tree/01-contexts)
- 02: [Running a Script](https://github.com/BlueBastion/DEV-github-actions-example/tree/02-running-a-script)
- 03: [Prebuilt Actions](https://github.com/BlueBastion/DEV-github-actions-example/tree/03-prebuilt-actions)

# Prebuilt Actions
We added two workflows in this branch in order to highlight the use of prebuilt actions.  
[A Failing Workflow]()  
[Proper Workflow]()

## Key Concepts

### runner-setup
    Runners must be setup every time the action runs. Without the proper setup, commands will be missing.

### prebuilt-actions
The signature for a prebuilt action is `[owner]/[action-name]@[version]`  
the version may be explicitly set with a sha signature if desired. If `@v1` is selected, 
any minor version changes will be honored.  For example. @v1.1.0 will be run when it is available, 
however @v2.0.0 will not.

- [*`actions/checkout@v4`*](https://github.com/marketplace/actions/checkout) : 
    We've seen this action before. It'll check out code and make it available to the Action
- [*`actions/setup-python@v5`*](https://github.com/marketplace/actions/setup-python) : 
    Installs a python interpreter inside the runner. 
    This must be performed or else python will not be installed in the runner machine.
- [*`ncipollo/release-action@v1`*](https://github.com/marketplace/actions/create-release): 
    Creates a github release and uploads artifacts from the poetry build process.

### GitHub Release
A release is generally used for binaries or artifacts created by a build. 
Often, you will see python wheels in a release with multiple architectures.

### Strategy Matrix
This is something we will introduce in the actions covered in this section.  
However, we will explain it later. For now, you can consider them as simple variables

### `$GITHUB_PATH` "Magic" Variable
Provides a default for the PATH variable provided to the bash process called by `run:`

### `secrets.GITHUB_TOKEN` Secret Variables & `GITHUB_TOKEN`

## Overview
Both workflows define their first and only job as follows
```yaml
name: [name]

on:
  push:
jobs:
  build_release:
    strategy:
      matrix:
        python-version: [ "3.12" ]
    runs-on: ubuntu-latest
    steps:
        # ...
```

### Failing Workflow
In [this workflow](), we fail to use `actions/setup-python`
```yaml
    # ...
    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Install Poetry
        run: |
          curl -sSL https://install.python-poetry.org | python - -y

      - name: Update PATH
        run: echo "$HOME/.local/bin" >> $GITHUB_PATH

      - name: Build project for distribution
        run: poetry test

      - name: Check Version
        id: check-version
        run: |
          [[ "$(poetry version --short)" =~ ^[0-9]+\.[0-9]+\.[0-9]+$ ]] || echo prerelease=true >> $GITHUB_OUTPUT
```

### Successful workflow
In this workflow, we remember to use the setup-python action, therefore, this action should complete.  

Take note of the `with` directive after `uses:` in `setup-python@v5`.  This is how arguments are passed to prebuilt actions.
```yaml
    # ...
    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      # Setup Python
      - name: Choose python
        uses: actions/setup-python@v5
        with:
          python-version: ${{ matrix.python-version }}

      - name: Install Poetry
        run: |
          curl -sSL https://install.python-poetry.org | python - -y

      - name: Update PATH
        run: echo "$HOME/.local/bin" >> $GITHUB_PATH

      - name: Build project for distribution
        run: poetry build

      - name: Check Version
        id: check-version
        run: |
          [[ "$(poetry version --short)" =~ ^[0-9]+\.[0-9]+\.[0-9]+$ ]] || echo prerelease=true >> $GITHUB_OUTPUT

      - name: Create Release
        uses: ncipollo/release-action@v1
        with:
          artifacts: "dist/*"
          token: ${{ secrets.GITHUB_TOKEN }}
          draft: false
          prerelease: steps.check-version.outputs.prerelease == 'true'
```
