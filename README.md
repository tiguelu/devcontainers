# Dev Containers for VSCode

## General quickstart for RHEL/Fedora

1. Install podman if you don't have it: ```sudo dnf install podman```
1. Install VSCode using the official MSFT repo (don't use the Flatpak from RHEL/Fedora or the Snap versions, as these need a lot of manual tuning to work with Remote Containers). Follow these [steps for RHEL/Fedora](https://code.visualstudio.com/docs/setup/linux#_rhel-fedora-and-centos-based-distributions).
1. Install MSFT [Remote Containers extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers).
1. Configure the extension to use podman:
    1.  Press ```Ctrl + ,``` to open settings.
    1. Type ```docker path```
    1. In the **Remote > Containers: Docker Path** setting replace ```docker``` with ```podman```.


## Note for RHEL CSB
In RHEL CSB, the container build will likely fail if your user receives a very high UID. This can break VSCode attempt to update the remote UID of the container. This happens because the default number of subuid/subgid allocated for rootless containers (typically 65536) is "too small" to allow podman to map a container UID&nbsp;>&nbsp;65536

When this occurs, you need to modify /etc/subuid and /etc/subgid to allocate enough ids for podman mappings. This prevents the errors for containers run with --userns=keep-id

1. Run id to see your assigned UID and GID:
    ```
    $ id
    uid=4194305(someuser) gid=4194305(someuser) groups=4194305(someuser),3(sys),100(users),983(libvirt) context=unconfined_u:unconfined_r:unconfined_t:s0-s0:c0.c1023
    ```
1. ```sudo vi /etc/subuid``` and change the line for your username, so that the initial subuid value (first numerical value) equals your UID + 1, and the count of allocatable ids (second numerical value) matches your UID. For the user above:
    ```
    $ cat /etc/subuid
    splunk:100000:65536
    someuser:4194306:4194305
    ```
1. ```sudo vi /etc/subgid``` and repeat the previous step for your user's entry, but now using your GID from step 1:
    ```
    $ cat /etc/subgid
    splunk:100000:65536
    someuser:4194306:4194305
    ```
1. Run ```podman system migrate```


## Dev containers in this repo

Clone this repo or a fork, to use the dev containers it contains. The tree in the repo defines the paths of remote repos (as per [alternative repo config folders](https://code.visualstudio.com/docs/remote/create-dev-container#_alternative-repository-configuration-folders)).

Add the full path of the locally cloned repo in your host to the global settings: **Remote > Containers: Repository Configuration Paths**.

Specific Dev Containers doc:
* [ARO-RP](github.com/Azure/ARO-RP/)


## Links
* [Overview and benefits of remote dev envs](https://code.visualstudio.com/docs/remote/remote-overview)
* [Remote Dev Containers docs](https://code.visualstudio.com/docs/remote/containers)
* [Advanced Dev Containers topics](https://code.visualstudio.com/remote/advancedcontainers/overview)
