# Steps to reproduce a bug in Cloudformation-RDS interaction

There is a way to bring cloudformation into an inconsistent state, so that a resource can never be updated again.

1. Create a simple stack 
```
aws --region eu-central-1 cloudformation create-stack --stack-name=fails --template-body="$(cat fails.yaml)"
```

2. Wait for it to complete and then try to update it.

```
aws --region eu-central-1 cloudformation update-stack --stack-name=fails --template-body="$(cat fails-update.yaml)"
```

The update fails due to "DB subnet group 'MySubnetGroup' does not exist." The group that was created is called mysubnetgroup, but cloudformation fails to mirror lowercasing.

As a comparison, similar stack with all-lowercase subnet group name updates just fine:

```
aws --region eu-central-1 cloudformation create-stack --stack-name=works --template-body="$(cat works.yaml)"
aws --region eu-central-1 cloudformation wait stack-create-complete --stack-name=works
aws --region eu-central-1 cloudformation update-stack --stack-name=works --template-body="$(cat works-update.yaml)"
```
