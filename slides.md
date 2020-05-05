---
title-prefix: Six Feet Up
pagetitle: SaltStack Training
author: Calvin Hendryx-Parker, CTO, Six Feet Up
author-meta:
    - Calvin Hendryx-Parker
date: Purdue SaltStack 2020
date-meta: May 5, 2020
keywords:
    - Python
    - Orchestration
    - Configuration Management
    - SaltStack
---


# Deployment Automation and Orchestration {.semi-filtered data-background-image="images/snake.jpg"}
## with SaltStack
#### Calvin Hendryx-Parker, CTO
#### Six Feet Up


# Deployment Automation and Orchestration with SaltStack

## What is the end goal?

* Reliable
* Consistent
* Repeatable
* Scalable
* (and preferably as automated as possible)

# What sets Salt apart? {.semi-filtered data-background-image="images/bg-community-hero3x.original.png"}

* Remote Execution
* Event-Driven Orchestration
* Agent or Agent-less Operation
* Cloud Provisioning
* Speed and Scalability

::: notes

    Combines many tools such as:

    * chef/puppet

    * fabric for orchestration

    * terraform for cloud provisioning
:::

# Salt Terminology

* States
* Pillars
* Reactors
* Mines
* Events
* Beacons
* Engines

::: notes

    dive deeper into the structure of the state files

    Show the event steam in the terminal, run a state.

    Show the difference between a normal state and one that has been turned into a "Formula"

    Make sure to setup check so that certain states such as `file.touch` don't run every time, but only if a file is missing.

:::

# Salt Installation

* Bootstrap -- <https://github.com/saltstack/salt-bootstrap>
* Docker -- <https://hub.docker.com/r/saltstack/salt>
* Package Management 👈 careful old packages


# Salt Architecture
:::::::::::::: {.columns}
::: {.column width="40%"}
![](images/basic-comm.png){.plain}
:::
::: {.column width="60%"}
![](images/Salt_Architecture.width-800.png){.plain}
:::
::::::::::::::


# Follow along

<http://github.com/calvinhp/salt-orchestation-demo>

::: notes

    Show the running cluster

    * Demo remote execution

    * salt proxy haproxy.show_backends

    * salt proxy haproxy.list_servers django

    * Do it across multiple servers

    * salt '*' user.info dev1

:::

# Salt CLI and Remote Execution

Demo using the CLI

# Intro to Salt States

# Dependency Management in Salt

* Requisite System
* Great for Configuration Management

::: notes

    Show the django.sls state to show how it has dependancies

    + Use `require` to set the order of operation

    + Trickiest bit is the ordering of the operations

    * learn the Requisite system of Salt well and it will pay back in dividends

    * Use `watch` with services to restart them if the configuration file changes

    * Use `listen` vs `watch` if you might have multiple states that would want to restart the service
:::

# Jinja Templates with Salt

# Minion Specific Data with Pillar

# Intro to Orchestration and Runners

# Job Management

* External and Master Job Caches

# Active Jobs

```yaml
vagrant@salt:~$ sudo salt-run jobs.active
20200505044728157234:
    ----------
    Arguments:
    Function:
        state.highstate
    Returned:
    Running:
        |_
          ----------
          app1:
              13619
    StartTime:
        2020, May 05 04:47:28.157234
    Target:
        salt-call
    Target-type:
        list
    User:
        root
```

# Scheduled Tasks in Salt

```yaml
schedule:
  dev-db-pack:
    function: state.orchestrate
    args:
      - orch.dev-db-pack
    cron: '0 2 * * 0'
  dev-db-backup:
    function: state.orchestrate
    args:
      - orch.dev-db-backup
    cron: '0 23 * * *'
  prod-db-pack:
    function: state.orchestrate
    args:
      - orch.prod-db-pack
    cron: '0 2 * * 0'
  prod-db-backup:
    function: state.orchestrate
    args:
      - orch.prod-db-backup
    cron: '0 23 * * *'
```

# Alternate Schedule Format

```yaml
schedule:
  dev-backups:
    function: state.orchestrate
    args:
      - orch.dev-db-backup
    when:
      - Monday 4:00am
      - Tuesday 4:00am
      - Wednesday 4:00am
      - Thursday 4:00am
      - Thursday 9:00pm
      - Friday 4:00am
      - Saturday 4:00am
      - Sunday 4:00am
```

# Returners

* Elasticsearch
* RDBMS

# Using Dependencies with Orchestration

* True `master` control
* Orchestrate from above
* Cross minion dependencies

::: notes

    demo the code release orchestration state

:::

# Requisites in Orchestrations

```yaml
Environment passed in as pillar:
  test.check_pillar:
    - present:
        - envname

{% set envname = salt['pillar.get']('envname', 'MISSING ENVNAME') %}

Current {{ envname }} Supervisor Status:
  salt.function:
    - name: supervisord.status
    - tgt: {{ envname }}plone0*
    - require:
      - test: Environment passed in as pillar
```


# Use Reactors to Orchestrate

* Salt new `minions` as they are created
* Heal broken services by attempting common fixes

::: notes

    vagrant up the app3 server

    show haproxy status

    remove the key for app2

    show, and then add back

:::

# Salt Proxy Minions

![](images/proxy_minions.png){.plain height="550"}

# Salt Security

* Access Control
* Hardening Salt
* Vault for Secrets


# Adding a REST API to salt

* Multiple Authentication Back-ends
* Supports ACLs
* Full async websockets for event notifications

Demo of <https://github.com/saltstack/pepper>

::: notes

    External Auth supports many different backends

    Show master config, you can control who can do what.

    Show that this is setup via salt itself

    How do we replace fabric?

    Use salt-pepper to control the master with external auth

:::

# Salt Cloud

## Support for popular public & private clouds

* AWS Examples

# Alternate Salt Transports

* ZeroMQ (Default)
* Raw TCP Sockets
* RAET

Or Execute via SSH

::: notes
Reliable Asynchronous Event Transport
:::

# Salt Master Deployment Options

* Single Master
* Master Fail-over
* Hierarchal Syndics

# SaltStack Enterprise

![](https://marketplace.vmware.com/resources/e12db3f03b814d089c8df58367ad418a/screenshot/74beb8d5cd194791abab80ac8e9389c2/SaltStack%20Enterprise%20screenshot%201.png){.plain}

#

![](images/enterprise.png){.plain}

# Questions? {data-background-image="https://c1.staticflickr.com/1/92/239595034_d51a99ced1_o.jpg"}

## <calvin@sixfeetup.com>

[`@calvinhp`](https://twitter.com/calvinhp)

<br />

### Get this project:

<http://github.com/calvinhp/salt-orchestation-demo>
