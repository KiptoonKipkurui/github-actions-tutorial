# github-actions-tutorial

## Github Actions
GitHub Actions is a continuous integration and continuous delivery (CI/CD) platform that allows you to automate your build, test, and deployment pipeline. 

Github provides Windows, linux and MacOs virtual machines to run workflow.


### Worflow:
Configurable automated process.
Defined by Yaml
defined in .github/workflows (you can have multiple workflows)


### Events:
Specific activity in the repository that triggers a workflow eg pull requests, issues, commits
or on a schedule [all events](https://docs.github.com/en/actions/using-workflows/events-that-trigger-workflows)


### Jobs

Set of steps in a workflow that execute on the same runner. Either a shell script or action
Executed in order and dependent on one another.
Data can be shared between actions
Jobs can have dependencies on other Jobs


### Actions

Custom application for github action that performs complex but repeated tasks
You can write your own actions or find some in [Github MarketPlace](https://github.com/marketplace?type=actions)

### Runners

Server that runs workflows. Each workflow runs on a newly configured VM

Event->Workflow->Jobs->Runner(Container)->Action


Example Workflow installs bats and tests the bats version

```py
name: learn-github-actions
on: [push]
jobs:
  check-bats-version:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: '14'
      - run: npm install -g bats
      - run: bats -v
```

Referencing action from market place in your action

Example 

## actions/setup-python

This action provides the following functionality for GitHub Actions users:

* Installing a version of Python or PyPy and (by default) adding it to the PATH
* Optionally caching dependencies for pip, pipenv and poetry
* Registering problem matchers for error output

The action.yml defines the parameters and optional parameters used by each action

[Action.yml](https://github.com/actions/setup-python/blob/main/action.yml)

``` sh

name: Python package

on: [push]

jobs:
  build:

    runs-on: ubuntu-latest
    strategy:
      matrix:
        python-version: ["3.7", "3.8", "3.9", "3.10"]

    steps:
      - uses: actions/checkout@v3
      - name: Set up Python ${{ matrix.python-version }}
        uses: actions/setup-python@v4
        with:
          python-version: ${{ matrix.python-version }}
      - name: Install dependencies
        run: |
          python -m pip install --upgrade pip
          pip install flake8 pytest
          if [ -f requirements.txt ]; then pip install -r requirements.txt; fi
      - name: Lint with flake8
        run: |
          # stop the build if there are Python syntax errors or undefined names
          flake8 . --count --select=E9,F63,F7,F82 --show-source --statistics
          # exit-zero treats all errors as warnings. The GitHub editor is 127 chars wide
          flake8 . --count --exit-zero --max-complexity=10 --max-line-length=127 --statistics
      - name: Test with pytest
        run: |
          pytest
```
