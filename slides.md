---
title: Deployment Automation and Orchestration with SaltStack
author: Calvin Hendryx-Parker, CTO, Six Feet Up
date: Purdue Training 2020
---


# Deployment Automation and Orchestration with SaltStack

## What is the end goal?

* Reliable
* Consistent
* Repeatable
* Scalable
* (and preferably as automated as possible)

# What sets Salt apart from other configuration management tools?

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

::: notes

    dive deeper into the structure of the state files

    Show the event steam in the terminal, run a state.

    Show the difference between a normal state and one that has been turned into a "Formula"

    Make sure to setup check so that certain states such as `file.touch` don't run every time, but only if a file is missing.

:::

# Salt Installation

* Bootstrap
* Docker

# Salt CLI and Remote Execution

Demo using the CLI


# Salt Architecture

* Masters
* Minions

## Follow along with this presentation:

TODO: change this up to use the Docker demo

http://github.com/calvinhp/salt-orchestation-demo
-------------------------------------------------

::: notes

    Show the running cluster

    * Demo remote execution

    * salt proxy haproxy.show_backends

    * salt proxy haproxy.list_servers django

    * Do it across multiple servers

    * salt '*' user.info dev1

:::

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

# Scheduled Tasks in Salt

# Using Dependencies with Orchestration

* True `master` control
* Orchestrate from above
* Cross minion dependencies

::: notes

    demo the code release orchestration state

:::

# Use Reactors to Orchestrate

* Salt new `minions` as they are created
* Heal broken services by attempting common fixes

::: notes

    vagrant up the app3 server

    show haproxy status

    remove the key for app2

    show, and then add back

:::


# Adding a REST API to salt

* Multiple Authentication Back-ends
* Supports ACLs
* Full async websockets for event notifications

::: notes

    External Auth supports many different backends

    Show master config, you can control who can do what.

    Show that this is setup via salt itself

    How do we replace fabric?

    Use salt-pepper to control the master with external auth

:::

# Salt Cloud

# Alternate Salt Transports

* SSH
* reat

# SaltStack Enterprise

# Questions?

## Thanks!

Get this project:

http://github.com/calvinhp/salt-orchestation-demo

`@calvinhp <http://twitter.com/calvinhp>`__
