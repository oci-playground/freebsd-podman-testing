# Podman testing on FreeBSD

## What is this project? 
The FreeBSD OCI Runtime Extension Working Group is running a time-boxed testing program for its experimental implementation of Podman on FreeBSD. 

## What is Podman on FreeBSD?
Simply put:
alias docker=podman.

Podman (Pod Manager) is a fully featured container engine that is a simple daemonless tool.  Podman provides a Docker-CLI comparable command line that eases the transition from other container engines and allows the management of pods, containers and images.

It’s a FreeBSD port of the https://github.com/containers stack. Install sysutils/podman-suite from FreeBSD Ports/Packages.

### Maturity
Suitable for testing and evaluation.

### CLI
Podman provides a CLI which is a drop-in replacement for docker.

### Storage
Container storage using the zfs and vfs storage drivers. ZFS is strongly preferred since its use of snapshots and clones makes it more efficient than vfs.

### Networking 
Supports docker-style networking using a port of https://github.com/containernetworking/plugins.

### Container Images
Container images use the same formats and infrastructure as containerd and can be shared between the two implementations. Podman uses Buildah internally to create container images.  Both tools share image (not container) storage, hence each can use or manipulate images (but not containers) created by the other.

## Who can get involved?
We welcome anyone to try Podman on FreeBSD. You will need to be comfortable using experimental software and be able to supply your own infrastructure.

You will need: 
- One server with FreeBSD-13.1 or later installed. (For more information on minimum requirements for podman on FreeBSD, see [https://www.freshports.org/sysutils/podman/](https://www.freshports.org/sysutils/podman/).)
- Storage running ZFS
- Familiarity with Docker or Podman

## How do I get started?
1) Prepare your infrastructure
2) Install podman (see below)
3) Pull the base container image provided
4) Deploy your workload or use the Hello application provided.
5) Raise issues in this repo to give feedback on any bugs, improvements or feature gaps.

## FAQs

Q: Where can I learn more about containers on FreeBSD? 

A: On the [Wiki](https://wiki.freebsd.org/Containers)

Q: How does the podman implementation fit with FreeBSD (is it agnostic of the usage of jails / bhyve or tied to one of these tools)?

A: Right now it's fairly tightly linked to jails although, with the addition of something like 9pfs, I think a bhyve OCI runtime is doable, similar to krun or runx.

Q: Also, how does it compare to other tooling in the ports tree (e.g. iocage, bastille)?

A: Comparing to iocage or bastille, there is a strong focus on providing tools to separate function from state. For instance you can make a generic image for e.g. mysql and run a container based on that that keeps its state in a separate volume managed by podman. 

Q: Are there docs for Podman usage on FreeBSD?

A: The beauty of it is that you can take just about any docker or Podman doc and it'll work. Except that it's running natively rather than emulating something. Some people find the buildah tool to be particularly useful - it gives so much more control for building containers than a typical dockerfile.

Q: What OCI runtime is this using? Runj?

A: This is using its own runtime, ocijail, mostly so support for podman and buildah can be developed without hassling people all the time. When we start working on an OCI platform specification for FreeBSD, I expect both runtimes to be usable.

Q: Is there a manpage for ocijail?

A: Not currently, but you should not need to use it directly - its main function is as an abstraction layer, hiding most of the low-level container management from the high-level podman/buildah/cri-o engines.
It might be worth reading a few docker guides to cover the basic  ideas, e.g. [https://docker-curriculum.com](https://docker-curriculum.com)  - just substitute podman for docker and [quay.io/dougrabson/freebsd-minimal](quay.io/dougrabson/freebsd-minimal) for busybox.

Q: Is there a source file for the construction of the container image?

A: You may find it at [github.com/dfr/freebsd-images](github.com/dfr/freebsd-images) 

Q: Does this support Linux images?

A: Not natively, but you can check out this example of how to use FreeBSD’s Linux emulator (outside of the scope of this test) [https://productionwithscissors.run/2022/09/04/containerd-linux-on-freebsd/](https://productionwithscissors.run/2022/09/04/containerd-linux-on-freebsd/) 

Q: How does it work without cgroups? And what are the differences from the Linux containers?

A: I heard someone describe FreeBSD jails as what cgroups want to be when they grow up. I don't think that's fair, but would imagine that just about anything you could do with cgroups you can also do with jails.

Q. What other file systems will FreeBSD OCI Runtime Extension support?

A. We are working on making Union File system work (will be similar to Linux overlayfs).



