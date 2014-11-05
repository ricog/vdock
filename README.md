# Vagrant Docker Demo

This is a simple Vagrant configuration using the Docker provider. It creates
separate Docker containers for PHP, Nginx, MySQL, and Memcached. This is a
proof-of-concept for future PHP development with Docker.

## Requirements

Vagrant >= 1.7.0.dev

## WARNING

Using Docker with Vagrant is a fairly new thing and thus does not always work
as expected. For example, the initial `up` will fail because `mysql` reports as
started before it is actually available to `web`. So you must start `mysql`
separately first. This is demonstrated in the usage example below.

## Usage

    git clone https://github.com/ricog/vdock.git
    cd vdock
    vagrant up mysql && vagrant up --no-parallel

Now browse to http://192.168.50.4:8030/ to see the demo.

## Shared Folders

Vagrant uses `rsync` to keep shared folders in sync with the `docker` provider
(at least on OSX). You can make changes and manually sync them:

    vagrant rsync php

Or you can keep a watcher running with:

    vagrant rsync-auto php

