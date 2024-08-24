# EC2 Instance Creation with CDK

This example will create:

- A new VPC
- Two public subnets
- A security group
- An EC2 instance in one of the subnets

The `/src/config.sh` file is used as [user-data](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/user-data.html) for the EC2 instance. Update this with any commands you'd like to be executed when the EC2 instance first boots.

[_learn more about user-data_](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/user-data.html)

## To Build and Deploy

```bash
$ dotnet build
$ cdk bootstrap
$ cdk deploy
```

## Connecting to the EC2 Instance

We need to create the key pair and save it to the `~/.ssh` directory.

```bash
aws ssm get-parameter --profile sandbox-swe-dparra-admin --name /ec2/keypair/{PrivateKeyID} --with-decryption --query Parameter.Value --output text > cdk-key.pem
```
and copy cdk-key.pem to `~/.ssh` directory

Copy these lines in `~/.ssh/config` file

```bash
# Ec2 environment for VSCode remote, in sandbox-swe-dparra ({Account Number} aws account)
Host {HostID: i-*}
    HostName {Public IP}
    IdentitiesOnly yes
    IdentityFile ~/.ssh/cdk-key_ed25519
    User ec2-user
```

## To Destroy

```bash
# Destroy all project resources.
$ cdk destroy
```