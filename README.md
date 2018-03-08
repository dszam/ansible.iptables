# Ansible Role: iptables

[![Build Status](https://travis-ci.org/sbaerlocher/ansible.iptables.svg?branch=master)](https://travis-ci.org/projectgroup/ansible.iptables) [![license](https://img.shields.io/github/license/mashape/apistatus.svg)](https://sbaerlo.ch/licence) [![Ansible Galaxy](http://img.shields.io/badge/ansible--galaxy-iptables-blue.svg)](https://galaxy.ansible.com/sbaerlocher/iptables)

## Description

Installs and configures iptables.

## Installation

```bash
ansible-galaxy install sbaerlocher.iptables
```

## Requirements

This role requires Ansible 2 or higher.

## Role Variables

| Name                             | Default                                                                                               | Description                                 |
|:---------------------------------|:------------------------------------------------------------------------------------------------------|:--------------------------------------------|
| iptables_filter_input_policy     | drop                                                                                                  | IPv4 default filter input policy            |
| iptables_filter_forward_policy   | drop                                                                                                  | IPv4 default filter forward policy          |
| iptables_filter_output_policy    | accept                                                                                                | IPv4 default filter output policy           |
| iptables_filter_rules            | [{protocol: tcp, source_address: 0.0.0.0/0, destination_port: 22, comment: OpenSSH, target: accept }] | Array of filter rules represented as hashes |
| iptables_nat_prerouting_policy   | accept                                                                                                | IPv4 default nat prerouting policy          |
| iptables_nat_input_policy        | accept                                                                                                | IPv4 default nat input policy               |
| iptables_nat_output_policy       | accept                                                                                                | IPv4 default nat output policy              |
| iptables_nat_postrouting_policy  | accept                                                                                                | IPv4 default nat postrouting policy         |
| iptables_nat_rules               | []                                                                                                    | Array of nat rules represented as hashes    |
| iptables6_filter_input_policy    | drop                                                                                                  | IPv6 default filter input policy            |
| iptables6_filter_forward_policy  | drop                                                                                                  | IPv6 default filter forward policy          |
| iptables6_filter_output_policy   | accept                                                                                                | IPv6 default filter output policy           |
| iptables6_nat_prerouting_policy  | accept                                                                                                | IPv6 default nat prerouting policy          |
| iptables6_nat_input_policy       | accept                                                                                                | IPv6 default nat input policy               |
| iptables6_nat_output_policy      | accept                                                                                                | IPv6 default nat output policy              |
| iptables6_nat_postrouting_policy | accept                                                                                                | IPv6 default nat postrouting policy         |

## Dependencies

None

## Example Playbook

```yml
- hosts: all
  roles:
     - sbaerlocher.iptables
```

Install and configure iptables to disallow ICMP, allow OpenSSH and HTTP

```yaml
- hosts: all
  vars:
    iptables_filter_rules:
      - chain: input
        protocol: tcp
        source_address: 0.0.0.0/0
        destination_port: 22
        comment: OpenSSH
        target: accept
      - chain: input
        protocol: tcp
        source_address: 0.0.0.0/0
        destination_port: 80
        comment: HTTP
        target: accept
  roles:
    - sbaerlocher.iptables
```

Install and configure iptables with a port forward rule for HTTP

```yaml
- hosts: all
  vars:
    iptables_filter_rules:
      - chain: input
        protocol: tcp
        source_address: 0.0.0.0/0
        destination_port: 80
        comment: HTTP
        target: accept
    iptables_nat_rules:
      - chain: prerouting
        protocol: tcp
        destination_port: 80
        target: dnat
        to_destination: 192.168.88.88
        to_port: 8080
  roles:
    - sbaerlocher.iptables
```

## Changelog

### 2.0

* new strucktur
* new tests

### 1.0

* Initial release

## Author

* [Simon Bärlocher](https://sbaerlocher.ch)

## License

This project is under the MIT License. See the [LICENSE](https://sbaerlo.ch/licence) file for the full license text.

## Copyright

(c) 2018, Simon Bärlocher