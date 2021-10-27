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

## Using execution environment from ansible-navigator

**Step 1**
- Login and Pull Ansible EE images from registry.redhat.io from ``podman``

```bash
$ podman login registry.redhat.io
Username: {REGISTRY-SERVICE-ACCOUNT-USERNAME}
Password: {REGISTRY-SERVICE-ACCOUNT-PASSWORD}
Login Succeeded!

$ podman pull registry.redhat.io/ansible-automation-platform***********/********
```
Check your images if it is avaliable in podman and ansible-navigator
```bash
$ podman images
$ ansible-navigator images
```
**Step 2**
- Update your image with the respective image name and tag
- change enable to ``true``

```yaml
execution-environment:
    container-engine: podman
    image: ee-supported-rhel8:2.0.1-6.1634243686
    enabled: true
```
Open ansible-navigator.yml and change enabled: false to enabled: true under the execution-environment settings block.

Notice that ansible-navigator knows that it should be using an execution environment but none are currently present. You should see a pull process happening now where an execution environment is being pulled from container registry. ansible-navigator can be configured in the same yaml file to pull from your own Private Automation Hub.

Your test.yml file should have executed successfully. You can now use ansible-navigator to inspect this execution environment by issuing the :collections subcommand.

While inspecting collections, there is a module in the ansible.utils collection called fact_diff. Locate the author of this module and remember the github handle associated with this person.