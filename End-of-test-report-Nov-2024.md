# End of Project Report: Podman Testing on FreeBSD

## Project Overview

The FreeBSD OCI Runtime Extension Working Group conducted a time-boxed testing program for its experimental implementation of Podman on FreeBSD. The project ran from September 2, 2024, to October 11, 2024, with the primary goal of gathering feedback from experienced Podman or Docker users to understand user needs and improve container support on FreeBSD through the OCI Runtime Extension.

## Project Objectives

1. Evaluate the experimental implementation of Podman on FreeBSD  
2. Gather user feedback on functionality, performance, and usability  
3. Identify bugs, improvements, and feature gaps  
4. Assess the viability of Podman as a container solution for FreeBSD

## Testing scope

1. **Podman Implementation**: A port of Podman to FreeBSD, providing an OCI container stack  
2. **Runtime**: Utilization of ocijail for running containers on FreeBSD  
3. **Storage**: Support for ZFS and VFS storage drivers, with ZFS recommended for efficiency  
4. **Networking**: Implementation of Docker-style networking using ported CNI network plugins  
5. **Container Images**: Compatibility with containerd image formats and infrastructure  
6. **Building Tools**: Integration with Buildah for creating container images

## Project Execution

### Infrastructure and Setup

Participants were required to:

* Use FreeBSD 13.1 or later (preferably the latest stable release)  
* Implement ZFS storage  
* Install Podman (sysutils/podman or sysutils/podman-suite from Ports/Packages)  
* Pull base container images from community sources

### Testing Methodology

Participants were encouraged to:

* Deploy their own workloads or use provided sample applications  
* Utilize automation tools like Ansible for deployment and testing  
* Build custom containers using the provided Dockerfile examples  
* Test various aspects of container management, including networking and storage

### Feedback Collection

* Issues were logged on the [test project's GitHub repository](https://github.com/oci-playground/freebsd-podman-testing)  
* Categorization of feedback using labels  
* Weekly office hours were held for direct interaction with the working group

## Key Findings and Observations

### Functionality and Performance

Podman itself performed well in the testing period. The main areas of feedback were mainly related to the general maturity of areas such as documentation, configuration, use of image registries, and dependencies that would allow the user to discover and use related tools.

### Usability

Some potential improvements have been suggested that align mainly with the handling of image registries and configuration management. Logged at [https://github.com/oci-playground/freebsd-podman-testing/issues](https://github.com/oci-playground/freebsd-podman-testing/issues)

* Possible improvement: align lock location with FreeBSD convention (/run/lock/podman-cni.lock \-\> /var/run/lock/podman-cni.lock) [\#20](https://github.com/oci-playground/freebsd-podman-testing/issues/20)   
* Possibly ambiguous config item in containers/container.conf [\#19](https://github.com/oci-playground/freebsd-podman-testing/issues/19)   
* docs: set default registry to containers-registries.conf or add instructions to manually update it [\#15](https://github.com/oci-playground/freebsd-podman-testing/issues/15)  
* docs: Use an open registry for images referenced in documentation [\#10](https://github.com/oci-playground/freebsd-podman-testing/issues/10)  
* feat: set default registry [\#9](https://github.com/oci-playground/freebsd-podman-testing/issues/9)    
* Add catatonit as a dependency of either podman-suite or podman [\#7](https://github.com/oci-playground/freebsd-podman-testing/issues/7)

### Bugs and Issues 

Logged at [https://github.com/oci-playground/freebsd-podman-testing/issues](https://github.com/oci-playground/freebsd-podman-testing/issues)

* After S3 suspend and resume any of Podman bridge and host-side epairs sometimes end up down. [\#21](https://github.com/oci-playground/freebsd-podman-testing/issues/21)    
* Failure: "operation not permitted" inside jail [\#18](https://github.com/oci-playground/freebsd-podman-testing/issues/18)

### Feature Gaps

* Rootless containers  
* Lack of resource limits

## Challenges and learnings

* Some of our Office Hours did not have participants, which meant that Doug and Alice had to wait online for the duration.  
* During the test period, there was a Containers on FreeBSD tutorial at EuroBSDcon, but we didn’t cross-promote the two, which was a missed opportunity.  
* Towards the end of the program, it became clear that we had no single effective way of sending push communications to our test participants. Some detective work on finding people through LinkedIn, etc., provided a route to sending messages, and we invited people to join the Jails mailing list. We should do this at the start next time.   
* Our social media campaign helped raise awareness, but I think we could have done more to promote it.   
* We got almost no engagement on Slack.

## Successes and Achievements

* It was the first time we had run such a testing project, and it went well. It gave us good feedback and helped to develop a nucleus of interested users.  
* Using a test-project-specific repo on GitHub worked well.  
* The OCI project was helpful in setting up our Office Hours and recording and distributing the recordings on YouTube. They also hosted our GH repo and the Slack channel for the overall OCI runtime extension working group.

## Community Engagement

* We had approximately 8 people meaningfully engaged in the program and a further handful who engaged with issues logged.   
* About half of these attended office hours at some point; the rest engaged by raising issues in the Podman on FreeBSD test GitHub repo.

## Future Directions

* We will continue to take feedback and provide asynchronous support for testing Podman on FreeBSD until the end of 2024\. There will not be any more office hours, but we will continue to monitor the GitHub repo for issues.   
* The next steps for developing Podman on FreeBSD development are to:  
  * Improve documentation.  
  * Improve configuration options.  
  * Improve use of image registries.  
  * Add support for rootless containers.  
  * Add resource limits.  
  * Add support for netavark (an alternative to CNI), the preferred network management tool for Podman.  
* Timeline for advancing Podman towards production readiness  
  * Experimental FreeBSD base images as part of regular FreeBSD release. Planned for 14.2 but may slip to 14.3.  
  * Add support for FreeBSD in the Podman integration test suite and enable FreeBSD testing in Podman CI.

## Conclusion

The test project was a good experience. It helped to build a group of people interested in Cloud Native Containers on FreeBSD. Their feedback has been helpful in finding areas for improvement. Running a test project has given us some experience to build on for future similar projects. The test project also helped to raise awareness about the work. 

## Acknowledgements

Thanks to everyone who invested time and effort in this critical testing exercise:

* Doug Rabson for all his hard work and dedication to the OCI work for FreeBSD.

* FreeBSD Foundation for providing program management resources for this program. 

* The Open Container Initiative for hosting the test GH repo and managing the Office Hours calls. 

* All the test participants.
