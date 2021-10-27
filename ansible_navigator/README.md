# ansible_navigator Test Use Cases
Test Use Cases for ansible navigator
See this link for ansible-navigator settings: https://ansible-navigator.readthedocs.io/en/latest/settings/

## ansible-navigator settings
You are able to configure the settings of ansible-navigator what you see below are three parameters:
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
You are able to obtain a 'ansible-playbook like' output by running ansible-navigator as stdout mode

  ```bash
  ansible-navigator run ping_test.yml -m stdout
  ```

