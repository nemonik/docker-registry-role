# docker-registry Ansible role

![Basic role syntax check](https://github.com/nemonik/docker-registry-role/workflows/Basic%20role%20syntax%20check/badge.svg)

An Ansible role for ensuring the configuration of a docker regsitry.  

If `registry_deploy_via` is set to `kubernetes` (The default.):
- If `registry` is set to `yes` (The default.), a private registry is spun up on `registry_port` (`5000` is the default.) on a private IP (`192.168.0.201` is the default.)
- If `passthrough_regsitry` is set to `yes` (The default is `no`.), a passthrough registry is spun up on on `passthrough_registry_port` (`5000` is the default.) of a private IP (`192.168.0.202` is the default.)

If `registry_deploy_via` is set to `docker-compose`:
- If `registry` is set to `yes` (The default.), a private registry is spun up on `registry_port` (`5000` is the default.) of the host
- If `passthrough_regsitry` is set to `yes` (The default is `no`.), a passthrough registry is spun up on on `passthrough_registry_port` (`5001` is the default.) of the host

## Requirements

Requires Kubernetes and MetalLB installed, when `registry_deploy_via` is set to `kubernetes`.  Otherwise, requires docker-compose.

## Role Variables

| Variable                  | Required | Default               | Choices                      | Comments                                                                                   |
|---------------------------|----------|-----------------------|------------------------------|--------------------------------------------------------------------------------------------|
| http_proxy                | no       | not defined           | http proxy setting           | Patches registry image for http_proxy                                                      |
| https_proxy               | no       | not defined           | https proxy setting          | Patches registry image for https_proxy                                                     |
| no_proxy                  | no       | not defined           | no_proxy setting             | Patches registry image for no_proxy                                                        |
| docker_timeout            | yes      | 300                   | Integer value                | Number of seconds before docker pull timeout                                               |
| docker_retries            | yes      | 60                    | Integer value                | Number of tries for docker pull                                                            |
| docker_delay              | yes      | 10                    | Integer value                | Delay in seconds between pull retries                                                      |
| default_retries           | yes      | 60                    | Integer value                | Default number of retries                                                                  |
| default_delay             | yes      | 60                    | Integer value                | Default delay in seconds between retries                                                   |
| registry_version          | yes      | 2.7.1                 | matches tag                  | Docker registry version                                                                    |
| registry                  | no       | yes                   | yes or no                    | spin up a private regsitry                                                                 |
| registry_host             | yes      | 192.168.0.201         | Private IP address           | IP address of the registry, if deployed via Kubernetes                                     |
| registry_port             | yes      | 5000                  | Integer                      | Port for the registry                                                                      |
| passthrough_registry      | no       | yes                   | yes or no                    | Also, create a passthrough registry                                                        |
| passthrough_registry_host | no       | 192.168.0.202         | Private IP address           | Use this ip if deployed via Kubernetes otherwise use the IP of the host                    |
| passthrough_registry_port | no       | 5000 or 5001          | Integer value                | Port for passthrough registry. 5001, if via docker-compose. Otherwise 5000, if Kuberenetes |
| registry_deploy_via       | no       | kubernetes            | kubernetes or docker-compose | how to spin up                                                                             |
| images_cache_path         | no       | /vagrant/cache/images | Path                         | Path to folder used to cache saved Docker images                                           |

## Example Playbook

An example can be found used in my Hands-on DevOps course's [master-playbook.yml](https://github.com/nemonik/hands-on-DevOps/blob/master/ansible/master-playbook.yml).

```
- hosts: masters
  remote_user: vagrant
  roles:
    - docker-registry
    - k3s-server
    - metallb
```

The above Ansible playbook spins up the Docker Regsitry prior to using my [K3s-server role](https://github.com/nemonik/k3s-server-role) to install Lightweight Kubernetes (K3s) and my [metallb role](https://github.com/nemonik/metallb-role) to install MetalLB.

For more information and to see this role put into action checkout my [Hands-on DevOps class](https://github.com/nemonik/hands-on-DevOps) project.

## License

3-Clause BSD License

## Author Information

Michael Joseph Walsh <mjwalsh@nemonik.com>
