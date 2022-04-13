# simple

This is an Google Deployment Manager (DM) template that installs Neo4j Enterprise Edition.  You can run it from the  CLI.

The template provisions Instance Group Managers (IGM), pd-ssd, and a Service Account to create a Runtime Config.

## Environment Setup

You will need a GCP account.

We also need to install glcoud.  Instructions for installing the Google Cloud SDK that includes gcloud are [here](https://cloud.google.com/sdk/).

To set up your Google environment, run the command:

    gcloud init

Now, you'll need a copy of this repo.  To make a local copy, run the commands:

    git clone https://github.com/neo4j-partners/google-deployment-manager-neo4j.git
    cd google-deployment-manager-neo4j
    cd simple

## Creating a Deployment

This repo contains different parameters files.  You can deploy with any of them using [deploy.sh](deploy.sh).  For example, for a deployment called <i>simple-deployment-007</i> that uses the <i>parameters.custom.yaml</i> configuration file, run the command:

    ./deploy.sh simple-deployment-007 custom

The script then passes the cluster configuration to GCP and builds your cluster automatically.

To access the cluster, open the [Google Cloud Console](http://cloud.google.com/console), navigate to Compute Engine and pick a node.  You can access the Neo4j Browser on port 7474 of the external (public) IP of that node.

## Working with CA-signed Certificates

When working with non-public data, please carefully follow instructions in [Neo4j SSL Setup - 4.x.pdf](../../azure-resource-manager-neo4j/simple/Neo4jSSLSetup-4.x.pdf) to configure a proper CA-signed certificate.  This document links to resources and videos which further clarify configuration.</i>

## Configuration notes

<ol> 
<li>Graph Data Science (GDS) currently runs on single node instances so even if you specify a license key and GDS version, it will not install on a multi-node cluster.</li>
<li>By default, the installer will choose the versions of APOC, Bloom, and GDS which are bundled on the server in <i>/labs</i> and <i>/plugins</i> directories specifically.  You can override these configurations explicitly.  For example, v4.4.5 ships with Bloom 2.1.0, which does not open in Neo Desktop.  So you would specify Bloom Version <i>2.1.1</i> in the configuration.</li>
<li>Installation takes around 4 minutes per machine.  You can check up on progress (and detect errors) with a command like
        
    sudo tail -100 /var/log/messages

</li>
<li>In a typical install, you will accept defaults and leave versions for GDS, Bloom, and APOC -- intentionally blank. 

    graphDataScienceVersion: None
    bloomVersion: None
    apocVersion: None

</li>
</ol>

## Deleting a Deployment

To delete the <i>simple-deployment-007</i> deployment you can either run the command below or use the GUI in the [Google Cloud Console](http://cloud.google.com/console).

    gcloud deployment-manager deployments delete simple-deployment-007
