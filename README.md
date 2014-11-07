# ElasticSearch OpsWorks

Deploy an ElasticSearch cluster to AWS OpsWorks from https://github.com/ThoughtWorksStudios/opsworks-elasticsearch-cookbook

Take a look at https://github.com/ThoughtWorksStudios/opsworks-elasticsearch-cookbook/blob/75c4941b097c4b4d044b2a6cb745f587532e4fc6/Berksfile for all cookbook versions installed.
This has not been tested with other versions. YMMV.

## Before deployment

Please setup the following dependencies in your AWS region:

* SSL certificate
* An SSH key pair, defaults to a keypair named "elasticsearch" if not specified
* A domain name for accessing the elasticsearch cluster
* A Route53 zone for the domain
* The default `aws-opsworks-service-role` and `aws-opsworks-ec2-role` need to exist before provisioning. OpsWorks should automatically create these roles when you add your first stack through the OpsWorks console. See http://docs.aws.amazon.com/opsworks/latest/userguide/gettingstarted-simple-stack.html and http://docs.aws.amazon.com/opsworks/latest/userguide/opsworks-security-appsrole.html for details.

## Setup environment

* Clone this repository
* Run `init_rbenv` to setup the rbenv environment, gems, etc. if you don't have it yet
** This is designed to be used in a clean environment, e.g. build agents
** The script "go" is a script to run rake tasks in a build
* `cp rbenv-vars.example .rbenv-vars`
* Fill out values in .rbenv-vars to suit your deployment

## Usage

Provision the environment:

    rake provision

Open https://<your search domain name>/_plugin/head

Destroy the environment:

    rake destroy


## Infrastructure details

    Route53 -- ELB -- EC2 attached with EBS

* Index will be stored on EBS
* One master node by default
* Load balanced by ELB
* HTTPS only with basic auth
* EC2 instance type defaults to c3.large
