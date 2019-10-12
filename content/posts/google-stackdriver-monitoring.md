---
title: "Google Stackdriver Monitoring"
date: 2019-10-11T12:01:37+03:00
draft: false
toc: false
images:
tags: 
  - monitoring
  - logging
  - stackdriver
---

Stackdriver Monitoring provides visibility into the _performance_, _uptime_, and _overall health_ of cloud-powered applications.

Stackdriver collects metrics, events, and metadata from:

- Google Cloud Platform
- Amazon Web Services
- Hosted uptime probes
- Application instrumentation
- Common application components including _Cassandra_, _Nginx_, _Apache Web Server_, _Elasticsearch_ e.t.c

Stackdriver ingests that data and generates insights via:

- Dashboards
- Charts
- And alerts.

Stackdriver alerting helps you collaborate by integrating with _Slack_, _PagerDuty_, _HipChat_, _Campfire_, and more.

To use Stackdriver, your project must be in a Stackdriver account.

Install Stackdriver monitoring agent and Stackdriver logging agent for your instance.

Uptime checks verify that a resource is always accessible. Uptime checks let you quickly verify the health of any web page,
instance, or group of resources. Each configured check is regularly contacted from a variety of locations around the world.
Uptime checks can be used as conditions in alerting policy definitions.

Alerting gives timely awareness to problems in your cloud applications so you can resolve the problems quickly. To create an alerting policy, you must describe the circumstances under which you want to be alerted and how you want to be notified.

### About Stackdriver Groups

Stackdriver lets you define and monitor groups of resources, such as VM instances, databases, and load balancers.
Groups can be based on names, tags, regions, applications, and other criteria. You can also create subgroups, up to
six levels deep, within groups.

### Incidents

When the alerting policy conditions are violated, an "incident' is created and displayed in the Incident section.

Responders can acknowledge receipt of the notification and can close the incident when it has been taken care of.
