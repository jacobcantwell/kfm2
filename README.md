# Kung Fu Master 2

---
![Stability: Concept](https://img.shields.io/badge/stability-concept-critical.svg?style=for-the-badge)

> Initial development is in progress, but there has not yet been a stable, usable release suitable for the public.
---

The Kung Fu Master 2 application is a fun game that demonstrates best development practices for building a Node.js web application using the AWS Serverless Application Model (AWS SAM) open-source framework.

The goal is to include examples of:

- Code repository practices (incremental feature branches, feature flags to decouple parallel code and release changes with less coordination)
- Folder structures
- Include CodeBuild, CodeDeploy, CodePipeline
- Include AWS Infrastructure deployment
- Include Application deployment
- Define best practices for small development teams starting a new workload
- Define when repositories should start splitting from a mono repo - e.g. keep in one repo unless teams are blocking each other then think about splitting out functionality

## Best Practice Assumptions

- One repository per workload
- One pipline per workload that deploys into different environments that are in different AWS accounts
- Repository and pipeline are in a shared development account
- Infrastructure as code includes the code pipeline, infrastructure, and application
- Split the infrastructure and application builds to keep rapid builds when possible
- Four environments: development, testing, staging, production

## What's this

This is code sample that uses CDK to:

- Create a Lambda function that can be invoked using API Gateway
- Create a CI using CodeSuite that deploys the Lambda+ApiGateway resources using `cdk deploy`

## How do I start using it

- Ensure you've followed the [guide to Getting Started to AWS CDK](https://docs.aws.amazon.com/cdk/latest/guide/getting_started.html), and you have CDK installed, and the AWS SDK installed and credentials configured.
- [Bootstrap your AWS environment](https://docs.aws.amazon.com/cdk/latest/guide/serverless_example.html#serverless_example_deploy_and_test)
- Create a CodeCommit repository. See [this documentation](https://docs.aws.amazon.com/codecommit/latest/userguide/how-to-create-repository.html) for help.
- Place the contents of this folder inside it
- Set the repository name in the `repositoryName` prop in `bin/ci.ts`.
- Build the stack with `npm run build`
- Deploy the CI stack with `cdk deploy`
- `Todo` summarize permissions
- If you'd like to deploy just the Lambda+ApiGateway stack, you can do so with `cdk deploy -a "npx ts-node bin/lambda.ts"`

## Build

To build this app, you need to be in this example's root folder. Then run the following:

```bash
npm install -g aws-cdk
npm install
npm run build
```

This will install the necessary CDK, then this example's dependencies, and then build your TypeScript files and your CloudFormation template.

## Deploy

Run `cdk deploy`. This will deploy / redeploy your Stack to your AWS Account.

After the deployment you will see the API's URL, which represents the url you can then use.

## The Component Structure

The whole component contains:

- An API, with CORS enabled on all HTTTP Methods. (Use with caution, for production apps you will want to enable only a certain domain origin to be able to query your API.)
- Lambda pointing to `src/create.ts`, containing code for __storing__ an item  into the DynamoDB table.
- Lambda pointing to `src/delete-one.ts`, containing code for __deleting__ an item from the DynamoDB table.
- Lambda pointing to `src/get-all.ts`, containing code for __getting all items__ from the DynamoDB table.
- Lambda pointing to `src/get-one.ts`, containing code for __getting an item__ from the DynamoDB table.
- Lambda pointing to `src/update-one.ts`, containing code for __updating an item__ in the DynamoDB table.
- A DynamoDB table `items` that stores the data.
- Five `LambdaIntegrations` that connect these Lambdas to the API.

## CDK Toolkit

The [`cdk.json`](./cdk.json) file in the root of this repository includes
instructions for the CDK toolkit on how to execute this program.

After building your TypeScript code, you will be able to run the CDK toolkits commands as usual:

    $ cdk ls
    <list all stacks in this program>

    $ cdk synth
    <generates and outputs cloudformation template>

    $ cdk deploy
    <deploys stack to your account>

    $ cdk diff
    <shows diff against deployed stack>

## Links

- [Seamless branch deploys with Kubernetes](https://m.signalvnoise.com/seamless-branch-deploys-with-kubernetes/)
- [End-to-End Multibranch Pipeline Project Creation](https://www.jenkins.io/doc/tutorials/build-a-multibranch-pipeline-project/)
