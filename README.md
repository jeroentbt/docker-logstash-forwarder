<div id="table-of-contents">
<h2>Table of Contents</h2>
<div id="text-table-of-contents">
<ul>
<li><a href="#orgheadline5">1. Docker logstash-forwarder base image</a>
<ul>
<li><a href="#orgheadline1">1.1. Versions</a></li>
<li><a href="#orgheadline4">1.2. Usage</a>
<ul>
<li><a href="#orgheadline2">1.2.1. Dockerfile</a></li>
<li><a href="#orgheadline3">1.2.2. docker run</a></li>
</ul>
</li>
</ul>
</li>
</ul>
</div>
</div>


# Docker logstash-forwarder base image<a id="orgheadline5"></a>

Basically debian:jessie with [logstash-forwarder](https://github.com/elastic/logstash-forwarder) (and [Go](http://golang.org) to build the latter).

## Versions<a id="orgheadline1"></a>

-   **Go:** 1.5.1 (64bit)
-   **logstash-forwarder:** 0.4.0

## Usage<a id="orgheadline4"></a>

Use this as a base image in your own `Dockerfile` or run as-is.

Remember `logstash-forwarder` [needs a certificate](https://github.com/elastic/logstash-forwarder#important-tlsssl-certificate-notes).

### Dockerfile<a id="orgheadline2"></a>

Here is an example of how I use it as a base for building a configured image:

    FROM jeroentbt/logstash-forwarder:latest
    
    MAINTAINER Jeroen Tiebout <jeroen@tinktenk.be>
    
    ADD ["certs", "/etc/certs/"]
    ADD ["config", "/etc/logstash-forwarder/"]
    
    # Set workdir to dir in persistent storage. Your .logstash-forwarder statefile
    # will be kept here
    WORKDIR /logs
    CMD [ "logstash-forwarder", "-config", "/etc/logstash-forwarder/" ]

### docker run<a id="orgheadline3"></a>

Or go manual:

    docker run -it \
      -v "{$PWD}/certs":/etc/certs \
      -v "{$PWD}/config":/etc/logstash-forwarder \
      jeroentbt/logstash-forwarder:latest \
      cd /logs && logstash-forwarder -config /etc/logstash-forwarder/
