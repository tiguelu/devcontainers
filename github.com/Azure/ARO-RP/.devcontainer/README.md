# ARO RP Dev Container

It automates the [steps to prepare your dev environment](https://github.com/Azure/ARO-RP/blob/master/docs/prepare-your-dev-environment.md) in a Fedora container for the [ARO-RP repo](https://github.com/Azure/ARO-RP/), plus part of the env setup through tasks and launch configurations.

It also contains the required VSCode extensions for the ARO-RP, the Golang debugging tools used by the Go extension and some other dev tools (oc, az, etc)

The username and uid of your container will match your username and uid on the host, although it can be changed in devcontainer.json.

When you open a terminal, VSCode will move you to ```/ARO-RP``` (where the workspace resides inside the container) and if an env file exists, it will be loaded.


## Tasks

If your workspace didn't already contain a tasks.json, the dev container will create its own in the workspace.

If your workspace already had a tasks.json file, it won't be changed. It can be merged with the dev container tasks.json, if desired, but any original comments will be lost. To do so:
```bash
$ cp /ARO-RP/.vscode/tasks.json /ARO-RP/.vscode/orig.tasks.json
$ jsonlint -Sf /ARO-RP/.vscode/orig.tasks.json | jq -s '.[0].tasks + .[1].tasks | {version: "2.0.0", tasks: .}' - ~/.vscode-server/data/Machine/tasks.json > /ARO-RP/.vscode/tasks.json
```


## Debug launch setup

The debugging tools for the Go extension are alrady installed and should work within the IDE.



