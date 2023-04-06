# Setup, DevOps, Integration, Deployment, and Security<a name="chap-oracle-postgresql.migration-process.deployment"></a>

Deployment to production is the culmination of the migration activity and is a high stakes effort which requires good planning and benefits from well tested automation\.

With DevOps, you can create a process that helps easily deploy and update your virtual architecture in a scalable and repeatable way\. This reduces the risk of human error\. Furthermore, DevOps allow us to deploy much faster than humans which may be a factor in large deployments\.

## Wave Planning<a name="chap-oracle-postgresql.migration-process.deployment.wave"></a>

For any application or cluster of applications there is an important question of sequencing because every cutover window, for example, a weekend can only accommodate so much work\. This means that larger portfolio may need to be migrated in multiple waves, and this makes wave planning necessary\.

Wave planning considers that some parts of the application will move while other stay behind under different network, connectivity and security conditions\. Different parts of the application may also be under different ownership, so wave planning becomes the place where all stakeholders coordinate their efforts\. Wave planning is a matter of minimizing risk during the overall migration\.

## Infrastructure Automation<a name="chap-oracle-postgresql.migration-process.deployment.infrastructure"></a>

Infrastructure automation is a code layer that wraps API calls to a cloud provider with commands to provision infrastructure\. Such code typically in YAML scripts are easily learned with any coding experience and are scalable and powerful\. This layer will allow you to spin up one or one hundred web nodes nearly simultaneously\. This layer is not designed to configure files on a server, install software on a server, or run commands on a server \- That comes in the next section\.

 [Terraform](https://www.terraform.io/) represents a cross cloud incarnation of this idea\. The downside of Terraform is that its cross\-cloud and open source nature makes it slower to adopt new features and provide detailed provisioning, often months after a new feature or configuration is released\.

 [CloudFormation](https://aws.amazon.com/cloudformation/) is a native AWS language in JSON or YAML format\. Use AWS CloudFormation to write infrastructure code more specifically to specific features because it doesn’t have to work with other clouds\.

## Configuration Management<a name="chap-oracle-postgresql.migration-process.deployment.configuration"></a>

Configuration management systems manages configuration of software and state of files on a server or group of servers\. These systems however are capable of much more than that, they also allow functionality such as installing software, running local commands, starting services and more in the same scalable way\.

 [Ansible](https://www.ansible.com/) is a lightweight and easily installed tool that is configured using YAML files\. It doesn’t require a local daemon to be installed on the instances that Ansible is managing\. The way that ansible does all this is through open source functions that essentially wrap cli commands that are run on remote hosts over Secure Shell Protocol \(SSH\)\. This allows for a plethora of functionality from database manipulation to package instillation through simple changes in pre\-written functions at the YAML level\. Beyond a large library of open source function one can easily write custom functions \(in python\) or simply use the cli function to run any cli command through ansible remotely and in a scalable fashion\. Some environments could be prevented from using Ansible due to limited or highly restricted SSH access to resources due to security protocols and standards\.

 [Puppet](https://puppet.com/) works in a primary and secondary system that communicates over https \(443\) and is configured using its own language called [puppet](https://puppet.com/docs/puppet/7/puppet_language.html)\. It’s often found in enterprise level deployments, configuration management platforms based on a locally deployed daemon called a node\. Puppet differs from other similar platforms like Chief is its methodology in regards to how a resource acquires a desired state\. Puppet takes a declarative approach, which is to say it defines the end state it requires, but makes not design on how it is achieved\. Due to its fairly high level of technical investment in regards to its programming language, puppet is generally not recommended for smaller deployments as the investment\.

 [Chef](https://www.chef.io/) has a lot in common with puppet like a similar primary and secondary model, they both communicate over https, and are configured using a programming language\. Where they differ is in terms of how they handles state\. Chef takes an imperative approach, which is to say you as the end user have nearly full control on how a resource acquires a desired state which it achieved through Ruby as its configuration system\. This type of deployment provides more flexibility as well as being easy to adopt if you are already using Ruby\.

## Code Repository<a name="chap-oracle-postgresql.migration-process.deployment.coderepository"></a>

A code repository offers safe storage of code and a change capture log which facilitates parallel development of a codebase by many developers simultaneously with the use of code branches, and integration with CI/CD pipelines\. Other files than application source code might be stored in a Git repository such as infrastructure as code \(IaC\), database reports; recorded state changes or even short logs\. Importantly you should never store credentials in the code repository\. Credentials should be handled in other ways discussed below to avoid sensitive files being deleted or scrubbed, remnants left behind of the original state\. The two most widely used code repositories are [GitHub](http://github.com/) and [GitLab](http://gitlab.com/) with similar features and functionality\.

## Secrets Management<a name="chap-oracle-postgresql.migration-process.deployment.secrets-management"></a>

A vault is for credentials\. There are several options when it comes to Vault, many of which are baked into a lot of the technologies already described\. Ansible has ansible vault, Jenkins has a functionality for storing and parametrize credentials which can be used at a smaller scale as a vault\. However most of these baked in vaults don’t generally have the effectiveness and capabilities of a dedicated vault\.

 [HashiCorp Vault](https://www.vaultproject.io/) is a dedicated vault that differs from a secrets manager that comes packaged with another product is its varied capabilities around secret management\. HashiCorp is an industry leader in this regards with capabilities such as dynamic secrets that can be generated on the fly for database or application credentials, data encryption, leasing and renewal which always credentials to be expired and rotated as well as the ability to revoke credentials remotely\. In general, if an enterprise requires a wide range of credentials stored across a range a technologies, Vault is generally a good option to start\.

## Orchestration<a name="chap-oracle-postgresql.migration-process.deployment.orchestration"></a>

Orchestration is the glue that binds DevOps together\. Many of the described technologies can be executed manually or from a scheduled script, however building on this idea of removing one of the largest points of failures such as humans from the actual deployment and management of infrastructure we can eliminate many of the pitfalls that can arise in the process of executing deployment scripts\. Orchestration allows you to create a repeatable timeline, with logic gates, to deploy your infrastructure in exactly the order with the configurations chosen\. This also allows deployments themselves to be tested before a change or deployment to production\. Each of the configuration management platforms discussed above generally have an orchestration platform, they are generally focused on scheduling jobs within their particular vertical\. For example, AWX for Ansible is mostly limited to scheduling ansible jobs\.

 [Jenkins](https://www.jenkins.io/) is managed through a GUI running on a primary node that is deployed on a resource within the company\. There is also functionality to allow secondary nodes deployed on micro services for larger scale\.

The process is arranged in a series of [Groovy](http://groovy-lang.org/semantics.html) files called Jenkinsfile that dictate what action will be taken during each step in the pipeline\. These jobs then can be scheduled jobs that periodically run and kick any job, from a ansible jobs that runs periodically to prevent configuration drift, to reporting jobs that execute a series of database calls\. Generally Jenkins can connect any modern codebase and technology together\.

An example pipeline you might run on Jenkins: use git to pull configuration scripts for the pipeline, then use a Terraform job from the cloned codebase to deploy a server, use Ansible to install and configure a database, then push a status file with information on the pipeline run back to git all while using Vault to manage the secrets for both access right for Jenkins and configuration of users on the database\.

![\[Orchestration\]](http://docs.aws.amazon.com/dms/latest/sbs/images/oracle-postgresql-orchestration.png)