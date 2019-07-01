# amazon-managed-blockchain-cfn-template


## Usage

```console
# Get Repository
> git clone https://github.com/kai-kou/amazon-managed-blockchain-cfn-template.git
> cd amazon-managed-blockchain-cfn-template

# Create KeyPair
> aws ec2 create-key-pair \
  --key-name amb-client-ec2-key \
  --query "KeyMaterial" \
  --output text > amb-client-ec2-key.pem

> chmod 400 amb-client-ec2-key.pem


# Publish Lambda Layer
> mkdir python
> pip install -t ./python boto3
> aws lambda publish-layer-version \
  --layer-name amb-boto3 \
  --zip-file fileb://python.zip \
  --compatible-runtimes python3.7


# Get Parameter
## VpcId
> aws ec2 describe-vpcs \
  --query "Vpcs"


## SubnetId
> aws ec2 describe-subnets \
  --query "Subnets"


## GlobalIP
> curl ifconfig.io

xxx.xxx.xxx.xxx


# Create Stack
> aws cloudformation create-stack \
  --stack-name hyperledger-fabric-network \
  --template-body file://hyperledger-fabric-network.yaml \
  --capabilities CAPABILITY_IAM \
  --region us-east-1 \
  --parameters '[
    {
      "ParameterKey": "VpcId",
      "ParameterValue": "vpc-xxxxxxxx"
    },
    {
      "ParameterKey": "SubnetId",
      "ParameterValue": "subnet-xxxxxxxx"
    },
    {
      "ParameterKey": "NetworkName",
      "ParameterValue": "FabricNetwork"
    },
    {
      "ParameterKey": "NetworkDescription",
      "ParameterValue": "Hyperledger Fabric Network"
    },
    {
      "ParameterKey": "AdminUsername",
      "ParameterValue": "AdminUser"
    },
    {
      "ParameterKey": "AdminPassword",
      "ParameterValue": "Password123"
    },
    {
      "ParameterKey": "OrgName",
      "ParameterValue": "org1"
    },
    {
      "ParameterKey": "Boto3LambdaLayerArn",
      "ParameterValue": "arn:aws:lambda:us-east-1:xxxxxxxxxxxx:layer:amb-boto3:1"
    },
    {
      "ParameterKey": "EC2KeyPairName",
      "ParameterValue": "amb-client-ec2-key"
    },
    {
      "ParameterKey": "ClientSSHCidrIp",
      "ParameterValue": "xxx.xxx.xxx.xxx/32"
    }
  ]'
```