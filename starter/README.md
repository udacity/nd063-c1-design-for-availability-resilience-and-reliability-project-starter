# Steps to deploy the environment

## Deploy the S3 bucket which will store the rest of the code.
aws cloudformation create-stack --stack-name s3-code-repo --template-body file://c1-s3code.yml

## copy the code to the new s3 bucket
aws s3 cp . s3://s3-code-repo-s3bucket-1wl5k4bxi7mml/ --recursive

## Deploy the cloudformation stack for the application environment AWS resources
aws cloudformation create-stack --stack-name applicationEnvironment --template-url https://s3.amazonaws.com/s3-code-repo-s3bucket-1wl5k4bxi7mml/c1-stack-top.yml
