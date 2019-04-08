all: help

help:
	echo "Help text"

bootstrap:
	aws cloudformation create-stack --stack-name serverless-pangeo-bootstrap --template-body file://./bootstrap.cf.yaml
	aws cloudformation wait stack-create-complete --stack-name serverless-pangeo-bootstrap

create: package
	aws cloudformation create-stack --stack-name serverless-pangeo-deployment --template-body file://./dist/packaged-template.cf.yaml --capabilities CAPABILITY_IAM --capabilities CAPABILITY_NAMED_IAM
	aws cloudformation wait stack-create-complete --stack-name serverless-pangeo-deployment

update: package
	aws cloudformation update-stack --stack-name serverless-pangeo-deployment --template-body file://./dist/packaged-template.cf.yaml --capabilities CAPABILITY_IAM --capabilities CAPABILITY_NAMED_IAM
	aws cloudformation wait stack-update-complete --stack-name serverless-pangeo-deployment

destroy:
	aws cloudformation delete-stack --stack-name serverless-pangeo-deployment
	aws cloudformation wait stack-delete-complete --stack-name serverless-pangeo-deployment

package:
	aws cloudformation package --template-file cloudformation.cf.yaml --s3-bucket serverless-pangeo-code --output-template-file dist/packaged-template.cf.yaml

install-lambda-deps:
	docker run --rm -v "$(CURDIR):/usr/local/src" -w /usr/local/src python:3.7 pip install --upgrade --target=serverless-hub/vendor -r serverless-hub/requirements.txt
