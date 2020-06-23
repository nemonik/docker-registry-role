# docker-registry Ansible role

![Basic role syntax check](https://github.com/nemonik/docker-registry-role/workflows/Basic%20role%20syntax%20check/badge.svg)

An Ansible role for ensuring the configuration of a docker regsitry.  

If `registry_deploy_via` is set to `kubernetes` (The default.):
- If `registry` is set to `yes` (The default.), a private registry is spun up on `registry_port` (`5000` is the default.) on a private IP (`192.168.0.200` is the default.)
- If `passthrough_regsitry` is set to `yes` (The default is `no`.), a passthrough registry is spun up on on `passthrough_registry_port` (`5000` is the default.) of a private IP (`192.168.0.201` is the default.)

If `registry_deploy_via` is set to `docker-compose`:
- If `registry` is set to `yes` (The default.), a private registry is spun up on `registry_port` (`5000` is the default.) of the host
- If `passthrough_regsitry` is set to `yes` (The default is `no`.), a passthrough registry is spun up on on `passthrough_registry_port` (`5001` is the default.) of the host

## Requirements

Requires Kubernetes and MetalLB installed, when `registry_deploy_via` is set to `kubernetes`.  Otherwise, requires Docker and Docker Compose, when `registry_deploy_via` is set to `docker-compose`..

## Role Variables

| Variable                  | Required | Default      | Choices                      | Comments                                                                                   |
|---------------------------|----------|--------------|------------------------------|--------------------------------------------------------------------------------------------|
| docker_timeout            | yes      | 300          | Integer value                | Number of seconds before docker pull timeout                                               |
| docker_retries            | yes      | 60           | Integer value                | Number of tries for docker pull                                                            |
| docker_delay              | yes      | 10           | Integer value                | Delay in seconds between pull retries                                                      |
| default_retries           | yes      | 60           | Integer value                | Default number of retries                                                                  |
| default_delay             | yes      | 60           | Integer value                | Default delay in seconds between retries                                                   |
| registry_version          | yes      | 2.7.1        | matches tag                  | Docker registry version                                                                    |
| registry                  | no       | yes          | yes or no                    | spin up a private regsitry                                                                 |
| registry_host             | yes      | not defined  | IP address                   | IP address if deployed via Kubernetes otherwise use the IP of the host                     |
| registry_port             | yes      | 5000         | Integer                      | Port for the registry                                                                      |
| passthrough_registry      | no       | yes          | yes or no                    | Also, create a passthrough registry                                                        |
| passthrough_registry_host | no       | not defined  | IP address                   | IP address if deployed via Kubernetes otherwise use the IP of the host                     |
| passthrough_registry_port | no       | 5000 or 5001 | Integer value                | Port for passthrough registry. 5001, if via docker-compose. Otherwise 5000, if Kuberenetes |
| registry_deploy_via       | no       | kubernetes   | kubernetes or docker-compose | How to spin up                                                                             |
| images_cache_path         | no       | not defined  | Path                         | Path to folder used to cache saved Docker images                                           |

## Example Playbook

An example can be found used in my Hands-on DevOps course's [ansible/master-playbook.yml](https://github.com/nemonik/hands-on-DevOps/blob/master/ansible/master-playbook.yml).

```
- hosts: masters
  remote_user: vagrant
  roles:
    - common
    - docker
    - docker-compose
    - docker-registry
```

The above Ansible playbook uses my

- [Common role](https://github.com/nemonik/common-role) to configure the instance past the base CentOS 7, Alpine 3.10 or Ubuntu Bionic image
- [Docker role](https://github.com/nemonik/docker-role) to install and configure Docker
- [Docker-compose role](https://github.com/nemonik/docker-compose-role) to install Docker-compose
- [Docker Registry role](https://github.com/nemonik/docker-registry-role) to install a private Docker registry

For more information and to see this role put into action checkout my [Hands-on DevOps class](https://github.com/nemonik/hands-on-DevOps) project.

## License

3-Clause BSD License

## Author Information

Michael Joseph Walsh <mjwalsh@nemonik.com>
