In this post I am collecting pull requests that I author that I enjoyed
writing, or I found particularly interesting to work on.

## June 2020

### GOV.UK Notify incident

Following
[an incident on GOV.UK Notify](https://status.notifications.service.gov.uk/incidents/jbwmppqcphg0)
I raised the following PRs:

- [alphagov/paas-cf - horizontally scale scheduler VM](https://github.com/alphagov/paas-cf/pull/2358) - an explanation of the incident
- [cloudfoundry/cf-networking-release - bosh-dns-adapter/sdcclient: delay retries](https://github.com/cloudfoundry/cf-networking-release/pull/81) - the importance of retry logic

Somewhat related, I paired on
[a PR to reduce the number of metrics related DNS requests by 10-20x](https://github.com/alphagov/notifications-utils/pull/752)
via some short TTL caching.
This decreased p90 latency by >1s during peak traffic

### GitHub management

I had a lot of fun writing on
[a Concourse pipeline to help us manage ruby versions across GitHub repos](https://github.com/alphagov/paas-release-ci/pull/145)

### Concourse

Getting Concourse pipelines to manage themselves is a very useful feature of Concourse.
I raised
[a pull request to allow Concourse's `set-pipeline` step to manage pipelines in other teams](https://github.com/concourse/concourse/pull/5729)

## March / April / May 2020

### GOV.UK PaaS

One of the scariest PRs I've raised is
[a PR which changed how GOV.UK PaaS does automatic certificate rotation](https://github.com/alphagov/paas-cf/pull/2323).
This is related to
[a PR to ensure our CAs and certificates are generated correctly](https://github.com/alphagov/paas-cf/pull/2314)
which was raised due to
[GOV.UK PaaS's first P1 incident](https://status.cloud.service.gov.uk/incidents/92gmvk51zw19)
which was caused by a certificate rotation bug

GOV.UK PaaS have a service broker which provisions CDNs. I raised
[a PR which demonstrates the potential usability issues of Go's zero values](https://github.com/alphagov/paas-cdn-broker/pull/34)

A particularly proud moment was when GOV.UK PaaS ran out of IP addresses that
were available for provisioning backing services. I rectified this
[in a pull request to add more CIDR ranges](https://github.com/alphagov/paas-cf/pull/2290).

After we finished up some work relating to auditing operator actions,
I raised [a documentation PR about how GOV.UK PaaS's auditing system works](https://github.com/alphagov/paas-team-manual/pull/340)
which included some
[Graphviz](https://graphviz.org/)
graphs that were fun to create

### GOV.UK

GOV.UK (the publishing website) has
[a microservice called `router`](https://github.com/alphagov/router/pull/148)
which is written in Go, unlike the rest of GOV.UK which is Ruby.
Myself and a couple of colleagues
[instrumented `router` using Prometheus metrics](https://github.com/alphagov/router/pull/148).
Prometheus's multi-dimensional queries are very powerful and the metrics are
quite useful to measure the reliability of the different GOV.UK "microservices"

## January / February 2020

### Cloud Foundry

The CF Networking team are a joy to collaborate with, and I think this is very
well demonstrated in
[a PR I raised to extend the Silk CNI to give operators more egress control](https://github.com/cloudfoundry/silk-release/pull/23).
This PR was fun to work on technically, because container networking, and
because the project team are so friendly, encouraging, and helpful

### GOV.UK PaaS

I raised
[a very mundane PR to configure Prometheus storage retention via BOSH properties](https://github.com/cloudfoundry/silk-release/pull/23).
I like this PR because it is a nice demonstration of using RSpec to test YAML

### Concourse

In January 2020 Concourse created
[a new site to curate Concourse resource types](https://resource-types.concourse-ci.org/).
I raised
[a PR to add the Grafana annotation Concourse resource](https://github.com/concourse/resource-types/pull/20).
This resource type is useful for correlating Concourse pipeline actions to
metrics in Grafana.

## October / November/ December 2019

### GOV.UK PaaS

Over the quieter winter holiday period,
[I re-implemented a Concourse pipeline linting tool in Go](https://github.com/alphagov/paas-cf/pull/2210).
The previous implementation was a python script that no one in the team understood or looked at.
I added Rubocop support, secret redaction support, and terminal colours.
Using Go means that it can easily be installed using `go get`

### Cloud Foundry

Cloud Foundry has a service called Gorouter, which routes HTTP traffic.
[I raised a PR to extend tracing headers](https://github.com/cloudfoundry/gorouter/pull/261)
to support
[the W3C trace context standard](https://www.w3.org/TR/trace-context/)

I fixed
[an interesting Gorouter bug which exposed the IP address of the load balancer](https://github.com/cloudfoundry/gorouter/pull/257).
Prior to this PR I didn't know that the Host header was optional in HTTP/1.0

A colleague and I
[raised a PR to improve the BOSH vm-strategy documentation[(https://github.com/cloudfoundry/docs-bosh/pull/684)
that we found very confusing. I like pairing with technical writers because
they always ask you to explain things properly, and then you get better at
explaining

### GOV.UK


GOV.UK (the publishing website) has
[a microservice called `router`](https://github.com/alphagov/router/pull/148)
which is written in Go, unlike the rest of GOV.UK which is Ruby.
[Myself and a colleague fixed a dormant
bug](https://github.com/alphagov/router/pull/145) which was awakened by
switching GOV.UK PaaS's load balancer from an AWS ELB to an ALB. We started
using an ALB in HTTPS mode, which allowed us to support HTTP keep-alives. The
router app had a zero value for `MaxIdleConns` and so never cleaned up idle
connections, eventually hitting the open file limit.

## July/ August / September 2019

### Cloud Foundry

I discovered and fixed
[a strange bug in the BOSH Director API when a VM is in both a VIP and a dynamic network](https://github.com/cloudfoundry/bosh/pull/2206).

A strange side effect of changing BOSH's config server, is that the SSH
fingerprint format can change.
[This broke the CF conduit plugin, which had to be changed to support both fingerprint formats](https://github.com/alphagov/paas-cf-conduit/pull/42)

### GOV.UK PaaS

GOV.UK PaaS brokers relationships between government services and
infrastructure providers, part of this process involves currency conversion.
I added
[a metric to our metric microservice which tracks the European Central Bank's USD to GBP exchange rate](https://github.com/alphagov/paas-cf/pull/2016)

I also added metrics to monitor aggregate user activity:

- [how many users have logged in in the last 30 days](https://github.com/alphagov/paas-cf/pull/2005)
- [how many users do we have for each identity provider](https://github.com/alphagov/paas-cf/pull/2002)

We upgraded Cloud Foundry via
[cf-deployment](https://github.com/cloudfoundry/cf-deployment)
to version 10. We did this via two pull requests and a maintenance window:

- [CF Deployment version ten part one](https://github.com/alphagov/paas-cf/pull/1995)
- [CF Deployment version ten part two](https://github.com/alphagov/paas-cf/pull/1996)