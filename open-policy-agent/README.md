# Open Policy Agent (OPA)

Credit:
[Base policies came from Ned which can be found here with a solid tutorial](https://github.com/ned1313/learning-opa-and-terraform/tree/main)

Start by having some type of data to pass along to OPA. Then we shall create REGO policies to tell OPA if the 
data/action is approved or not. 

We can add this into our CI/CD for Terraform pretty easily. Since OPA is just a binary, simply add the binary to your
container that runs terraform. Then add a workflow step that checks the *.rego file located in the directory you execute
terraform in.

### If you are calling a module in then you'll want to view the data from the below command:
```bash
terraform plan -out tf.plan && tf show -json tf.plan | jq . > tfplan.json
# If you get an error on jq (brew install jq)
# jq is a json parser
opa run tfplan.json
data.planned_values.root_module.child_modules[0].resources[keys] # Just to visialize the resources that are going to change
```

I will add more in the future! I just need some more time/ideas with how I can use OPA :) 