.PHONY: apply test clean

TF_SRCS=$(wildcard *.tf)

apply: .applied

.applied: .terraform example.zip $(TF_SRCS)
	terraform apply -auto-approve
	touch .applied

destroy:
	-terraform destroy -auto-approve && rm -f .applied

test: apply
	aws lambda invoke --endpoint http://localhost:4566 --region=us-east-2 --function-name=ServerlessExample --payload {} output.txt
	jq . output.txt

.terraform: provider.tf
	terraform init

example.zip: example.py
	zip $@ $^

clean: destroy
	-rm -f example.zip terraform.tfstate terraform.tfstate.backup output.txt
	-rm -rf .terraform
