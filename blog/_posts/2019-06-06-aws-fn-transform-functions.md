---
layout:      post
title:       Supported functions in Fn::Transform
description: Getting to know the list of supported functions in `Fn::Transform`.
date:        2019-06-06 21:25
categories:  aws cloudformation
---

This is a note-to-self about [`Fn:Transform`](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/intrinsic-function-reference-transform.html)
in AWS CloudFormation. When combined with the macro [`AWS::Include`](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/create-reusable-transform-function-snippets-and-add-to-your-template-with-aws-include-transform.html),
as of today, the included template is limit to these functions:

* `Fn::Base64`
* `Fn::GetAtt`
* `Fn::GetAZs`
* `Fn::ImportValue`
* `Fn::Join`
* `Fn::Split`
* `Fn::FindInMap`
* `Fn::Select`
* `Ref`
* `Fn::Equals`
* `Fn::If`
* `Fn::Not`
* `Condition`
* `Fn::And`
* `Fn::Or`
* `Fn::Contains`
* `Fn::EachMemberEquals`
* `Fn::EachMemberIn`
* `Fn::ValueOf`
* `Fn::ValueOfAll`
* `Fn::RefAll`
* `Fn::Sub`
* `Fn::Cidr`

By the way, isn't it amazing that only `Ref` and `Condition` are available in short
form? Where's the consistency, AWS?
