# ansible_navigator Test Use Cases
Test Use Cases for ansible navigator
See this link for ansible-navigator settings: https://ansible-navigator.readthedocs.io/en/latest/settings/

## ansible-navigator as stdout mode
You are able to obtain a 'ansible-playbook like' output by running ansible-navigator as stdout mode

  ```bash
  ansible-navigator run ping_test.yml -m stdout
  ```

