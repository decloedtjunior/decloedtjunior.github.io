---
layout: post
sitemap: true
noindex: false
title: AWS CloudFormation notes while studing for AWS Certifications
categories: aws developer-certification cloudformation
---

Some notes about CloudFormation while studing for AWS Developer Certification...

# Table of Contents

When you use AWS CloudFormation, you work with **templates** and **stacks**. 

# Stacks

When you use AWS CloudFormation, you manage related resources as a single unit called a stack. To create resources, you create a stack by submitting the template that you created, and AWS CloudFormation provisions all those resources for you. CloudFormation can perform only actions that you have permission to do. You use AWS Identity and Access Management (IAM) to manage permissions.The calls that AWS CloudFormation makes are all declared by your template. By default, you can only launch 200 AWS CloudFormation stacks per region in your AWS account.

[Go Back to Index](#table-of-contents)


# Change Sets

If you need to make changes to the running resources in a stack, you update the stack. Change sets allow you to see how your changes might impact your running resources, especially for critical resources, before implementing them.

[Go Back to Index](#table-of-contents)


# Templates

AWS CloudFormation template (JSON, YAML or CloudFormation Designer) describes the resources you want and their settings. You can specify multiple resources in a single template and configure these resources to work together. 

## Template Anatomy

The following example shows a YAML-formatted template fragment.

```yaml
AWSTemplateFormatVersion: "version date"
Description:
  String
Metadata:
  template metadata
Parameters:
  set of parameters
Mappings:
  set of mappings
Conditions:
  set of conditions
Transform:
  set of transforms
Resources:
  set of resources
Outputs:
  set of outputs
```

Templates include several major sections. The **Resources** section is the only required section. 

 - AWSTemplateFormatVersion (optional): The AWS CloudFormation template version that the template conforms to. 
 - Description (optional): A text string that describes the template. This section must always follow the template format version section.
 - Metadata (optional): Objects that provide additional information about the template.
 - Parameters (optional): Values to pass to your template at runtime (when you create or update a stack). You can refer to parameters from the Resources and Outputs sections of the template.
 - Mappings (optional): A mapping of keys and associated values that you can use to specify conditional parameter values, similar to a lookup table (can use Fn::FindInMap intrinsic function in the Resources and Outputs sections)
 - Conditions (optional): Conditions that control whether certain resources are created or whether certain resource properties are assigned a value during stack creation or update. 
 - Transform (optional): For serverless applications, specifies the version of the AWS Serverless Application Model (AWS SAM) to use, using AWS SAM syntax to declare resources in your template. The model defines the syntax that you can use and how it is processed. You can also use AWS::Include transforms to work with template snippets that are stored separately from the main AWS CloudFormation template. You can store your snippet files in an Amazon S3 bucket and then reuse the functions across multiple templates.
 - Resources (required): Specifies the stack resources and their properties. You can refer to resources in the Resources and Outputs sections of the template.
 - Outputs (optional): Describes the values that are returned whenever you view your stack's properties. For example, you can declare an output for an S3 bucket name and then call the aws cloudformation describe-stacks AWS CLI command to view the name.

[Go Back to Index](#table-of-contents)


## Parameters

The following requirements apply when using parameters:

 - You can have a maximum of 60 parameters in an template. Use the Fn::Ref function (!Ref) to reference parameters (this function can also reference other elements within the template)
 - Each parameter must be given a logical name (also called logical ID), which must be alphanumeric and unique among all logical names within the template.
 - Each parameter must be assigned a value at runtime for AWS CloudFormation to successfully provision the stack. You can optionally specify a default value.
 - Parameters must be declared and referenced from within the same template. You can reference parameters from the Resources and Outputs sections of the template.
 - Parameters supported properties:
    - AllowedPattern
    - AllowedValues
    - ConstraintDescription
    - Default
    - Description
    - NoEcho
    - MaxLength, MinLength
    - MaxValue, MinValue
    - Type
 - Each parameter must be assigned a [parameter type that is supported by AWS CloudFormation](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/parameters-section-structure.html#parameters-section-structure-properties-type)
   - String
   - Number
   - List<Number>
   - CommaDelimitedList
   - [AWS-Specific Parameter Types](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/parameters-section-structure.html#aws-specific-parameter-types)
      - AWS::EC2::Image::Id  or  List<AWS::EC2::Image::Id>
      - AWS::EC2::Instance::Id  or  List<AWS::EC2::Instance::Id>
      - AWS::EC2::AvailabilityZone::Name  or  List<AWS::EC2::AvailabilityZone::Name>
      - AWS::EC2::SecurityGroup::Id  or  List<AWS::EC2::SecurityGroup::Id>
      - AWS::EC2::SecurityGroup::GroupName  or  List<AWS::EC2::SecurityGroup::GroupName>
      - AWS::EC2::Subnet::Id  or  List<AWS::EC2::Subnet::Id>
      - AWS::EC2::Volume::Id  or  List<AWS::EC2::Volume::Id>
      - AWS::EC2::VPC::Id  or  List<AWS::EC2::VPC::Id>
      - AWS::Route53::HostedZone::Id  or  List<AWS::Route53::HostedZone::Id>
      - AWS::EC2::KeyPair::KeyName
   - [SSM Parameter Types](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/parameters-section-structure.html#aws-ssm-parameter-types)
      - AWS::SSM::Parameter::Name
      - AWS::SSM::Parameter::Value<String>
      - AWS::SSM::Parameter::Value<List<String>> or AWS::SSM::Parameter::Value<CommaDelimitedList>
      - AWS::SSM::Parameter::Value<AWS-specific parameter type>
      - AWS::SSM::Parameter::Value<List<AWS-specific parameter type>>


Using dynamic references to specify template values
 - ssm, for plaintext values stored in AWS Systems Manager Parameter Store
 - ssm-secure, for secure strings stored in AWS Systems Manager Parameter Store
 - secretsmanager, for entire secrets or specific secret values that are stored in AWS Secrets Manager

```yaml
Parameters:
  SecurityGroupDescription:
    Description: Security Group Description (Simple parameter)
    Type: String
  SecurityGroupPort:
    Description: Simple Description of a Number Parameter, with MinValue and MaxValue
    Type: Number
    MinValue: 1150
    MaxValue: 65535
  InstanceType:
    Description: WebServer EC2 instance type (has default, AllowedValues)
    Type: String
    Default: t2.small
    AllowedValues:
      - t1.micro
      - t2.nano
      - t2.micro
      - t2.small
    ConstraintDescription: must be a valid EC2 instance type.
  DBPwd:
    NoEcho: true
    Description: The database admin account password (won't be echoed)
    Type: String
  KeyName:
    Description: Name of an existing EC2 KeyPair to enable SSH access to the instances. Linked to AWS Parameter
    Type: AWS::EC2::KeyPair::KeyName
    ConstraintDescription: must be the name of an existing EC2 KeyPair.
  SecurityGroupIngressCIDR:
    Description: The IP address range that can be used to communicate to the EC2 instances
    Type: String
    MinLength: '9'
    MaxLength: '18'
    Default: 0.0.0.0/0
    AllowedPattern: (\d{1,3})\.(\d{1,3})\.(\d{1,3})\.(\d{1,3})/(\d{1,2})
    ConstraintDescription: must be a valid IP CIDR range of the form x.x.x.x/x.
  MyVPC:
    Description: VPC to operate in
    Type: AWS::EC2::VPC::Id
  MySubnetIDs:
    Description: Subnet IDs that is a List of Subnet Id
    Type: "List<AWS::EC2::Subnet::Id>"
  DbSubnetIpBlocks:
    Description: "Comma-delimited list of three CIDR blocks"
    Type: CommaDelimitedList
    Default: "10.0.48.0/24, 10.0.112.0/24, 10.0.176.0/24"

Resources:
  MyEC2Instance:
    Type: "AWS::EC2::Instance"
    Properties:
      InstanceType: !Ref InstanceType  #we reference the InstanceType parameter
      KeyName: !Ref KeyName
      ImageId: "ami-a4c7edb2"  
      SubnetId: !Ref DbSubnet1  # here we reference an internal CloudFormation resource

  MySecurityGroup:
    Type: "AWS::EC2::SecurityGroup"
    Properties:
      GroupDescription: !Ref SecurityGroupDescription
      SecurityGroupIngress:
        - CidrIp: !Ref SecurityGroupIngressCIDR
          FromPort: !Ref SecurityGroupPort
          ToPort: !Ref SecurityGroupPort
          IpProtocol: tcp
      VpcId: !Ref MyVPC

  DbSubnet1:
    Type: AWS::EC2::Subnet
    Properties:
      VpcId: !Ref MyVPC
      CidrBlock: !Select [0, !Ref DbSubnetIpBlocks]  # the select function allows us to select across a list
  DbSubnet2:
    Type: AWS::EC2::Subnet
    Properties:
      VpcId: !Ref MyVPC
      CidrBlock: !Select [1, !Ref DbSubnetIpBlocks]  # the select function allows us to select across a list
  DbSubnet3:
    Type: AWS::EC2::Subnet
    Properties:
      VpcId: !Ref MyVPC
      CidrBlock: !Select [2, !Ref DbSubnetIpBlocks] # the select function allows us to select across a list
```

[Go Back to Index](#table-of-contents)

# Resources

Declares the AWS resources that you want to include in the stack

AWS Resource and Property Types Reference: [https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-template-resource-type-ref.html](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-template-resource-type-ref.html)


South America (Sao Paulo) Region **[Sample Template](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-sample-templates-sa-east-1.html)**:


AutoScaling available properties:

```yaml
Type: AWS::AutoScaling::AutoScalingGroup
Properties: 
  AutoScalingGroupName: String
  AvailabilityZones: 
    - String
  Cooldown: String
  DesiredCapacity: String
  HealthCheckGracePeriod: Integer
  HealthCheckType: String
  InstanceId: String
  LaunchConfigurationName: String
  LaunchTemplate: 
    LaunchTemplateSpecification
  LifecycleHookSpecificationList: 
    - LifecycleHookSpecification
  LoadBalancerNames: 
    - String
  MaxInstanceLifetime: Integer
  MaxSize: String
  MetricsCollection: 
    - MetricsCollection
  MinSize: String
  MixedInstancesPolicy: 
    MixedInstancesPolicy
  NotificationConfigurations: 
    - NotificationConfiguration
  PlacementGroup: String
  ServiceLinkedRoleARN: String
  Tags: 
    - TagProperty
  TargetGroupARNs: 
    - String
  TerminationPolicies: 
    - String
  VPCZoneIdentifier: 
    - String
```

[Go Back to Index](#table-of-contents)


## Resource Attribute Reference

https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-product-attribute-reference.html

 - **_DependsOn_**
   - With the DependsOn attribute you can specify that the creation of a specific resource follows another. 
   - You can use the DependsOn attribute with any resource.
   - DependsOn attribute can take a single string or list of strings.
 - **_DeletionPolicy_**
   - With the DeletionPolicy attribute you can preserve or (in some cases) backup a resource when its stack is deleted. Note that this capability also applies to stack update operations that lead to resources being deleted from stacks. 
   - To keep a resource when its stack is deleted, specify `Retain` for that resource.
   - For resources that support snapshots, such as AWS::EC2::Volume, specify `Snapshot`
   - `Delete` deletes the resource and all its content if applicable during stack deletion. However, be aware of the following considerations:
     - For Amazon S3 buckets, you must delete all objects in the bucket for deletion to succeed.
     - For AWS::RDS::DBCluster resources, the default policy is Snapshot.
     - For AWS::RDS::DBInstance resources that don't specify the DBClusterIdentifier property, the default policy is Snapshot.
 - **_CreationPolicy_**
 - **_Metadata_**
 - **_UpdatePolicy_**
 - **_UpdateReplacePolicy_**

[Go Back to Index](#table-of-contents)


# Mappings

The `Mappings` section consists of the key name Mappings. The keys in mappings must be literal strings. The values can be String or List types. You can use the Fn::FindInMap function to return a named value based on a specified key. 

```yaml
RegionMap: 
    us-east-1:
      HVM64: ami-0ff8a91507f77f867
      HVMG2: ami-0a584ac55a7631c0c
    ap-southeast-1:
      HVM64: ami-08569b978cc4dfa10
      HVMG2: ami-0be9df32ae9f92309
Resources: 
  myEC2Instance: 
    Type: "AWS::EC2::Instance"
    Properties: 
      ImageId: !FindInMap [RegionMap, !Ref "AWS::Region", HVM64]
      InstanceType: m1.small
```

You can use an **_input parameter_** with the `!FindInMap` function to refer to a specific value in a map.

```yaml
Parameters: 
  EnvironmentType: 
    Type: String
    AllowedValues: 
      - prod
      - test
Mappings: 
  RegionAndInstanceTypeToAMIID: 
    us-east-1: 
      test: "ami-8ff710e2"
      prod: "ami-f5f41398"     
    ...other regions and AMI IDs...
Resources:
  ...other resources...
Outputs: 
  TestOutput: 
    Value: !FindInMap [RegionAndInstanceTypeToAMIID, !Ref "AWS::Region", !Ref EnvironmentType]
```


### Pseudo Parameters Reference

Pseudo parameters are parameters that are predefined by AWS CloudFormation. You do not declare them in your template.

 - AWS::AccountId
   - Returns the AWS account ID of the account in which the stack is being created
 - AWS::NotificationARNs
   - Returns the list of notification Amazon Resource Names (ARNs) for the current stack.
 - AWS::Partition
   - Returns the partition that the resource is in. For standard AWS regions, the partition is aws.
 - AWS::Region
   -  string representing the AWS Region like us-west-2
 - AWS::StackId
   - Returns the ID of the stack as specified with the aws cloudformation create-stack command, such as `arn:aws:cloudformation:us-west-2:123456789012:stack/teststack/51af3dc0-da77-11e4-872e-1234567db123`.
 - AWS::StackName
   - Returns the name of the stack as specified with the aws cloudformation create-stack command, such as `teststack`.
 - AWS::URLSuffix
   - Returns the suffix for a domain. The suffix is typically `amazonaws.com` - Beijing region is `amazonaws.com.cn`.
 - AWS::NoValue
   - Removes the corresponding resource property when specified as a return value in the Fn::If intrinsic function. For example, you can use the AWS::NoValue parameter when you want to use a snapshot for an Amazon RDS DB instance only if a snapshot ID is provided. 

```yaml
    DBSnapshotIdentifier:
      Fn::If:
      - UseDBSnapshot
      - Ref: DBSnapshotName
      - Ref: AWS::NoValue
```

[Go Back to Index](#table-of-contents)

# Outputs

The optional `Outputs` section declares output values that you can import into other stacks (to create cross-stack references), return in response (to describe stack calls), or view on the AWS CloudFormation console. You can declare a maximum of 60 outputs in a template.

 - Logical ID
   - An identifier for the current output. The logical ID must be alphanumeric (a-z, A-Z, 0-9) and unique within the template.
 - Description (optional)
   - A String type that describes the output value, must be a literal string that is between 0 and 1024 bytes in length, cannot use a parameter or function to specify the description.
 - Value (required)
   - The value of the property returned by the aws cloudformation describe-stacks command, can include literals, parameter references, pseudo-parameters, a mapping value, or intrinsic functions.
 - Export (optional)
   - The name of the resource output to be exported for a cross-stack reference. You can use intrinsic functions to customize the Name value of an export
   - The following restrictions apply to cross-stack references:
     - For each AWS account, Export names must be unique within a region.
     - You can't create cross-stack references across regions. 
     - For outputs, the value of the Name property of an Export can't use Ref or GetAtt functions that depend on a resource.
     - Similarly, the ImportValue function can't include Ref or GetAtt functions that depend on a resource.
     - You can't delete a stack if another stack references one of its outputs.
     - You can't modify or remove an output value that is referenced by another stack.


> File: create-ssh-security-group.yaml

```yaml
Resources:
  MyCompanyWideSSHSecurityGroup:
    Type: AWS::EC2::SecurityGroup
    Properties:
      GroupDescription: Enable SSH access via port 22
      SecurityGroupIngress:
      - CidrIp: 10.0.48.0/24
        FromPort: 22
        IpProtocol: tcp
        ToPort: 22
      - CidrIp: 10.0.112.0/24
        FromPort: 22
        IpProtocol: tcp
        ToPort: 22
      - CidrIp: 10.0.176.0/24
        FromPort: 22
        IpProtocol: tcp
        ToPort: 22

Outputs:
  StackSSHSecurityGroup:
    Description: The SSH Security Group for our Company
    Value: !Ref MyCompanyWideSSHSecurityGroup
    Export:
      Name: SSHSecurityGroup
```


> File: ec2-reference-ssh-security-group.yaml cross-stack reference

```yaml
Resources:
  MySecureInstance:
    Type: AWS::EC2::Instance
    Properties:
      AvailabilityZone: us-east-1a
      ImageId: ami-a4c7edb2
      InstanceType: t2.micro
      SecurityGroups:
        # we reference the output here, using the Fn::ImportValue function
        - !ImportValue SSHSecurityGroup

```

[Go Back to Index](#table-of-contents)


# Conditions

`Conditions` section contains statements that define the circumstances under which entities are created or configured. You might use conditions when you want to reuse a template that can create resources in different contexts, such as a test environment versus a production environment. 

Depending on the entity you want to conditionally create or configure, you must include statements in the following template sections:

**_Parameters section_**
Define the inputs that you want your conditions to evaluate, based on the values of these input parameters.

**_Conditions section_**
Define conditions by using the intrinsic condition functions. 

**_Resources and Outputs sections_**
Associate conditions with the resources or outputs that you want to conditionally create. Use the Condition key and a condition's logical ID to associate it with a resource or output. 

```yaml
AWSTemplateFormatVersion: "2010-09-09"
Mappings:
  RegionMap:
    ap-southeast-1:
      AMI: "ami-77af2014"
      TestAz: "ap-southeast-1a"
    ap-southeast-2:
      AMI: "ami-10918173"
      TestAz: "ap-southeast-2a"
Parameters:
  EnvType:
    Type: String
    AllowedValues:
      - prod
      - test

Conditions:
  CreateProdResources: !Equals [ !Ref EnvType, prod ]

Resources:
  EC2Instance:
    Type: "AWS::EC2::Instance"
    Properties:
      ImageId: !FindInMap [RegionMap, !Ref "AWS::Region", AMI]
      InstanceType: t2.micro
      AvailabilityZone: !FindInMap [RegionMap, !Ref "AWS::Region", TestAz]

  NewVolume:
    Type: "AWS::EC2::Volume"
    Condition: CreateProdResources
    Properties:
      Size: 100
      AvailabilityZone:
        !GetAtt EC2Instance.AvailabilityZone

  MountPoint:
    Type: "AWS::EC2::VolumeAttachment"
    Condition: CreateProdResources
    Properties:
      InstanceId:
        !Ref EC2Instance
      VolumeId:
        !Ref NewVolume
      Device: /dev/sdh

Outputs:
  VolumeId:
    Condition: CreateProdResources
    Value:
      !Ref NewVolume
```

Condition funcions: 

 - Fn::And
 - Fn::Equals
 - Fn::If
 - Fn::Not
 - Fn::Or


[Go Back to Index](#table-of-contents)


# Metadata

You can use the optional Metadata section to include arbitrary JSON or YAML objects that provide details about the template.


# CloudFormation Helper Scripts Reference

AWS CloudFormation provides the following Python helper scripts that you can use to install software and start services on an Amazon EC2 instance that you create as part of your stack:

 - `cfn-init`: Use to retrieve and interpret resource metadata, install packages, create files, and start services.
 - `cfn-signal`: Use to signal with a CreationPolicy or WaitCondition, so you can synchronize other resources in the stack when the prerequisite resource or application is ready.
 - `cfn-get-metadata`: Use to retrieve metadata for a resource or path to a specific key.
 - `cfn-hup`: Use to check for updates to metadata and execute custom hooks when changes are detected.

You call the scripts directly from your template. The scripts work in conjunction with resource metadata that's defined in the same template. 

[Go Back to Index](#table-of-contents)


# Nested Stacks

Nested stacks are stacks created as part of other stacks. You create a nested stack within another stack by using the AWS::CloudFormation::Stack resource. Nested stacks can themselves contain other nested stacks, resulting in a hierarchy of stacks. Using nested stacks to declare common components is considered a best practice.

[Go Back to Index](#table-of-contents)


# Update Behaviors of Stack Resources

When you submit an update, AWS CloudFormation updates resources based on differences between what you submit and the stack's current template. Resources that have not changed run without disruption during the update process. For updated resources, AWS CloudFormation uses one of the following update behaviors:

Update with No Interruption
 - AWS CloudFormation updates the resource without disrupting operation of that resource and without changing the resource's physical ID. For example, if you update any property on an AWS::CloudTrail::Trail resource, AWS CloudFormation updates the trail without disruption.

Updates with Some Interruption
 - AWS CloudFormation updates the resource with some interruption and retains the physical ID. For example, if you update certain properties on an AWS::EC2::Instance resource, the instance might have some interruption while AWS CloudFormation and Amazon EC2 reconfigure the instance.

Replacement
 - AWS CloudFormation recreates the resource during an update, which also generates a new physical ID. AWS CloudFormation creates the replacement resource first, changes references from other dependent resources to point to the replacement resource, and then deletes the old resource. For example, if you update the Engine property of an AWS::RDS::DBInstance resource type, AWS CloudFormation creates a new resource and replaces the current DB instance resource with the new one.

The method AWS CloudFormation uses depends on which property you update for a given resource type. The update behavior for each property is described in the AWS Resource Types Reference.

For example, if you update the Port property of an AWS::RDS::DBInstance resource type, AWS CloudFormation replaces the DB instance by creating a new DB instance with the updated port setting and deletes the old DB instance. Before the update, you might plan to do the following to prepare for the database replacement:

 - Take a snapshot of the current databases.
 - Prepare a strategy for how applications that use that DB instance will handle an interruption while the DB instance is being replaced.
 - Ensure that the applications that use that DB instance take into account the updated port setting and any other updates you have made.
 - Use the DB snapshot to restore the databases on the new DB instance.

This example is not exhaustive; it's meant to give you an idea of the things to plan for when a resource is replaced during an update.


NICE **NESTED STACK**  EXAMPLE:

> [https://github.com/aws-samples/ecs-refarch-cloudformation](https://github.com/aws-samples/ecs-refarch-cloudformation)

```yaml
Description: >
  This template deploys a VPC, with a pair of public and private subnets spread across two Availabilty Zones. It deploys an Internet Gateway, with a default route on the public subnets. It deploys a pair of NAT Gateways (one in each AZ), and default routes for them in the private subnets. It then deploys a highly available ECS cluster using an AutoScaling Group, with ECS hosts distributed across multiple Availability Zones. Finally, it deploys a pair of example ECS services from containers published in Amazon EC2 Container Registry (Amazon ECR).
Resources:
  VPC:
    Type: AWS::CloudFormation::Stack
    Properties:
      TemplateURL: https://s3.amazonaws.com/ecs-refarch-cloudformation/infrastructure/vpc.yaml
      Parameters:
        EnvironmentName: !Ref AWS::StackName
        VpcCIDR: 10.180.0.0/16
        PublicSubnet1CIDR: 10.180.8.0/21
        PublicSubnet2CIDR: 10.180.16.0/21
        PrivateSubnet1CIDR: 10.180.24.0/21
        PrivateSubnet2CIDR: 10.180.32.0/21

  SecurityGroups:
    Type: AWS::CloudFormation::Stack
    Properties:
      TemplateURL: https://s3.amazonaws.com/ecs-refarch-cloudformation/infrastructure/security-groups.yaml
      Parameters:
        EnvironmentName: !Ref AWS::StackName
        VPC: !GetAtt VPC.Outputs.VPC

  ALB:
    Type: AWS::CloudFormation::Stack
    Properties:
      TemplateURL: https://s3.amazonaws.com/ecs-refarch-cloudformation/infrastructure/load-balancers.yaml
      Parameters:
        EnvironmentName: !Ref AWS::StackName
        VPC: !GetAtt VPC.Outputs.VPC
        Subnets: !GetAtt VPC.Outputs.PublicSubnets
        SecurityGroup: !GetAtt SecurityGroups.Outputs.LoadBalancerSecurityGroup

  ECS:
    Type: AWS::CloudFormation::Stack
    Properties:
      TemplateURL: https://s3.amazonaws.com/ecs-refarch-cloudformation/infrastructure/ecs-cluster.yaml
      Parameters:
        EnvironmentName: !Ref AWS::StackName
        InstanceType: t2.large
        ClusterSize: 4
        VPC: !GetAtt VPC.Outputs.VPC
        SecurityGroup: !GetAtt SecurityGroups.Outputs.ECSHostSecurityGroup
        Subnets: !GetAtt VPC.Outputs.PrivateSubnets

  ProductService:
    Type: AWS::CloudFormation::Stack
    Properties:
      TemplateURL: https://s3.amazonaws.com/ecs-refarch-cloudformation/services/product-service/service.yaml
      Parameters:
        VPC: !GetAtt VPC.Outputs.VPC
        Cluster: !GetAtt ECS.Outputs.Cluster
        DesiredCount: 2
        Listener: !GetAtt ALB.Outputs.Listener
        Path: /products

  WebsiteService:
    Type: AWS::CloudFormation::Stack
    Properties:
      TemplateURL: https://s3.amazonaws.com/ecs-refarch-cloudformation/services/website-service/service.yaml
      Parameters:
        VPC: !GetAtt VPC.Outputs.VPC
        Cluster: !GetAtt ECS.Outputs.Cluster
        DesiredCount: 2
        ProductServiceUrl:
          !Join ["/", [!GetAtt ALB.Outputs.LoadBalancerUrl, "products"]]
        Listener: !GetAtt ALB.Outputs.Listener
        Path: /
        ECSServiceAutoScalingRoleARN: !GetAtt ECS.Outputs.ECSServiceAutoScalingRole

  LifecycleHook:
    Type: AWS::CloudFormation::Stack
    Properties:
      TemplateURL: https://s3.amazonaws.com/ecs-refarch-cloudformation/infrastructure/lifecyclehook.yaml
      Parameters:
        Cluster: !GetAtt ECS.Outputs.Cluster
        ECSAutoScalingGroupName: !GetAtt ECS.Outputs.ECSAutoScalingGroupName

Outputs:
  ProductServiceUrl:
    Description: The URL endpoint for the product service
    Value: !Join ["/", [!GetAtt ALB.Outputs.LoadBalancerUrl, "products"]]

  WebsiteServiceUrl:
    Description: The URL endpoint for the website service
    Value: !Join ["", [!GetAtt ALB.Outputs.LoadBalancerUrl, "/"]]
```

[Go Back to Index](#table-of-contents)


# AWS CloudFormation Best Practices

[AWS CloudFormation Best Practices](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/best-practices.html)

**Planning and organizing**
 - Organize Your Stacks By Lifecycle and Ownership
 - Use Cross-Stack References to Export Shared Resources
 - Use IAM to Control Access
 - Reuse Templates to Replicate Stacks in Multiple Environments
 - Verify Quotas for All Resource Types
 - Use Nested Stacks to Reuse Common Template Patterns
 **Creating templates**
 - Do Not Embed Credentials in Your Templates
 - Use AWS-Specific Parameter Types
 - Use Parameter Constraints
 - Use AWS::CloudFormation::Init to Deploy Software Applications-on Amazon EC2 Instances
 - Use the Latest Helper Scripts
 - Validate Templates Before Using Them
 **Managing stacks**
 - Manage All Stack Resources Through AWS CloudFormation
 - Create Change Sets Before Updating Your Stacks
 - Use Stack Policies
 - Use AWS CloudTrail to Log AWS CloudFormation Calls
 - Use Code Reviews and Revision Controls to Manage Your Templates
 - Update Your Amazon EC2 Linux Instances Regularly


[Go Back to Index](#table-of-contents)
