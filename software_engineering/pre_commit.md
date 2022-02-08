# pre-commit
`pre-commit` is a framework that uses Git hooks to go through checklists before the commit. 
This precheck can help to review if your code is meeting the coding standards or if the commiting file is too large.
And the commit will only be made when all pre-defined conditions are met. 
So that one can worry less on the formatting and focus more on building the code.

## What is Git Hooks?
Git hooks are the scripts triggered by Git when certain actions take place, such as commit and merge.
They contain customised actions to be executed before or after when certian Git actions are used. 

There are 2 ways the hooks can be implemented; Client-side and Server-side.<br>
Client-side hooks are called locally by actions such as commit, whereas, server-side hooks are called on the server when receiving a push.<br>
This framework helps to build the client-side hooks to trigger before the commit.

## How to Set Up
To implement `pre-commit`, the following steps are required.

### Installation
The framework can be installed with the following lines.

Using `pip`

    $ pip install pre-commit

Using `homebrew`

    $ brew install pre-commit

Using `conda`

    $ conda install -c conda-forge pre-commit

### Configuration
Before activating `pre-commit`, configuration needs to be defined with 
the sources of the `pre-commit` hooks/plugins.<br>

They are defined in the configuration file, `.pre-commit-config.yaml` in the target repository,
and follows the format below. Although, the format is written for Python, `pre-commit` works
for any programming language.

    repos:
    -   repo: https://github.com/pre-commit/pre-commit-hooks
        rev: v4.1.0
        hooks:
        -   id: trailing-whitespace
    -   repo: https://github.com/psf/black
        rev: 22.1.0
        hooks:
        - id: black
          args: [--config=pyproject.toml]
    -   repo: https://github.com/PyCQA/flake8
        rev: 4.0.1
        hooks:
        -   id: flake8
            args: [--config=.flake8]
    -   repo: https://github.com/asottile/reorder_python_imports
        rev: v2.7.1
        hooks:
        -   id: reorder-python-imports
            args: [--py37-plus, --add-import, 'from __future__ import annotations']
            exclude: ^testing/resources/python3_hooks_repo/

|Syntax|Description|
|------|-----------|
|repo|repository url to git clone from|
|rev|repository revision/release tag|
|hooks|hooks to implement from the repository|
|id|identification for the hooks
|args|list of parameters to pass to the hooks|
|exclude|file exclusion pattern|
<br>

In the example, the following hooks are configured:
- trailing-whitespace: `pre-commit` hook to remove trailing whitespaces
- `black`: Python style formatter 
  - code style: https://black.readthedocs.io/en/stable/the_black_code_style/current_style.html
  - Key differences to PEP 8 includes: 
    - 88 characters for a line instead of 79 characters
    - Use of double quotes instead of single quotes
- `flake8`: PEP 8 compliance checker
- reorder_python_imports: reordering tool for Python imports
- Note that `black` and `flake8` hooks contain configuration arguments which will be discussed next

### Compatibility between Black & Flake8
In the example, `black` is used to format the code style, followed by 'flake8' to further 
comply with PEP 8 standards for non-style linting concerns.<br>
Although `black` is compliant with PEP 8, there are some
opiniated formatting that may clash with `flake8`. <br>
Hence, it is important to amend the configuration to prevent spewing errors. 

As per `black` repository, the following configuration can be set for `flake8` to
avoid collison by creating a configuration file, `.flake8` file in the target repository.
(source: https://github.com/psf/black/blob/main/.flake8)

    [flake8]
    ignore = E203, E266, E501, W503
    # line length is intentionally set to 80 here because black uses Bugbear
    # See https://black.readthedocs.io/en/stable/the_black_code_style/current_style.html#line-length for more details
    max-line-length = 80
    max-complexity = 18
    select = B,C,E,F,W,T4,B9

In addition, a configuration file, `pyproject.toml` can also be set for `black` to ensure it ignores certain files and the string quote normalisation (converting " to ') is disabled.<br>
The string quote normalisation is upto personal preference. 

    [tool.black]
    line-length = 88
    skip-string-normalization = 1
    include = '\.pyi?$'
    exclude = '''
    /(
        \.git
    | \.hg
    | \.mypy_cache
    | \.tox
    | \.venv
    | _build
    | buck-out
    | build
    | dist
    )/
    '''

### Usage
After the configuration is done, `pre-commit` needs to be installed into the git hooks of the 
target repository. Ensure all relelvant configuration files are in the same target repository
directory. 

    $ pre-commit install

This command will enable `pre-commit` to be triggered before the commit for the target repository.

To manually execute the hooks, the follow commands can be used. When `pre-commit` runs, it will 
download, install and run the hooks specified in the `.pre-commit-config.yaml`.

    $ pre-commit run --all-files
    $ pre-commit run <hook_id> # for specific hooks

## Warning
If the repository is located in the network drive using UNC (Universal Naming Convension), 
Git may not recognise the path and `pre-commit` may not be installed properly. 

