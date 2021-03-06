# Serverless GitHub Self-Hosted Runners
## A Serverless Architecture AWS CDK application to run GitHub Actions Self-Hosted Runners in ECS Fargate

This project creates the nessasary resources in AWS to run GitHub Self-Hosted runners. More information here https://docs.github.com/en/free-pro-team@latest/actions/hosting-your-own-runners/about-self-hosted-runners

Under the hood the brilliant containerised version of actions-runner is provided by https://github.com/myoung34/docker-github-actions-runner

## Getting Started

Install the dependancies for both CDK and the Lambdas:
```
npm install
cd src && npm install
cd ..
```

edit the `cdk.json` `github_repos` parameter and add the names of the repo(s) you wish to run self-hosted runners against

Build and deploy the CDK app:
```
npm run build
cdk deploy -c vpc_name=example.vpc.dev -c github_owner=ExampleGithubOrgName
```

alternatively `vpc_name` and `github_owner` can be configured in the `cdk.json`

## Limitations
* Docker Container type actions cannot run in Fargate. Use Javascript type actions: https://docs.github.com/en/actions/creating-actions/about-actions#types-of-actions
* Docker Build, Run, etc commands cannot run in Fargate. [#1](https://github.com/androidwiltron/serverless-github-self-hosted-runners/issues/1) Builds may be done using a dedicated kaniko task
* Only Supports GitHub Organisation level runners. [#2](https://github.com/androidwiltron/serverless-github-self-hosted-runners/issues/2) Support Repository and Enterprise level runners
* Scaling up/down runners may lag by a minute as its run by a cron event. [#3](https://github.com/androidwiltron/serverless-github-self-hosted-runners/issues/3) An option to scale up via a webhook trigger
* The maximum number of `pending`/`in-progress` workflows per repository cannot exceed 100. [#4](https://github.com/androidwiltron/serverless-github-self-hosted-runners/issues/4) Implement limited pagination for octokit API call

## Alternatives

* https://github.com/philips-labs/terraform-aws-github-runner
* https://github.com/intercept6/aws-cdk-actions-self-hosted
