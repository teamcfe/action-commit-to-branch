# Commit and push to remote branch

This action will commit and push to a specific remote branch. 

## Inputs

### `branch`

**Required** The branch name of the branch to commit and push to. 

If the branch does not already exist, it will be created for you. 

### `commit_message`

Custom commit message. **default** "Automated commit from action""

### `GITHUB_TOKEN`
You must create a Personal Access Token otherwise this action will not run.

### `SOURCE_BRANCH`
If you want to override your `branch` add a `SOURCE_BRANCH` to your `env` (environment variables). This step is useful for testing the `SOURCE_BRANCH`. 


## Example usage
```
- name: Push from dev to prod branch.
  uses: Automattic/action-commit-to-branch@master
  with:
    branch: 'dev'
    commit_message: 'Automated tests for main.'
  env:
    GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }} # Required
    SOURCE_BRANCH: prod
```


## Full Github Action example
This uses Django's built in tests:

```
name: Python-Django application

on: 
  push:
      branches:
         - main

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v1
    - name: Set up Python 3.6
      uses: actions/setup-python@v1
      with:
        python-version: 3.6
    - name: Install dependencies
      run: |
        python -m pip install --upgrade pip
        pip install -r requirements.txt
    - name: Run migrations
      run: python manage.py migrate
    - name: Run tests
      run: python manage.py test
    - name: Push main branch into production
      uses: teamcfe/action-commit-to-branch@main
      with:
        branch: 'production-v1'
        commit_message: 'Release production version'
      env:
        GITHUB_TOKEN: ${{ secrets.PERSONAL_ACCESS_TOKEN }}
        SOURCE_BRANCH: main
```
