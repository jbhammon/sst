---
title: Bootstrapping
description: "Bootstrapping your AWS account and managing metadata for your SST apps."
---

<HeadlineText>

Bootstrapping is the process of creating resources in your AWS account before you can deploy SST apps into them.

</HeadlineText>

SST needs to know about the current state of your app. To achieve this, SST stores information about the app, including [app metadata](#app-metadata) and [stack metadata](#stack-metadata), during each deployment. This information is gathered by a Lambda function that listens to CloudFormation stack deploy events. After collecting the information, the Lambda function uploads and stores it in an S3 bucket.

The Lambda function and S3 bucket are defined in a CloudFormation stack named `SSTBootstrap`. It is deployed per AWS account per region. This means that deploying multiple SST apps in the same AWS account in a single region, such as the `us-east-1` region, will result in only one SSTBootstrap stack.

---

## App metadata

The app metadata store information about the mode in which the app is running, whether it is in dev mode (`sst start`) or in production mode (`sst deploy`). Apps are deployed different in the dev and productin mode, and SST uses the app metadata to warn the user if the mode is switched.

App metadata is stored in the S3 bucket at `appMetadata/app.{appName}/stage.{stageName}.json`. 

## Stack metadata

The stack metadata includes information about the constructs created in each stack. The information is used by the [SST Console](../console.md) and by [Config](../config#updating-secrets) to look up the functions that need to be restarted when updating secret values.

Stack metadata is stored in the S3 bucket at `appMetadata/app.{appName}/stage.{stageName}/stack.{stackName}.json`. 

## CDK Bootstrap

SST is built on top of the [AWS CDK](https://aws.amazon.com/cdk/), which also has its own bootstrapping process. The CDK bootstrapping process works in a similar fashion to the SST bootstrapping process. Each AWS account and region needs to be bootstrapped only once. Read more about the [CDK bootstrapping process](https://docs.aws.amazon.com/cdk/v2/guide/bootstrapping.html).