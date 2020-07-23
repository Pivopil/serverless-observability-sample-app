# serverless-observability-sample-app

A sample application you can use to test observability for serverless apps

https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-install.html

## Install SAM 
```shell script
sh -c "$(curl -fsSL https://raw.githubusercontent.com/Linuxbrew/install/master/install.sh)"
test -d ~/.linuxbrew && eval $(~/.linuxbrew/bin/brew shellenv)
test -d /home/linuxbrew/.linuxbrew && eval $(/home/linuxbrew/.linuxbrew/bin/brew shellenv)
test -r ~/.bash_profile && echo "eval \$($(brew --prefix)/bin/brew shellenv)" >>~/.bash_profile
echo "eval \$($(brew --prefix)/bin/brew shellenv)" >>~/.profile
sudo apt install linuxbrew-wrapper
sudo apt-get install build-essential
brew --version
brew tap aws/tap
brew install aws-sam-cli
sam --version
```

## Deploy app
```shell script
git clone https://github.com/Pivopil/serverless-observability-sample-app.git
cd serverless-observability-sample-app/
sam build --profile=main-dev --region=us-east-1 
aws s3api create-bucket --bucket=aws-sam-bucket-ghjdf45 --region=us-east-1 --profile=main-dev
sam deploy \
 --stack-name=aws-training \
 --profile=main-dev \
 --region=us-east-1 \
 --s3-bucket=aws-sam-bucket-ghjdf45 \
 --capabilities=CAPABILITY_IAM
```

## Test the app
### Create order
```shell script
for i in $(seq 1 1000); do
  curl --header "Content-Type: application/json" \
  --request POST \
  --data '{"accountId":"AwsTraining","items":[{"sku":"123","price":5},{"sku":"321","price":10}]}' \
  https://XXXXXXXX.execute-api.us-east-1.amazonaws.com/Prod/order
done
```

### Check status
```shell script
watch -n1 curl --request GET \
 https://XXXXXXX.execute-api.us-east-1.amazonaws.com/Prod/order/199731be-8a6d-4566-8a70-72702f797e73
```
