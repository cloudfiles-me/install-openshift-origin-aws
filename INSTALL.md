- Copy inventory, prepare.yaml files from openshift-lab to master
- sudo yum install git epel-release python-pip
- sudo pip install ansible==2.6.5
- ansible-playbook prepare.yaml -i inventory --key-file ssh_key.pem
- git clone https://github.com/openshift/openshift-ansible
- cd openshift-ansible/
- git checkout release-3.9
- ansible-playbook openshift-ansible/playbooks/prerequisites.yml -i inventory --key-file ssh_key.pem
- ansible-playbook openshift-ansible/playbooks/deploy_cluster.yml -i inventory --key-file ssh_key.pem
- sudo htpasswd /etc/openshift/openshift-passwd admin
- Deploy aws-service-broker-prerequisites.yaml CloudFormation stack (it creates a DynamoDB needed for the service broker)
- Create a EC2 assume role with Full Access, because there is something in the aws service broker that works wrong with the default IAM policy
- Attach IAM role to openshift EC2 instances (this role is used for aws service broker to assume the role and deploy the resources)
- curl -O https://raw.githubusercontent.com/awslabs/aws-servicebroker/release-v1.0.0/packaging/openshift/deploy.sh
- curl -O https://raw.githubusercontent.com/awslabs/aws-servicebroker/release-v1.0.0/packaging/openshift/aws-servicebroker.yaml
- curl -O https://raw.githubusercontent.com/awslabs/aws-servicebroker/release-v1.0.0/packaging/openshift/parameters.env
- chmod +x deploy.sh
- Edit parameters.env and update parameters as needed (change IMAGE to this value: awsservicebroker/aws-servicebroker:stable)
- ./deploy.sh

- Useful commands:
	* oc logs $(oc get pods --no-headers -o name | grep aws-servicebroker)
	* oc adm policy add-cluster-role-to-user cluster-admin admin
