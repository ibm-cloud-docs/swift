---

copyright:
  years: 2018, 2020
lastupdated: "2020-08-03"

keywords: develop swift, swift local, service credentials swift, developer tools swift, swift cli, ibmcloud build swift, ibmcloud swift

subcollection: swift

---

{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Developing locally
{: #develop-locally}

By developing locally, you can easily build, run, and test Swift apps. You use the {{site.data.keyword.dev_cli_long}} commands to perform these actions by using standard commands.

You can use the {{site.data.keyword.dev_cli_short}} to manage your server-side applications with more than a dozen commands. Learn more about the various commands at [{{site.data.keyword.dev_cli_notm}} `ibmcloud dev` commands](/docs/cli?topic=cli-idt-cli).

## Before you begin
{: #prereqs-local}

To develop locally, you must install the {{site.data.keyword.cloud_notm}} Command Line Interface. Use the following command to run the installation script:
```
curl -sL https://ibm.biz/idt-installer | bash
```
{: codeblock}

See [Getting started with the {{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-getting-started) to learn more about the configuration and use of the {{site.data.keyword.dev_cli_short}} commands.

## Retrieving the service credentials
{: #credentials-local}

After you clone your application from Git, you must retrieve the credentials for services that are bound to your application, as they aren't stored in the Git repo for your application. Retrieving the credentials allows the use of bound services. You can easily download the credentials by running the following command in the root of the application directory:
```
ibmcloud dev get-credentials
```
{: codeblock}

## Building, running, and deploying your application
{: #build-deploy-local}

1. **Build** - You can now build your application, which is a prerequisite to running your application.
  Use the following command in the root of the application directory to build your app:
  ```
  ibmcloud dev build
  ```
  {: codeblock}

2. **Run** - After a successful build, you can run your application in a local container with the following command:
  ```
  ibmcloud dev run
  ```
  {: codeblock}

  A local host and port to view your application's landing page is displayed if the command runs successfully.

3. **Deploy** - Deploy your application to the {{site.data.keyword.cloud_notm}} with the `deploy` command:
  ```
  ibmcloud dev deploy
  ```
  {: codeblock}
