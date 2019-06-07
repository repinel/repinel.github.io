---
layout:      post
title:       Including AWS Step Functions definitions in AWS SAM templates
description: |-
  AWS Step Functions definitions can become long, but you can keep them out
  of AWS SAM templates.
date:        2019-06-06 22:10
categories:  aws cloudformation
---

When defining [AWS Step Function][aws-step-functions] in
[AWS Serverless Application Model (SAM)][aws-sam], it's common to have a
template like:

```yaml
# sam.yaml
# ...

MyStepFunctionsStateMachine:
  Type: AWS::StepFunctions::StateMachine
  Properties:
    RoleArn: !GetAtt MyStepFunctionsRole.Arn
    DefinitionString: !Sub |-
      {
        "StartAt": "HelloWorld",
        "Comment": "Say hello world",
        "States": {
          "HelloWorld": {
            "Type": "Task",
            "Resource": "${HelloWorldFunction.Arn}",
            "Comment": "Run the HelloWorld Lambda function",
            "End": true
          }
        }
      }

# ...
```

The JSON provided to `DefinitionString` inline can easily get long and make it
harder to maintain the CloudFormation template. Thankfully, as described in
[this GitHub issue][sam-issue-531], it's possible to use the macro `AWS::Include`
to include an external template file as the definition of Step Functions.

```yaml
# sam.yaml
# ...

MyStepFunctionsStateMachine:
  Type: AWS::StepFunctions::StateMachine
  Properties:
    RoleArn: !GetAtt MyStepFunctionsRole.Arn
    Fn::Transform:
      Name: AWS::Include
      Parameters:
        Location:
          Fn::Sub: s3://${BucketName}/state-functions-definition.yaml

# ...
```

```yaml
# state-functions-definition.yaml

DefinitionString:
  Fn::Sub: |-
    {
      "StartAt": "HelloWorld",
      "Comment": "Say hello world",
      "States": {
        "HelloWorld": {
          "Type": "Task",
          "Resource": "${HelloWorldFunction.Arn}",
          "Comment": "Run the HelloWorld Lambda function",
          "End": true
        }
      }
    }
```

Please note that the template file `state-functions-definition.yaml` would have
to be copied to the S3 bucket refered by `BucketName` in the `sam.yaml` pior to
creating/updating the CloudFormation stack.

[aws-step-functions]: https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-stepfunctions-statemachine.html
[aws-sam]: https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-template-basics.html
[sam-issue-531]: https://github.com/awslabs/serverless-application-model/issues/531
