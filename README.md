# PHP Demo for CodeDeploy

This solution demonstrates multiple best-practices for deploying a PHP web application using AWS CodeDeploy.

---
## Deploy on AWS

This application is deployed using AWS CloudFormation.

#### CloudFormation Parameters: (required)
* GitHubRepo
* GitHubBranch
* GitHubToken (never commit this value)
* GitHubUser


#### AWSCLI Deployment scenarios:
* Local bash terminal
* <a href="https://us-west-2.console.aws.amazon.com/cloud9/home?region=us-west-2">Cloud9</a> (Oregon)
* <a href="https://us-west-2.console.aws.amazon.com/cloud9/home?region=us-west-2">Cloud9</a> (Oregon)

This application will be deployed on EC2 Autoscaling Group nodes working behind an Application Load Balancer. Multiple stacks can be used in the same account.

Create shared resources (create once):
```
aws cloudformation deploy --stack-name php-shared --template-file cloudformation_templates/shared_resources.yml --capabilities CAPABILITY_NAMED_IAM --parameter-overrides WorkshopName="php"
```

Create website resources (can create multiple stacks for a workshop):
```
aws cloudformation deploy --stack-name php-main --template-file cloudformation_templates/application.yml --parameter-overrides SharedResourceStack="php-shared"
```

Go to the CodePipeline console:
```
https://console.aws.amazon.com/codesuite/codepipeline/pipelines
```

Once the deployment completes, go to the application URL:
```
aws cloudformation describe-stacks --stack-name php-main --query 'Stacks[0].Outputs[?OutputKey==`Url`].OutputValue' --output text
```
---
#### Cleanup:
1. Delete S3 objects for CodeSuite before deleting CloudFormation stacks
1. Delete Stacks:
```
aws cloudformation delete-stack --stack-name php-shared

aws cloudformation delete-stack --stack-name php-main
```
---
## Credits
* Based on [React Trivia](https://github.com/ccoenraets/react-trivia)
* Grace Hopper clip art by [gingercoons](https://openclipart.org/detail/137533/grace-hopper)
* Based on [grace-hopper-jeopardy](https://github.com/clareliguori/grace-hopper-jeopardy)
---
## License

This library is licensed under the MIT License.
