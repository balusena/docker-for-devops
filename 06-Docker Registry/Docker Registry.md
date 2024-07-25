# Docker Registry
### 1.What Is a Docker Registry?
Docker registry is a server-side platform used by software developers to create, store, manage, and distribute
anywhere different reusable versions of Docker images. Docker images are standalone executables that include 
everything needed to run an application or service.

The images are then used as a template to instantiate (i.e., generate) a container (i.e., lightweight packages 
of application code and its dependencies) that will be utilized to run an application process or a service. 
Typically used to build containerization-based applications (i.e., apps comprised of independent components) 
and services, a Docker registry is a bit like a warehouse shelf filled with Docker images instead of goods.

![Docker Registry](https://github.com/balusena/docker-for-devops/blob/main/06-Docker%20Registry/docker_registry_image_container_relation.png)

Each Docker registry is divided into sections (i.e., Docker repositories), just like a warehouse shelf is 
divided into different sections containing different products. Each Docker repository includes various 
versions (i.e., tags) of a single Docker image and a description.

![Ubuntu Docker Image](https://github.com/balusena/docker-for-devops/blob/main/06-Docker%20Registry/ubuntu_docker.png)

![Ubuntu Docker Image Versions](https://github.com/balusena/docker-for-devops/blob/main/06-Docker%20Registry/official_ubuntu_docker_repository_versions.png)

### 2.Why Docker Registries Are Useful
According to Gartner, 65.9% of the money spent by organizations on traditional application software will 
be redirected to cloud-based products and services by 2025. With businesses shifting to cloud-native 
development and hybrid multi-cloud environments, Docker is becoming increasingly popular.

Anyone who has access to the Docker registry can upload an image, search for it, or download it. For example:

- Software developers access the Docker registry to select the Docker images they need to build containers.

- While application consumers use it to choose the Docker images needed to run services or web applications.

Why do you need it? Because it provides you application flexibility and portability. A Docker registry will
also extremely simplify and automate your deployment process, even for the most complex applications.

Do you remember when we said that the Docker registry is like a warehouse shelf? Good. Now, think about the
challenges a warehouse manager may face.I can tell you that the issues you get are very similar to the issues you encounter in 
application development. For example:

- How can you identify and/or prevent tampering with the products before they reach the stores?

- How do you protect your products against counterfeits?

- What process can you follow to deal with faulty products?

- How can you limit access to the products and the warehouse to a restricted number of people of your choice?

Depending on the Docker registry type you choose, it may also protect application consumers from
man-in-the-middle attacks and guarantee the integrity of your images.

Does it sound familiar? I bet it does. And guess what? The right Docker registry type will help you deal 
with these issues.

Moreover, it also:

- Offers a centralized location for your images. Teams can easily store, search, manage, and share public
  and private images (select or all versions).

- Guarantees an enhanced level of security. 55% of the respondents to the 2022 Kubernetes and cloud-native
  operations survey, confirmed that the security of container-based images is the top criteria for image selection. Docker private Registry and Docker Trusted Registry (more on these in a minute) can fulfill that requirement through access control and image signing.

- Saves time and resources. Thanks to automation, flexible version management, and integration with the 
  development workflow.

- It’s easily integrated with other tools and scalable. A Docker Registry can integrate with tools like 
  Kubernetes, GitHub, and Jenkins. The advantage? For example, images created from the codes tested in 
  GitHub can be added to Docker Registry to tackle inconsistencies and bugs.

Now that we know what benefits Docker registries offer, let’s find out how they work.

### 3.How Does a Docker Registry Work?
Before we get into the nitty-gritty of the different Docker registry types available, we have to first 
understand how a Docker registry works. Let’s say you’re a developer who wants to build a containerized 
application. To create a container, you’ll usually:

   - 1.Create a DockerFile. This is a text file with a standardized command-line interface (CLI) instructions
       to automatically build a Docker image.

   - 2.Build and save Docker images. Each image is made up of layers, and each layer represents one specific
       version of each image.

   - 3.Upload your Docker images to the Docker registry. This is where the Docker registry comes into play. 
       Once the images are created, you’ll need a place to store and manage them.

![Create Docker Image And Upload To Docker registry](https://github.com/balusena/docker-for-devops/blob/main/06-Docker%20Registry/create_docker_image_and_upload_to_docker_registry.png)

That’s it. Now, other users can access the Docker registry and search and/or download the image you’ve just 
uploaded to use for their development project or to run as a container.

![The Role of Docker Registry In Image Storage](https://github.com/balusena/docker-for-devops/blob/main/06-Docker%20Registry/the_role_of_docker_registry_in_image_storage.png)

### 4.What Types of Docker Registries Are Out There?
The term “Docker registry” can sometimes be a bit confusing as it can be used to indicate two different 
(but also similar) things with the same purpose. If you noticed, up till now we’ve written Docker registry
with a lowercase “r.” This is because we used the term to refer to any tool or service used to store and 
distribute container images, like those listed below:

#### 1.DockerHub:
This is one of the largest repositories with thousands of Docker images stored in public and private 
(more on that in a minute) Docker Registries. It integrates with the most used version control systems 
like GitHub and BitBucket.

#### 2.Amazon Elastic Container Registry (ECR):
Offering seamless integration with AWS Identity and Access Management (IAM) service for authentication, 
it supports private and public Docker registries. Among its features includes vulnerability image scanning 
and immutable image tags (i.e., it’s impossible to override an image once pushed to the registry),

#### 3.Azure Container Registry (ACR):
Supporting multi-region registries and Active Directory authentication, ACR offers automatic purging of old
images, retention policies, and content trust.

#### 4.Google Artifact Registry (GAR):
Previously called GCR, it’s hosted on the Google Cloud Platform and features automated image builds through
Google Cloud Source Repositories, GitHub, and Bitbucket integrations. It also includes a vulnerability scan
and it integrates perfectly with CI/CD pipelines to help you streamline container deployments.

#### 5.GitHub Package Registry:
Free for public images, GitHub Package Registry offers enhanced security permissions and supports public and
private image storage. Authentication is managed via personal access token or a GitHub account if you use a 
public repository.

#### 6.Red Hut Quay:
Offering a secure and scalable Docker registry platform for private cloud development projects, Red Hut 
Quay integrates with GitHub, BitBucket, GitLab, Red Hat OpenShift, and more.

And then, there is Docker Registry (with a capital R). This is Docker’s project official tool to store, 
manage and distribute images supported by DockerHub.

- There are three docker registry types and each one offers different features, just like the three 
  caravels (i.e., Spanish or Portuguese sailing ships) used by Columbus to get to America:

- 1.The Docker public Registry,
- 2.The Docker private Registry, and
- 3.The Docker Trusted Registry (DTR).

Time to explore all of them, one by one, more in depth.

### 5.Docker Registries: The Public, Private, and Trusted
Did you know that more than 18 million developers are already using Docker to build their applications? 
This includes small businesses, freelance developers, and big corporations. They all have different needs
addressed by one or another Docker Registry flavor. Which one is the right one for you? Read on to find it
out.

#### 1.The Docker Public Registry
This is the first type of Docker registry I played with during my career. It’s easy to use and it doesn’t
cost a penny. Ideal for small businesses and individuals with limited budgets, anyone can upload or 
download images for free.

The downsides? Like all free products, there are a bunch:

- Users are allowed to pull only a limited number of images per hour.
- The features offered are minimal.
- They might have bandwidth limits and throttling.
- Security and privacy can be an issue. As everyone can access it, privacy isn’t an option, making it unsuitable for storing proprietary images. Access control and patch management aren’t on the menu, either.
- When the service is down, you’ll have to wait for the provider to fix the issue.

However, if you’re a start-up or a small organization, with the right precautions, standardized and 
open-source images offered by a Docker public registry can still be a decent alternative to more expensive 
solutions. They’ll also enable you to share them with all your teams and communities without worrying too 
much about access.

Just remember to download only trusted images and scan them for vulnerabilities before using them. Don’t 
forget that DockerHub’s free Autobuild service was abused to mine cryptocurrencies. Bad guys are 
everywhere!

#### 2.The Docker Private Registry
Did you ever have to slow down or even delay an application deployment project because of security 
concerns , like 67% of enterprises interviewed by RedHat? If you’re looking for a more secure environment, 
a few extra features, and privacy without breaking the bank, then a Docker private registry may be the 
right choice for you.

Among its features, a Docker private registry includes:

- Distribution and sharing of images in an isolated network. This will prevent you from sending images over
  insecure internet connections.

- Possibility of picking your favorite hosting option. You’ll be able to choose between a remotely hosted 
  Docker private Registry (e.g., commercial services like the ones we mentioned at the beginning of this 
  article) or an on-premises option to cut deployment latency.

- Granular and personalized access control (e.g., based on roles). Grant upload and download access to 
  developers while giving download access only to testers and other team members. Define chains of approval
  and verification. Taking this approach will help you protect your organization against breaches.

- Security options. Scan your images for vulnerabilities at any stage of your development process. Install
  patches and track users’ activities in auditable logs. To protect your images from tampering, malware, 
  and man-in-the-middle attacks, you can also add a digital signature to the most critical images of your 
  collection (more on this in a moment) and use a secure socket layer/transport layer security (SSL/TLS) 
  certificate to encrypt the connection.

![How SSL Certificate Protect Your Docker Registry](https://github.com/balusena/docker-for-devops/blob/main/06-Docker%20Registry/how_ssl_certificate_protect_your_docker_registry.png)

#### 3.The Docker Trusted Registry (DTR)
As organizations are shifting their development processes to the cloud, concerns about cloud security are 
steadily growing. According to a Cybersecurity Insiders report, 95% of organizations’ security professionals
are moderately to extremely concerned about cloud security.

Who could blame them, when 84% of open-source codebases scanned in 2022 contained at least one vulnerability?
That’s why validating the integrity and authenticity of all images pulled from Docker registries has become
essential.

How can you do that? In our previous point, among the security features we listed, we mentioned the
possibility of uploading signed images to the Docker private Registry. The Docker Trusted Registry (DTR), 
Docker’s image storage solution for enterprises, takes this to another level.

In fact, it allows you to store and download digitally signed images only, guaranteeing their authenticity 
and protecting them from unauthorized modification. As a result, application consumers are, in turn, 
protected by man-in-the-middle attacks.

To go back to our warehouse example, the Docker trusted Registry is a bit like those heavily guarded 
warehouses. Nothing gets in or out without being identified (image signature) and checked for anomalies 
first.

Why is it important? When a developer pushes their image to a Docker registry, the image with all its 
version layers (sometimes controlled by other developers or teams), is made available to everyone who has 
access to the registry.

So, how can users downloading the image be sure that it has not been tampered with, and that it is really 
coming from a trusted source? How can you validate the security of the images throughout the whole software 
development life cycle? This is where the digital signature comes in.

There are several ways to sign an image. But, in general, before uploading the image to the Docker trusted Registry, the developer uses a tool of their choice (e.g., the Docker Content Trust [DCT] tool, Harbor, or JFrog Artifactory) to:

- 1.Hash the file using a strong algorithm (e.g., SHA-256),

- 2.Encrypt the outcome (i.e., digest) with his private key,

- 3.Create the digital signature block by adding his code signing certificate to the image.
  
Now the image is ready to be uploaded to the DTR.

![Image Signing Process](https://github.com/balusena/docker-for-devops/blob/main/06-Docker%20Registry/image_signing_process.png)

What happens when a user downloads the signed image? The signature is validated by decrypting the digest 
with the public key (usually fetched from a signature store or a local directory) and comparing the hashes. 
If they match, the signature is valid, and the image integrity hasn’t been compromised since it was signed.

Does all this sound familiar? Yup, essentially the same PKI-based process used by code signing and .exe file
signing.

Do your security policies require images to be signed by multiple parties or more than once during the SDLC?
Each image can have multiple signatures, from the same party or multiple parties. Bye-bye risky or suspicious
images!

Summary:

“What is a Docker Registry?” In layman’s terms, a Docker Registry is a great way to effectively create, 
manage and distribute container/Docker images. It enables developers and organizations to achieve super-fast
software delivery without compromising on automation, and, in some cases, security.

Docker registries come in different flavors. By using the Docker public Registry, smaller businesses, and
individuals can take advantage of standardized, easily accessible, and open-source images. While corporations
and big enterprises in need of a more secure environment can keep their proprietary images private by 
investing in a Docker private registry.

Docker registries facilitate automation, streamline collaboration and processes through other platforms 
integration (e.g., GitHub and BitBucket). And with containerization quickly replacing the traditional 
development environment, Docker registries have become something organizations can’t do without.







