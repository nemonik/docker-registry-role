# docker-registry Ansible role

![](https://github.com/nemonik/docker-registry-role/workflows/Basic%20role%20syntax%20check/badge.svg)

An Ansible role for ensuring the configuration of a docker regsitry.

## Requirements

Requires Kubernetes and MetalLB installed.

## Role Variables

| Variable                | Required | Default               | Choices             | Comments                                         |
|-------------------------|----------|-----------------------|---------------------|--------------------------------------------------|
| http_proxy              | no       | not defined           | http proxy setting  | patches registry image for http_proxy            |
| https_proxy             | no       | not defined           | https proxy setting | patches registry image for https_proxy           |
| no_proxy                | no       | not defined           | no_proxy setting    | patches registry image for no_proxy              |
| docker_timeout          | yes      | 300                   | Integer value       | number of seconds before docker pull timeout     |
| docker_retries          | yes      | 60                    | Integer value       | number of tries for docker pull                  |
| docker_delay            | yes      | 10                    | Integer value       | delay in seconds between pull retries            |
| default_retries         | yes      | 60                    | Integer value       | default number of retries                        |
| default_delay           | yes      | 60                    | Integer value       | default delay in seconds between retries         |
| registry_version        | yes      | 2.7.1                 | matches tag         | docker registry version                          |
| registry_host           | yes      | 192.168.0.201         | Private IP address  | IP address of the registry                       |
| registry_port           | yes      | 5000                  | Integer             | Port for the registry                            |
| images_cache_path       | no       | /vagrant/cache/images | Path                | Path to folder used to cache saved Docker images |

## Example Playbook

An example can be found used in my Hands-on DevOps course's [master-playbook.yml](https://github.com/nemonik/hands-on-DevOps/blob/master/ansible/master-playbook.yml).

```
- hosts: masters
  remote_user: vagrant
  roles:
    - k3s-server
    - metallb
    - docker-registry
```

The above Ansible playbook uses my [K3s-server role](https://github.com/nemonik/k3s-server-role) to install Lightweight Kubernetes (K3s) and my [metallb role](https://github.com/nemonik/metallb-role) to install MetalLB.

For more information and to see this role put into action checkout my [Hands-on DevOps class](https://github.com/nemonik/hands-on-DevOps) project.

## License

3-Clause BSD License

## Author Information

Michael Joseph Walsh <mjwalsh@nemonik.com>
