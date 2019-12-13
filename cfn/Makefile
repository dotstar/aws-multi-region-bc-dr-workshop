stackName=mm4
bucket=daveaws/cloudformation/mm
rolearn=arn:aws:iam::428505257828:role/cdd
all: core

push:
	aws s3 cp core.yml s3://${bucket}/core.yml
	aws s3 cp cwdashboard.yml s3://${bucket}/cwdashboard.yml
	aws s3 cp cicd.yml s3://${bucket}/cicd.yml
	aws s3 cp parameters_primary.json s3://${bucket}/parameters_primary.json

cloud9: push
	aws cloudformation create-stack --stack-name ${stackName}-cloud9 --template-body file://cloud9.yml --capabilities CAPABILITY_NAMED_IAM CAPABILITY_IAM
update-cloud9: push
	aws cloudformation update-stack --stack-name ${stackName}-cloud9 --template-body file://cloud9.yml --capabilities CAPABILITY_NAMED_IAM CAPABILITY_IAM

update: push
	aws cloudformation update-stack \
	  --stack-name ${stackName} \
	  --template-url https://s3.amazonaws.com/${bucket}/core.yml \
	  --capabilities CAPABILITY_NAMED_IAM CAPABILITY_IAM
	  
core: push
	aws cloudformation create-stack \
	  --stack-name ${stackName} \
	  --template-url https://s3.amazonaws.com/${bucket}/core.yml \
	  --capabilities CAPABILITY_NAMED_IAM CAPABILITY_IAM --region us-east-2
	  # --role-arn ${rolearn}

delete:
	aws cloudformation delete-stack --stack-name ${stackName}
