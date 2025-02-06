# Amazon Nova Multimodal understanding workshop

Amazon Nova is a new generation of state-of-the-art (SOTA) foundation models (FMs) that deliver frontier intelligence and industry leading price-performance, available exclusively on Amazon Bedrock.

Amazon Nova Micro, Amazon Nova Lite, and Amazon Nova Pro are understanding models that accept text, image, or video inputs and generate text output. They provide a broad selection of capability, accuracy, speed, and cost operation points.

Fast and cost-effective inference across intelligence classes State-of-the-art text, image, and video understanding
Fine-tuning on text, image, and video input Leading agentic and multimodal retrieval augmented generation (RAG) capabilities
Easy integration to proprietary data and applications with Amazon Bedrock
In this workshop, we will explore the Nova family of multimodal models. There are some prerequisites before we can all get started with the workshop.

## Setup

### Prerequisites

- Access to a SageMaker Studio Domain
- Familiarity with Jupyter notebooks

### Getting Started

1. Navigate to your [SageMaker Studio Domain](https://us-east-1.console.aws.amazon.com/sagemaker/home?region=us-east-1#/studio)
2. Select **run** for the JupyterLab instance
3. Wait a few seconds, then click **open** to access
4. Clone this repo in your Sagemaker studio instance
5. Open the **multimodal-understanding/workshop** to complete the workshop
6. Please add the following permissions to the SageMaker execution role

```
 SageMakerExecutionRole:
    Type: AWS::IAM::Role
    Properties:
      AssumeRolePolicyDocument: 
        Version: 2012-10-17
        Statement:
          - Effect: Allow
            Principal: 
              Service: 
                - sagemaker.amazonaws.com
            Action: 
              - sts:AssumeRole
          - Effect: Allow
            Principal: 
              Service: 
                - bedrock.amazonaws.com
            Action: 
              - sts:AssumeRole
      Path: /
      ManagedPolicyArns:
        - 'arn:aws:iam::aws:policy/AmazonSageMakerFullAccess'
        - 'arn:aws:iam::aws:policy/AmazonS3ReadOnlyAccess'
        - 'arn:aws:iam::aws:policy/AWSCloudFormationReadOnlyAccess'

      Policies:

        - PolicyName: sagemaker-access
          PolicyDocument:
            Version: 2012-10-17
            Statement:
              - Effect: Allow
                Action:
                  - sagemaker:*
                  - logs:*
                  - cloudwatch:*
                Resource: '*'
      Policies:
        - PolicyName: s3-access
          PolicyDocument:
            Version: 2012-10-17
            Statement:
              - Effect: Allow
                Action:
                  - s3:GetObject
                  - s3:PutObject
                  - s3:DeleteObject
                  - s3:ListBucket
                Resource: arn:aws:s3:::mmu-workshop-*
        - PolicyName: iam-access
          PolicyDocument:
            Version: 2012-10-17
            Statement:
              - Effect: Allow
                Action:
                  - iam:GetRole
                  - iam:GetRolePolicy
                  - iam:PutRolePolicy
                  - iam:CreateRole
                  - iam:DeleteRole
                  - iam:CreatePolicy
                  - iam:AttachRolePolicy
                  - iam:CreateServiceLinkedRole
                  - iam:DeleteRole
                  - iam:DeletePolicy
                  - iam:PassRole
                  - iam:GetPolicy
                  - iam:DetachRolePolicy
                  - iam:DeleteRolePolicy
                  - iam:ListAttachedRolePolicies
                Resource: '*'
        - PolicyName: bedrock-kb-access
          PolicyDocument:
            Version: 2012-10-17
            Statement:
              - Effect: Allow
                Action:
                  - bedrock:CreateKnowledgeBase
                  - bedrock:UpdateKnowledgeBase
                  - bedrock:DeleteKnowledgeBase
                  - bedrock:GetKnowledgeBase
                  - bedrock:ListKnowledgeBases
                  - bedrock:Retrieve
                  - bedrock:RetrieveAndGenerate
                  - bedrock:ListDataSources
                  - bedrock:ListIngestionJobs
                  - bedrock:GetDataSource
                  - bedrock:StartIngestionJob
                  - bedrock:InvokeModel
                  - bedrock:InvokeModelWithResponseStream
                  - bedrock:ListFoundationModels
                  - bedrock:CreateDataSource
                  - bedrock:GetIngestionJob
                  - bedrock:DeleteDataSource
                  - bedrock:ListMarketplaceModelEndpoints
                Resource: "*"
        - PolicyName: bedrock-agents-access
          PolicyDocument:
            Version: 2012-10-17
            Statement:
              - Effect: Allow
                Action:
                  - bedrock:CreateAgent
                  - bedrock:AssociateAgentKnowledgeBase
                  - bedrock:CreateAgentActionGroup
                  - bedrock:CreateAgentAlias
                  - bedrock:DeleteAgent
                  - bedrock:DeleteAgentMemory
                  - bedrock:DeleteAgentActionGroup
                  - bedrock:DeleteAgentVersion
                  - bedrock:DeleteAgentAlias
                  - bedrock:PrepareAgent
                  - bedrock:DisassociateAgentKnowledgeBase
                  - bedrock:UpdateAgentAlias
                  - bedrock:UpdateAgentActionGroup
                  - bedrock:UpdateAgent
                  - bedrock:UpdateAgentKnowledgeBase
                  - bedrock:TagResource
                  - bedrock:UntagResource
                  - bedrock:GetAgent
                  - bedrock:GetAgentKnowledgeBase
                  - bedrock:GetAgentAlias
                  - bedrock:GetAgentVersion
                  - bedrock:GetAgentActionGroup
                  - bedrock:GetAgentMemory
                  - bedrock:InvokeInlineAgent
                  - bedrock:InvokeAgent
                  - bedrock:ListAgentActionGroups
                  - bedrock:ListAgentAliases
                  - bedrock:ListAgentKnowledgeBases
                  - bedrock:ListAgentVersions
                  - bedrock:ListAgents
                  - bedrock:AssociateAgentCollaborator
                  - bedrock:DisassociateAgentCollaborator
                  - bedrock:UpdateAgentCollaborator
                  - bedrock:GetAgentCollaborator
                  - bedrock:ListAgentCollaborators
                Resource: "*"
        - PolicyName: bedrock-guardrail-access
          PolicyDocument:
            Version: 2012-10-17
            Statement:
              - Effect: Allow
                Action:
                  - bedrock:GetGuardrail
                  - bedrock:ApplyGuardrail
                Resource: "arn:aws:bedrock:*:*:guardrail/*"
        - PolicyName: bedrock-deny
          PolicyDocument:
            Version: 2012-10-17
            Statement:
              - Effect: Deny
                Action:
                  - bedrock:CreateModelCustomizationJob
                  - bedrock:StopModelCustomizationJob
                  - bedrock:GetModelCustomizationJob
                  - bedrock:ListModelCustomizationJobs
                Resource: '*'
        - PolicyName: aoss-access
          PolicyDocument:
            Version: 2012-10-17
            Statement:
              - Effect: Allow
                Action:
                  - aoss:BatchGetLifecyclePolicy
                  - aoss:GetAccessPolicy
                  - aoss:CreateAccessPolicy
                  - aoss:UpdateSecurityConfig
                  - aoss:UpdateLifecyclePolicy
                  - aoss:UpdateSecurityPolicy
                  - aoss:CreateLifecyclePolicy
                  - aoss:ListAccessPolicies
                  - aoss:ListSecurityPolicies
                  - aoss:UpdateAccessPolicy
                  - aoss:DeleteSecurityPolicy
                  - aoss:UntagResource
                  - aoss:GetSecurityPolicy
                  - aoss:ListTagsForResource
                  - aoss:CreateAccessPolicy
                  - aoss:BatchGetCollection
                  - aoss:ListLifecyclePolicies
                  - aoss:ListSecurityConfigs
                  - aoss:DeleteLifecyclePolicy
                  - aoss:CreateSecurityConfig
                  - aoss:CreateSecurityPolicy
                  - aoss:TagResource
                  - aoss:BatchGetVpcEndpoint
                  - aoss:GetPoliciesStats
                  - aoss:ListVpcEndpoints
                  - aoss:UpdateAccountSettings
                  - aoss:GetAccountSettings
                  - aoss:GetSecurityConfig
                  - aoss:BatchGetEffectiveLifecyclePolicy
                  - aoss:DeleteSecurityConfig
                  - aoss:ListCollections
                  - aoss:DeleteAccessPolicy
                  - aoss:CreateCollection
                  - aoss:UpdateCollection
                  - aoss:DeleteCollection
                Resource: "*"
        - PolicyName: aoss-access-api
          PolicyDocument:
            Version: 2012-10-17
            Statement:
              - Effect: Allow
                Action:
                   - aoss:APIAccessAll
                Resource: "arn:aws:aoss:*:*:collection/*"
        - PolicyName: lambda-access
          PolicyDocument:
            Version: 2012-10-17
            Statement:
              - Effect: Allow
                Action:
                  - lambda:AddPermission
                  - lambda:CreateFunction
                  - lambda:DeleteFunction
                  - lambda:GetFunction
                Resource: "*"
        - PolicyName: dynamodb-access
          PolicyDocument:
            Version: 2012-10-17
            Statement:
              - Effect: Allow
                Action:
                  - dynamodb:DescribeTable
                  - dynamodb:CreateTable
                  - dynamodb:DeleteTable
                  - dynamodb:ListTables
                  - dynamodb:GetItem
                  - dynamodb:Query
                  - dynamodb:PutItem
                  - dynamodb:DeleteItem
                  - dynamodb:UpdateItem
                  - dynamodb:Scan
                Resource: "arn:aws:dynamodb:*:*:table/*"
        - PolicyName: bedrock-cris-invoke
          PolicyDocument:
            Version: '2012-10-17'
            Statement:
              - Effect: Allow
                Action:
                  - bedrock:InvokeModel*
                  - bedrock:CreateInferenceProfile
                Resource:
                  - 'arn:aws:bedrock:*::foundation-model/anthropic.claude-3-haiku.*'
                  - 'arn:aws:bedrock:*::foundation-model/amazon.*'
                  - 'arn:aws:bedrock:*:*:inference-profile/us.amazon.*'
                  - 'arn:aws:bedrock:*:*:application-inference-profile/*'
              - Effect: Allow
                Action:
                  - bedrock:GetInferenceProfile
                  - bedrock:ListInferenceProfiles
                  - bedrock:DeleteInferenceProfile
                  - bedrock:TagResource
                  - bedrock:UntagResource
                  - bedrock:ListTagsForResource
                Resource:
                  - 'arn:aws:bedrock:*:*:inference-profile/us.amazon.*'
                  - 'arn:aws:bedrock:*:*:application-inference-profile/*'
``` 

## Workshop Structure

Specifically we will go through the following labs:

Module 1: Exploring Nova Models in Action via Bedrock Console, Invoke API, Converse API

Module 2: Prompt Engineering Best Practices with Nova Models

Module 3: Building a Multimodal RAG System

Module 4: Adding Agent Actions to Multimodal RAG System

## Additional Resources

- [Amazon Nova Foundation Models](https://aws.amazon.com/ai/generative-ai/nova/)
- [Amazon Nova User Guide](https://docs.aws.amazon.com/nova/latest/userguide/what-is-nova.html)

