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

## How can I get involved?
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


