---
layout: default
---

With the field of astronomy undergoing a revolution in data volume and automation, many observatories
around the world are beginning to update their systems to take advantage of modern web technologies.
However, producing a fully-featured and maintainable Observatory Control System is an expensive undertaking!

A major strength of open source software is not having to reinvent the wheel. 
Las Cumbres Observatory successfully operates a network of 
20+ robotic telescopes around the world, driven entirely by APIs. Parts of the software that enable this are already
open source, with the rest of it to follow suit beginning mid-2020. The goal is to increase 
the rate of adoption of APIs in astronomical observing, and to share the knowledge gained in the process of building the software so that the entire community benefits.

**So, what does an API-driven observatory really accomplish?**

Astronomers can:

* Submit requests to observe a target, track the states of those requests, and cancel
requests if their needs have changed
* Be notified once their observation is complete
* Download their data

Observatories can:

* Automatically update the observing schedule for a telescope when a new request is submitted
* Upload data
* Update the states of requests to keep users in the loop
* Manage users who are able to submit requests

Since these actions are all backed by APIs, they can be triggered programmatically and can easily be incorported into other web interfaces and software systems.

# Projects under the Observatory Control System

Conceptually, the Observatory Control System has three main categories: request
and observation management, observation scheduling, and the science archive. An observatory 
that adopts this software has the option to use all of the parts, or only a subset of them. 
Specifically, the projects that make up the software are:

### Observation Portal

This Django application is the main interface that astronomers interact with to submit observation requests
and retrieve status updates for those observation requests. It also stores the schedule that is generated 
from all observation requests. It is fully backed by APIs and includes modules for the following:

<dl>
  <dt>Proposal management</dt>
  <dd>Calls for applications, accepting applications, converting them to 
  full proposals, time allocation, and management on proposals</dd>
  <dt>Request management</dt>
  <dd>Validation, submission, cancellation, and a set of views providing 
  ancillary information on observation requests</dd>
  <dt>Observation management</dt>
  <dd>Provides the telescope schedule, update observations, update 
  observations requests on observation update</dd>
  <dt>User identity management</dt>
  <dd>Provides Oauth2 authenticated user management that can be 
  tied into other applications</dd>
</dl>

### Configuration Database

This Django application stores a hierarchical view of observatory configuration, which
is needed by the observation portal to perform automatic validation and to calculate
estimated request durations. It includes details on the configuration of sites,
enclosures, telescopes, instruments, and cameras. The camera configuration has
customizable sets of modes and optical path elements to support a wide range of
current and future instrument configuration. The configuration is retrieved via an API.

### Downtime Database

This Django application stores periods of scheduled downtime on telescopes which can be retireved 
via an API. Scheduled downtimes occur for a variety of reasons including maintenance and education 
use. Downtimes are used in the validation of requests in the observation portal and they are also 
taken into account by the scheduler to schedule around the downtimes.

### Scheduler

This Python application creates telescope observing schedules. It gets a set of observation requests
from the observation portal, computes a schedule, and then saves a set of scheduled observations
back to the observation portal. It currently uses the GUROBI solver to solve for a schedule.

### Rise-Set Library

This Python library wraps the FORTRAN library SLALib. It performs visibility calculations
for requested targets in both the observation portal and scheduler. It supports Sidereal
and Non-Sidereal target types and includes airmass, moon distance, and zenith constraints
on visibility.

### Science Archive

This Django project provides an API to save and retrieve science data. Certain metadata are 
stored in a database for easy querying and full image data are stored in AWS s3 for download.

### Ingester Library

This Python library aids in uploading data to the science archive.

# Project conventions

All projects will follow clear conventions for consistency. Automated checks will be set up
to enforce these conventions.

### Code quality, guidelines, and style

The projects are all written in Python, and should use at least version 3.6. Stylistically,
all projects should conform to the [PEP8](https://www.python.org/dev/peps/pep-0008/) standard.
New code will be required to pass a static code analysis check.

### 3rd party libraries

3rd party libraries will be updated regularly. Patches for security vulnerabilities will be
applied as soon as possible.

### Testing

The codebases of these projects all have test suites, which are vital for quickly checking that 
the code works as intended. The projects all have varying degrees of test coverage. Each new 
feature or fix should have a test confirming its functionality, and a check that test coverage 
increases over time.

Besides running test suites, it is extremely useful for development to be able to
run a project locally on a development machine. Instructions will be provided for each project on 
how to run them locally. In addition, basic helm charts and docker images will be be provided for doing so.

### Releases

New versions of the projects will be released when there is new code available. Versioning 
of projects will follow the guidelines of [semantic versioning](https://semver.org/). In addition, detailed 
release notes will be provided on features, fixes, or breaking changes are part of the new release.

### Documentation

General top-level documentation, which includes things like FAQs, descriptions for
how the different projects fit together, and usage examples will be provided. In addition, 
documentation of public interfaces, which give a more precise definition of how to use the
libraries and web APIs, will be provided. Finally, each project will have a README, which 
will describe what a project does, why it is useful, how to get started, and where to get 
more help if needed.

# Community

This is a project for the community, by the community!

### Code of Conduct

In order to foster a welcoming and inclusive open source community, please note
that the projects will be released with a
[Contributor Code of Conduct](https://www.contributor-covenant.org/version/2/0/code_of_conduct/). By
participating in the Observatory Control System community, you agree to abide by its terms.

### Support and Contributing

Please see the TOM Toolkit documentation for [Support](https://tom-toolkit.readthedocs.io/en/stable/support.html) and 
[Contributing](https://tom-toolkit.readthedocs.io/en/stable/contributing.html). The TOM Toolkit 
has been an active open source project since 2018, also developed and maintained by engineers at Las Cumbres Observatory. 
They have been refining their process so we want to put to good use what they have learned during that time.

# Questions?

_This project is funded by the Heising-Simons Foundation._