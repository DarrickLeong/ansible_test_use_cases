# ansible_navigator Test Use Cases
Test Use Cases for ansible navigator.

See this link for ansible-navigator settings: https://ansible-navigator.readthedocs.io/en/latest/settings/

See this link for ansible-navigator guide: https://access.redhat.com/documentation/en-us/red_hat_ansible_automation_platform/2.0-ea/html-single/ansible_navigator_creator_guide/index

## ansible-navigator settings
You are able to configure the settings of ansible-navigator what you see below are three main parameters:
- execution-environment
- playbook-artifact
- logging


```yaml
---
ansible-navigator:
  execution-environment:
    container-engine: podman
    image: ee-supported-rhel8:2.0.0
    enabled: false

  playbook-artifact:
    save-as: /home/darrick/ansible_test_use_cases/playbook-artifacts/{playbook_name}-artifact-{ts_utc}.json

  logging:
    file: /home/darrick/ansible-navigator-log/ansible-navigator.log
    level: debug
  ```

## ansible-navigator as stdout mode
You are able to obtain a 'ansible-playbook like' output by running ansible-navigator as stdout mode.

  ```bash
  $ ansible-navigator run ping_test.yml -m stdout
  ```

## ansible-navigator commands
There are many commands that you can use with ansible-navigator here are a few to start off with.

  ```bash
  $ ansible-navigator
  $ ansible-navigator run ping_test.yml
  $ ansible-navigator doc ping
  ```
## ansible-navigator opening files on preferred editor
You are able to configure the settings of ansible-navigator to open an output of a task in a preferred editor. If no configuration is done the default editor will be ``vi``.

There is an option to see the output on the console instead of an editor as well.

**For vscode**
```yaml
editor:
  command: code -g {filename}:{line_number}
  console: false
```
Inspect the play by pressing ``0``, inspect the first task by pressing ``0`` again and issue the subcommand ``:open``

## ansible-navigator Using execution environment from podman

**Step 1**
- Login and Pull Ansible EE images from registry.redhat.io from ``podman``

```bash
$ podman login registry.redhat.io
Username: {REGISTRY-SERVICE-ACCOUNT-USERNAME}
Password: {REGISTRY-SERVICE-ACCOUNT-PASSWORD}
Login Succeeded!

```

Check your images if it is avaliable in podman and ansible-navigator
```bash
$ podman images
$ ansible-navigator images
```
**Step 2**
- Update your image with the respective image name
- change enable to ``true``

```yaml
execution-environment:
    container-engine: podman
    image: ansible-automation-platform-20-early-access/ee-supported-rhel8
    enabled: true
```


**Step 3**
You may add in a ```wait_for``` module to show the playbook has been executed as a container on podman

```yaml
wait_for:
  timeout: 30
```
Now that ```ansible-navigator``` knows that it should be using an execution environment you have two options:

**Option 1**
- Pull the image directly into podman
```bash
$ podman pull registry.redhat.io/ansible-automation-platform-20-early-access/ee-supported-rhel8
$ ansible-navigator run ping_test.yml
```
**Option 2**
- If the respective image are currently not present.
- Directly run the playbook and let ```ansible-navigator``` initiate a pull process, where an execution environment is being pulled from a container registry.
```bash
$ ansible-navigator run ping_test.yml
```
```bash
$ watch podman ps

CONTAINER ID  IMAGE                                                                                                 COMMAND               CREATED         STATUS             PORTS   NAMES
fce3314a3e05  registry.redhat.io/ansible-automation-platform-20-early-access/ee-supported-rhel8:latest  ansible-playbook ...  10 seconds ago  Up 10 seconds ago          ansible_runner_f4a4e932-013b-4dd3-8487-f7e45f27a40
```
You may continue to use ansible-navigator to inspect this execution environment by issuing the ```:collections``` subcommand.

## ansible-navigator Using execution environment from automation hub
* ansible-navigator can be configured in the same yaml file to pull from your own Private Automation Hub.
* see this link for the documentation: https://access.redhat.com/documentation/en-us/red_hat_ansible_automation_platform/2.0-ea/html-single/managing_containers_in_private_automation_hub/index

**Step 1**
- Push images from podman to Automation hub
```bash
$ podman images

$ podman tag registry.redhat.io/ansible-automation-platform-20-early-access/ee-supported-rhel8:latest [automation-hub-url]/ee-supported-rhel8:latest

$ podman login -u=[username] -p=[password] [automation-hub-url]
Login Succeeded!

$ podman push [automation-hub-url]/[container image name]
```
**Step 2**
- Edit container registry configuration file with the DNS of the registry
- If registry is insecure placed in under ```[registries.insecure]```
```bash
$ sudo vim /etc/containers/registries.conf

[registries.search]
registries = ['registry.access.redhat.com', 'registry.redhat.io', 'docker.io', 'automation-hub-url']
```
**Step 3**
To see if it is working
- Clear the pushed imaged from podman as we would like ansible-navigator to pull the image instead

```bash
$ podman rmi [automation-hub-url]/[container image name]
```

- Edit ```ansible-navigator.yml``` file to the image url in automation hub
```bash
$ vim ansible-nagivator.yml
```
```yaml
execution-environment:
    container-engine: podman
    image: [automation-hub-url]/[container image name]
    enabled: true
```

- Run Playbook
```bash
$ ansible-navigator run ping_test.yml
```
