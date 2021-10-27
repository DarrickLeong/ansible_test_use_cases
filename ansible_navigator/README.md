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
    level: debug
  ```

## ansible-navigator as stdout mode
You are able to obtain a 'ansible-playbook like' output by running ansible-navigator as stdout mode.

  ```bash
  ansible-navigator run ping_test.yml -m stdout
  ```

## ansible-navigator commands
There are many commands that you can use with ansible-navigator here are a few to start off with.

  ```bash
  ansible-navigator
  ansible-navigator run ping_test.yml
  ansible-navigator doc ping
  ```
## ansible-navigator opening files on preferred editor
You are able to configure the settings of ansible-navigator to open an output of a task in a preferred editor. If no configuration is done the default edirot will be ``vi``.

**For vscode**
```yaml
editor:
  command: code -g {filename}:{line_number}
  console: false
```
Inspect the play by pressing ``0``, inspect the first task by pressing ``0`` again and issue the subcommand ``:open``