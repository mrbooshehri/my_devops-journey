# What is Harbor

Harbor is an open-source cloud-native registry that stores, signs, and
scans container images for vulnerabilities. It provides a secure and
private environment for storing and deploying container images in a
Kubernetes cluster. Harbor offers features like user authentication,
access control, vulnerability scanning, image replication, and
vulnerability remediation.

# Harbor vs Other solutions

When comparing Harbor with other alternatives, it's important to
consider different aspects:

1. Docker Hub: Docker Hub is a public container registry provided by
	 Docker. It allows you to store and share container images, but it
	 lacks some of the advanced security features provided by Harbor, such
	 as vulnerability scanning and access control. Harbor, on the other
	 hand, is designed to provide a more secure and controlled environment
	 for container image management.

2. Google Container Registry (GCR): GCR is a private container registry
	 service provided by Google Cloud Platform. It offers similar
	 functionality to Harbor, including access control and vulnerability
	 scanning. However, Harbor is an open-source solution that can be
	 deployed on-premises or in any cloud environment, giving you more
	 flexibility and control over your container image management.

3. JFrog Artifactory: Artifactory is a universal artifact repository
	 manager that supports various package formats, including Docker
	 containers. While Artifactory offers container image management
	 capabilities, it is a more general-purpose solution compared to
	 Harbor. Harbor is specifically designed for container image
	 management and provides additional features like vulnerability
	 scanning and replication.

# Harbor Main Components

Harbor consists of several main components that work together to provide
a secure and feature-rich container registry environment. The main
components of Harbor are:

* Registry: The registry component is responsible for storing and serving
container images. It provides a secure and efficient way to push, pull,
and manage container images. Harbor uses the open-source Docker
Distribution as its underlying registry technology.

* Web UI: Harbor provides a web-based user interface that allows users to
interact with the registry. The UI provides functionalities such as
searching, browsing, and managing container images, as well as managing
users, projects, and repositories.

* Authentication and Access Control: Harbor includes an authentication and
access control component that enables user management and fine-grained
access control. It supports integration with external identity providers
like LDAP, Active Directory, and OIDC, allowing you to manage users and
roles effectively.

* Notary: Notary is a component integrated with Harbor that provides image
signing and verification capabilities. It allows you to sign container
images with digital signatures, ensuring the authenticity and integrity
of the images. Notary also supports image verification, allowing users
to verify the provenance of the images they pull.

* Clair: Clair is an open-source vulnerability scanning tool integrated
with Harbor. It scans container images for known vulnerabilities and
provides reports on any security issues found. Clair helps ensure that
your container images are free from known vulnerabilities before
deploying them.

* Replication: Harbor supports image replication, allowing you to
synchronize images between multiple Harbor instances or between Harbor
and external Docker registries. Replication helps in distributing images
across multiple locations and ensuring consistency and availability.

# Harbor WebUI Components

The web UI of Harbor provides a user-friendly interface for interacting
with the container registry and managing various aspects of the Harbor
environment. Here are the main components of the Harbor web UI:

* Dashboard: The dashboard provides an overview of the Harbor environment.
It typically includes key information such as the number of projects,
repositories, and users in the system. It may also display recent
activity logs and statistics related to image uploads, downloads, and
vulnerabilities.

* Navigation Menu: The navigation menu is usually located on the left side
of the UI and provides access to different sections and functionalities
of Harbor. It typically includes options for managing projects,
repositories, users, and system settings.

* Projects: The projects section allows you to create and manage projects
in Harbor. Projects provide a way to organize and group repositories and
users. Within a project, you can control access permissions and manage
repositories specific to that project.

* Repositories: The repositories section enables you to manage container
image repositories within a project. You can create new repositories,
browse existing ones, and perform actions such as pushing and pulling
images, deleting images, and setting access control policies for
repositories.

* Users: The users section provides functionality for managing user
accounts and access control. You can create new user accounts, assign
roles and permissions, and manage user authentication methods. This
section also allows for integration with external identity providers
like LDAP or Active Directory.

* System Settings: The system settings section allows administrators to
configure various aspects of Harbor. It includes options to manage
registry settings, configure authentication methods, set up email
notifications, enable vulnerability scanning, and manage replication
policies.

* Image Search and Browsing: The web UI typically includes search and
browsing functionalities to help users find specific container images.
You can search for images based on name, tags, or other metadata.
Browsing features allow you to navigate through repositories and view
image details.

# Harbor Registries

Harbor consists of two main types of registries: the internal registry
and external registries.

1. Internal Registry: The internal registry is the default registry
	 provided by Harbor. It is a private and secure registry that allows
	 you to store and manage container images within your Harbor instance.
	 The internal registry is based on the open-source Docker Distribution
	 project and provides functionalities such as image push, pull, and
	 management.

Key features of the internal registry include:

- Image Storage: The internal registry stores container images securely
	within the Harbor instance. It supports various image formats,
	including Docker images, and allows you to manage image versions and
	tags.

- Access Control: The internal registry in Harbor provides built-in
	access control mechanisms. You can define user roles and permissions
	to control who can push, pull, and manage images within the registry.
	This ensures that only authorized users have access to the images.

- Authentication: Users need to authenticate themselves to interact with
	the internal registry. Harbor supports various authentication methods,
	including username/password, token-based authentication, and
	integration with external identity providers like LDAP or Active
	Directory.

- Vulnerability Scanning: The internal registry in Harbor integrates
	with vulnerability scanning tools like Clair. This allows you to scan
	the container images stored in the registry for known vulnerabilities
	and receive reports on any security issues found.

2. External Registries: Harbor also supports integration with external
	 Docker registries. This feature allows you to pull images from
	 external registries and store them within your Harbor instance. By
	 integrating external registries, you can centralize your container
	 image management in Harbor while leveraging images from different
	 sources.

Key features of external registries in Harbor include:

- Image Replication: Harbor supports replication functionality, which
	enables you to synchronize images from external registries to your
	Harbor instance. This helps in consolidating images from different
	sources into a single consolidated registry.

- Access Control: When integrating external registries, Harbor provides
	access control mechanisms to manage who can access and pull images
	from these registries. You can define access policies and permissions
	to ensure secure and controlled access to the external images.

- Image Scanning: Harbor can also perform vulnerability scanning on
	images pulled from external registries. This ensures that even the
	images from external sources are scanned for vulnerabilities before
	being used in your environment.

By combining the internal registry with the capability to integrate with
external registries, Harbor offers a flexible and comprehensive solution
for container image management. It provides a secure and controlled
environment for storing, accessing, and managing container images, both
within your Harbor instance and from external sources.

# Harbor Replication

Harbor replication is a feature that allows you to synchronize container
images between multiple Harbor instances or between Harbor and external
Docker registries. Replication helps in distributing images across
different locations, ensuring availability, and providing a consistent
image management experience. Here's how Harbor replication works:

1. Replication Policy: To set up replication, you define a replication
policy in Harbor. The policy specifies the source registry (either
another Harbor instance or an external Docker registry) and the target
project in your local Harbor instance where the images will be stored.

1. Replication Trigger: You can configure the replication policy to be
triggered manually or automatically. Manual triggering requires an
administrator or user to initiate the replication process, while
automatic triggering allows for regular synchronization based on a
defined schedule or event.

1. Image Synchronization: Once the replication policy is set up and
triggered, Harbor will pull the container images from the source
registry and store them locally in the specified project. It ensures
that the images in the target project are an exact replica of the images
in the source registry.

1. Image Tagging and Metadata: During replication, Harbor maintains the
image tags and metadata. This includes information such as image names,
versions, labels, and any other metadata associated with the images. The
replication process ensures that the metadata remains consistent across
the source and target registries.

1. Authentication and Access Control: Harbor provides options for
controlling access and authentication during replication. You can
configure authentication credentials to access the source registry,
ensuring secure communication between the registries. Additionally, you
can apply access control policies to restrict who can replicate images
and control the visibility of the replicated images within your Harbor
instance.

1. Replication Status and Logs: Harbor keeps track of the replication
status and provides logs and notifications to keep you informed about
the replication process. You can monitor the replication progress, check
for errors or failures, and review the replication history for auditing
purposes.

By utilizing replication in Harbor, you can centralize your container
image management while leveraging images from multiple sources. It helps
you distribute images across different clusters, data centers, or cloud
environments, ensuring availability and consistency. Replication also
simplifies the process of sharing images across teams or organizations
and enables efficient collaboration in containerized environments.

Harbor is available in different distributions, which offer variations
in deployment options and additional features. Here are some of the
Harbor distributions:

1. Harbor Open Source: This is the open-source version of Harbor,
	 available for free. It is the core distribution maintained by the
	 Harbor community. You can download the source code, build, and deploy
	 it on-premises or in any cloud environment. The open-source version
	 provides all the fundamental features of Harbor, including the
	 internal registry, access control, authentication, vulnerability
	 scanning, and image replication.

2. VMware Harbor: VMware offers an enterprise-ready distribution of
	 Harbor as part of their cloud-native portfolio. It provides
	 additional enterprise-grade features and support options. VMware
	 Harbor includes advanced capabilities like high availability,
	 scalability, and disaster recovery. It also offers commercial support
	 and integration with other VMware products and solutions, enhancing
	 the overall container image management experience.

3. Harbor with Kubernetes: This distribution of Harbor is specifically
	 designed to be deployed and integrated with Kubernetes clusters. It
	 simplifies the process of setting up Harbor as a container registry
	 for Kubernetes environments. It includes Helm charts and Kubernetes
	 manifests, making it easier to deploy Harbor as a native component of
	 your Kubernetes cluster.

4. Cloud Provider Marketplaces: Some cloud providers, such as AWS and
	 Azure, offer Harbor as a pre-packaged solution in their marketplaces.
	 These distributions provide a simplified deployment experience,
	 allowing you to launch Harbor instances directly from the cloud
	 marketplace. They may also include additional integrations and
	 services specific to the respective cloud provider.

These different distributions of Harbor cater to various deployment
scenarios and requirements. Whether you choose the open-source version
for a self-hosted environment, the enterprise distribution for advanced
features and support, or the Kubernetes-specific distribution for
seamless integration with Kubernetes clusters, you can find a Harbor
distribution that suits your needs.

