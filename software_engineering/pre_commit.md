# pre-commit
pre-commit is a Python framework that uses Git hooks to go through checklists before the commit. 
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
To implement pre-commit, the following steps are required.
1. Install pre-commit framework<br>
Using pip: $ pip install pre-commit<br>
Using homebrew: $ brew install pre-commit<br>
Using conda: $ conda install -c conda-forge pre-commit<br>
2. Create pre-commit configuration file<br>
Create `.pre-commit-config.yaml` in the local repository


