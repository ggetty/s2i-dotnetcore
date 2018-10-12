.NET Core Docker Image
======================

This repository contains the source for building .NET Core apps as reproducible
Docker images using
[source-to-image](https://github.com/openshift/source-to-image).  The resulting
image can be run using [Docker](http://docker.io).

Versions
----------------

.NET Core versions currently provided are:

* 1.0 (RHEL 7, CentOS 7)
* 1.1 (RHEL 7)
* 2.0 (RHEL 7, CentOS 7)
* 2.1 (RHEL 7)

Building
----------------

```
$ git clone https://github.com/redhat-developer/s2i-dotnetcore.git
$ sudo VERSIONS=2.1 ./build.sh
```

Note: to build RHEL 7 based images, you need to run the build on a
properly subscribed RHEL machine. To build CentOS images on RHEL, set
BUILD_CENTOS=true. On non-RHEL, building CentOS images is the default.

Installing
----------------

The Red Hat documentation for .NET Core describes the steps to [install the image streams](https://access.redhat.com/documentation/en-us/net_core/2.1/html/getting_started_guide/gs_dotnet_on_openshift#install_imagestreams).

This repo contains a [bash script](https://raw.githubusercontent.com/redhat-developer/s2i-dotnetcore/master/install-imagestreams.sh) that performs these steps.

For example, to install the `rhel7` based images and add a secret for authenticating against the `registry.redhat.io` registry:

```sh
$ wget https://raw.githubusercontent.com/redhat-developer/s2i-dotnetcore/master/install-imagestreams.sh
$ chmod +x install-imagestreams.sh
$ ./install-imagestreams.sh --os rhel7 -u <subscription_username> -p <subscription_password>
```

Run `./install-imagestreams.sh --help` for more information.

Usage
---------------------------------

For information about usage of Docker image for .NET Core 2.1,
see [2.1 builder usage documentation](2.1/build/README.md) and
[2.1 runtime usage documentation](2.1/runtime/README.md).

For information about usage of Docker image for .NET Core 2.0,
see [2.0 builder usage documentation](2.0/build/README.md) and
[2.0 runtime usage documentation](2.0/runtime/README.md).

For information about usage of Docker image for .NET Core 1.1,
see [1.1 usage documentation](1.1/README.md).

For information about usage of Docker image for .NET Core 1.0,
see [1.0 usage documentation](1.0/README.md).

Image name structure
------------------------

##### Structure: dotnet/1-2-3

1. Platform name: 'dotnetcore' for 1.x, 'dotnet' for 2.0+
2. Platform version (without dots)
3. Base image: 'rhel7' or 'centos7'

Examples: `dotnet/dotnetcore-10-rhel7`, `dotnet/dotnet-20-centos7`

OpenShift Templates
-------------------

The `templates` folder contains OpenShift templates. Some of these will be shipped with OpenShift.
If a template is not on your OpenShift installation, you can import it:

```
oc create -f <template.json>
```

To instantiate a template you can use the `oc new-app` command:

```
oc new-app --template=<template>
```

The template can also be instantiated using the OpenShift web console. Login to the console and
navigate to the desired project. Click the **Add to Project** button. Search and select the desired template by it's name (e.g. dotnet-example).
Next, click **Create** to start a build and deploy the sample application. Once the build has and deployment
have completed, you can browse to the application using the url you find in project overview.

**dotnet-example**

The dotnet-example template can be used to create a new .NET Core service in OpenShift. It provides parameters for all the environment
variables of the s2i-dotnetcore builder. It also includes a liveness and a readiness probe.

**dotnet-runtime-example**

The dotnet-runtime-example template can be used to create a new .NET Core service in OpenShift. It is meant to serve as an example of
how to create a 'chained build' where one image is used to build an application but the runtime image is used to deploy the application.

For more information on build chaining in OpenShift, [please see this doc](https://docs.openshift.org/latest/dev_guide/builds/advanced_build_operations.html#dev-guide-chaining-builds).

**dotnet-pgsql-persistent**

The dotnet-pgsql-persistent creates a .NET Core service with a PostgreSQL backend. It provides parameters for all the environment
variables of the s2i-dotnetcore builder and variables to setup the database. The database connection information is passed to the
.NET application via the `ConnectionString` environment variable.
