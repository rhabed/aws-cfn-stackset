# aws-cfn-stackset
AWS CloudFormation StackSet Intro

## Permissions:
- Stack sets are created in this administrator account
- Determine how you want to structure permissions for the stack sets.

The simplest (and most permissive) permissions configuration is where you give all users and groups in the administrator account the ability to create and update all the stack sets managed through that account. If you need finer-grained control, you can set up permissions to specify:
    - Which users and groups can perform stack set operations in which target accounts.
    - Which resources users and groups can include in their stack sets.
    - Which stack set operations specific users and groups can perform.

- Create the necessary IAM service roles in your administrator and target accounts to define the permissions you want.


## Setup basic permissions

In the administration account, create IAM Role named: `AWSCloudFormationStackSetAdministrationRole`

`aws cloudformation create-stack --stack-name CfnAdminRoleStack --template-body file://$PWD/cfn/stackset-operations/stackset_admin.yml --region=ap-southeast-2 --profile my-admin --capabilities CAPABILITY_NAMED_IAM`

In each of the target account, create IAM Role named: `AWSCloudFormationStackSetExecutionRole`

`aws cloudformation create-stack --stack-name ExecnRoleStack --template-body file://$PWD/cfn/stackset-operations/stackset_exec.yml --region=ap-southeast-2 --profile my-account-1 --capabilities CAPABILITY_NAMED_IAM --parameters ParameterKey="AdministratorAccountId",ParameterValue="123456789012"`


## Create stack set 

`aws cloudformation create-stack-set --stack-set-name MyBucket --template-body file://$PWD/cfn/stackset/template.yaml --profile my-admin`

## Update stack set

`aws cloudformation update-stack-set --stack-set-name MyBucket --template-body file://$PWD/cfn/stackset/template.yaml --profile my-admin`

## Create stack Instance in destination account(s)

`aws cloudformation create-stack-instances --stack-set-name MyBucket --accounts 210987654321 209876543210--regions ap-southeast-2 --profile my-admin`