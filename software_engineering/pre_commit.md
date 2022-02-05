# pre-commit
pre-commit is a framework that uses Git hooks to go through checklists before the commit. 
This precheck can help to review if your code is meeting the coding standards or if the commiting file is too large.
So that one can worry less on the formatting and focus more on building the code.

## Git Hooks
Git hooks are the scripts triggered by Git when certain actions take place, such as commit and merge.
They contain customised actions to be executed before or after when certian Git actions are used. 

There are 2 ways the hooks can be implemented; Client-side and Server-side.<br>
Client-side hooks are called locally by actions such as commit, whereas, server-side hooks are called on the server when receiving a push.<br>
This framework helps to build the client-side hooks to trigger before the commit.

## How to Set Up

