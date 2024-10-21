# Podman testing on FreeBSD

## What is this project? 
The FreeBSD OCI Runtime Extension Working Group is running a time-boxed testing program for its experimental implementation of Podman on FreeBSD. We are looking for feedback from **experienced** Podman or Docker users that will help us understand user needs as we bring container support to FreeBSD through the OCI Runtime Extension.

## What is Podman?

Podman (Pod Manager) is a fully featured Open Container Initiative (OCI) compliant container engine that is a simple daemonless tool.  Podman provides a Docker-CLI compatible command line and building tool that eases the transition from other container engines and allows the management of pods, containers and images.

If your background is with Docker, you won't find much difference with Podman from the user perspective.

More information on the official [Podman website](https://podman.io/) and [Documentation](https://podman.io/docs)

## Podman on FreeBSD

Podman has been ported to FreeBSD to provide an OCI container stack. 
The installation of Podman on FreeBSD is documented in the [official Podman Installation Instructions](https://podman.io/docs/installation#installing-on-freebsd-140), and is as easy as installing *sysutils/podman* from the Ports/Packages.

We recommend, however, that you install the *sysutils/podman-suite* from FreeBSD Ports/Packages, as it has also the [Buildah](https://buildah.io/) tool that is a tool that facilitates building OCI container images.

Below are some specifics regarding the FreeBSD ports.

### Maturity
Although considered experimental, this port is suitable for testing and evaluation.

### Runtime
Podman uses ocijail under the hood to run the container on FreeBSD (Podman uses crun under Linux), mostly so that support for Podman and Buildah can be developed without asking for more help from people all the time. When we start working on an OCI platform specification for FreeBSD, we expect both ocijail and runj runtimes to be usable.

### Storage
Compared to the Linux counterpart which defaults to overlayfs, Podman on FreeBSD uses ZFS and VFS storage drivers. ZFS is strongly recommended since its use of snapshots and clones makes it more efficient than VFS.

### Networking 
Podman supports Docker-style networking using a port of [CNI network plugins](https://github.com/containernetworking/plugins).
If you are not familiar with Docker-style networking, we recommend that you read the [Basic Networking Guide for Podman](https://github.com/containers/podman/blob/main/docs/tutorials/basic_networking.md).

Also, to avoid performance problems in a [known bug](https://bugs.freebsd.org/bugzilla/show_bug.cgi?id=273046), we recommend you to disable the LRO functionality on the interface (using the -lro option). More information in this [blog post](https://www.tara.sh/posts/2023/2023-09-07_freebsd_linux_podman_and_lro/).

### Container Images
Container images use the same formats and infrastructure as containerd and can be shared between the two implementations. Podman uses Buildah internally to create container images. Both tools share image storage (but not container storage), and hence each can use or manipulate images (but not containers) created by the other.

There are currently no official OCI images for FreeBSD, but the community has made available base FreeBSD images (see [Building your own container](#building-your-own-container) paragraph below).
The FreeBSD OCI Runtime Extension Working Group is working on more official images once Podman reaches a more mature level after this testing timebox.

## Building your own container

Unofficial, pre-made FreeBSD minimal installation base images are available on [https://quay.io/dougrabson/freebsd-minimal](https://quay.io/dougrabson/freebsd-minimal) and are generated through [https://github.com/dfr/freebsd-images](https://github.com/dfr/freebsd-images). We encourage testers to provide feedback and/or contributions to the above scripts.

There are also experimental FreeBSD images for a wider set of releases available on [https://quay.io/repository/bergblume/freebsd](https://quay.io/repository/bergblume/freebsd?tab=tags) from independent contributors. Those images have been generated with [https://gitlab.com/bergblume/freebsd-images](https://gitlab.com/bergblume/freebsd-images). These images are available only for the amd64 platform.
 
A simple *Dockerfile* or *Containerfile* example using a FreeBSD image is available [on this snippet here](https://gitlab.com/-/snippets/3738513).

Please note that you might come across in a [bug](https://github.com/containers/buildah/pull/5580) while executing ``podman build`` which raises a golang stack trace.
As a workaround, use the command ``buildah build`` instead with the same syntax e.g.:

 ``buildah build -t mytag -f Dockerfile .``

The bug has been fixed upstream and is available on the *latest* package collection. 
To switch to *latest*, create a */usr/local/etc/pkg/repos/FreeBSD.conf* file with the following content:

```
FreeBSD: {
  url: "pkg+http://pkg.FreeBSD.org/${ABI}/latest",
}
```
You can also refer to the paragraph "4.4.2. Quarterly and Latest Ports Branches" of the [FreeBSD HandBook](https://www.freebsdhandbook.com/ports).

## Automation

We encourage users to use automation in this test as well. Automation and repeatable processes are part of the core of modern computing. Feel free to use any automation tools of your choice, like Ansible, SaltStack, or Puppet.

Our contributors have used and tested Ansible, and we want to share their work as an example.

A simple Ansible example can be found on this repository:
[https://gitlab.com/bergblume/podman-caddy-on-freebsd](https://gitlab.com/bergblume/podman-caddy-on-freebsd)
The above contains a sample/easy playbook to deploy Podman on FreeBSD, an example how to deploy a pre-made web server container (Caddy) and the Dockerfile/Containerfile to generate such container.

A more complex example is available on this repository:
[https://codeberg.org/Honeyguide/micropod-sampler](https://codeberg.org/Honeyguide/micropod-sampler)
This Ansible playbook will deploy a scaled-down example of a datacenter environment with container orchestration tools using FreeBSD container images. It will configure a single server with Podman and then build containers for Consul, Nomad, traefik-consul, Minio, and Nginx. It will run them with a sample environment. Please note that this repository has roles that can be either used or taken as inspiration to write additional roles.

## Who can get involved?
We welcome anyone to try Podman on FreeBSD. You will need to be comfortable using experimental software and be able to supply your own infrastructure.

You will need: 
- One machine with FreeBSD-13.1 or later installed. We recommend however that you use the latest official stable release to be able to report bugs on the latest codebase. For more information on minimum requirements for Podman on FreeBSD, see [https://www.freshports.org/sysutils/podman/](https://www.freshports.org/sysutils/podman/).
- Storage running ZFS
- Familiarity with Docker or Podman

## When is the testing timebox open?
Our original testing timebox was Monday September 2, 2024 - Friday October 11, 2024.
However, we decided to keep this repo open for new feedback until the end of 2024. All information below should still be relevant, including how to get help.

## How do I get started?
1) Prepare your infrastructure
2) Install podman (see below)
3) Pull a base container image (See *Container Images* section above)
4) Deploy your workload or use the Hello application provided.
5) Raise issues in this repo to give feedback on any bugs, improvements or feature gaps.

## How do I give feedback?
Please log issues on this repo and use the labels to help us categorize the feedback. 

## Where can I get help?
Check out the rest of this README, and also come and find us on the OCI Slack [#wg-freebsd-runtime](https://opencontainers.slack.com/archives/C06HF6D0GBV) 

## Office Hours
Office hours are now closed, but please join us in the [OCI Jails Working Group](https://github.com/opencontainers/wg-freebsd-runtime) call biweekly. 
Archive recordings of the office hours held in the testing period are available on the [OCI YouTube channel](https://www.youtube.com/@opencontainers/videos). 

## How can I keep up to date with progress on cloud native containers on FreeBSD?
Please join the [FreeBSD Mailing list for jails](https://lists.freebsd.org/archives/freebsd-jail/) where we will share the latest progress.

## Installation
As mentioned in the previous paragraph, you can refer to the [official Podman Installation Instructions](https://podman.io/docs/installation#installing-on-freebsd-140)

See also [https://lists.freebsd.org/archives/freebsd-jail/2022-May/000129.html](https://lists.freebsd.org/archives/freebsd-jail/2022-May/000129.html) for an example of how to use buildah as well (2nd half of instructions).

## Other helpful resources
Check out these community blog posts: 
- [OCI containers on FreeBSD with Podman](https://docs.skunkwerks.at/LqHthEkTSeGDwV0PDUQSyg)
- [Just when I thought I would relax.. someone messages me that Podman runs on FreeBSD!!!](https://community.veeam.com/kubernetes-korner-90/just-when-i-thought-i-would-relax-someone-messages-me-that-podman-runs-on-freebsd-7432)
- [FreeBSD (and Linux), Podman containers and Large Receive Offload](https://www.tara.sh/posts/2023/2023-09-07_freebsd_linux_podman_and_lro/)
- [OCI Containers for FreeBSD](https://medium.com/@dfr/oci-containers-for-freebsd-512a6df2bc85)


## FAQs

**Q: Where can I learn more about containers on FreeBSD?**

A: The official FreeBSD [Wiki](https://wiki.freebsd.org/Containers) has an extensive section about containers and jails.

**Q: How does the Podman implementation fit with FreeBSD (is it agnostic of the usage of jails / bhyve or tied to one of these tools)?**

A: Right now it's fairly tightly linked to jails although, with the addition of something like 9pfs, I think a bhyve OCI runtime is doable, similar to krun or runx.

**Q: Also, how does it compare to other tooling in the ports tree (e.g. iocage, bastille)?**

A: Comparing to iocage or bastille, there is a strong focus on providing tools to separate function from state. For instance you can make a generic image for e.g. mysql and run a container based on that that keeps its state in a separate volume managed by podman. 

**Q: Are there docs for Podman usage on FreeBSD?**

A: The beauty of it is that you can take just about any docker or Podman doc and it'll work. Except that it's running natively rather than emulating something. Some people find the buildah tool to be particularly useful - it gives so much more control for building containers than a typical dockerfile.

**Q: What OCI runtime is this using? Runj?**

A: This is using its own runtime, ocijail, mostly so support for podman and buildah can be developed without hassling people all the time. When we start working on an OCI platform specification for FreeBSD, I expect both runtimes to be usable.

**Q: Is there a manpage for ocijail?**

A: Not currently, but you should not need to use it directly - its main function is as an abstraction layer, hiding most of the low-level container management from the high-level podman/buildah/cri-o engines.
It might be worth reading a few docker guides to cover the basic  ideas, e.g. [https://docker-curriculum.com](https://docker-curriculum.com)  - just substitute podman for docker and [https://quay.io/dougrabson/freebsd-minimal](https://quay.io/dougrabson/freebsd-minimal) for busybox.

**Q: Is there a source file for the construction of the container image?**

A: You may find it at [https://github.com/dfr/freebsd-images](https://github.com/dfr/freebsd-images) 

**Q: Does this support Linux images?**

A: Not natively, but you can check out this example of how to use FreeBSD’s Linux emulator (outside of the scope of this test) [https://productionwithscissors.run/2022/09/04/containerd-linux-on-freebsd/](https://productionwithscissors.run/2022/09/04/containerd-linux-on-freebsd/) 

**Q: How does it work without cgroups? And what are the differences from the Linux containers?**

A: I heard someone describe FreeBSD jails as what cgroups want to be when they grow up. I don't think that's fair, but would imagine that just about anything you could do with cgroups you can also do with jails.

**Q. What other file systems will FreeBSD OCI Runtime Extension support?**

A. We are working on making Union File system work (will be similar to Linux overlayfs).

**Q. Why is the networking slow under Podman on FreeBSD?**

A. There are performance problems as reported in a [known bug](https://bugs.freebsd.org/bugzilla/show_bug.cgi?id=273046). We recommend to disable the LRO functionality on the interface, using the *-lro* option in the ifconfig directive(s) of */etc/rc.conf*.

**Q. I can't take part in the testing timebox, or I have already tried it out - can I still give feedback?**

A. Yes, please create an issue in this repo, you can use the `experience report` label to tell us about your experiences.

**Q. I am getting a golang exception while executing "podman build"**

A. You might hit a new [bug](https://github.com/containers/buildah/pull/5580). We suggest to use the "buildah build" command with the same syntax or switch to the latest package branch. See the paragraph "Building your own container" of this file.
