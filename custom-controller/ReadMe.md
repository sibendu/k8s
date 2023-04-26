# Creating Kubernetes Custom Resource Definition and Controller

Build the Sample Controller and Run. It can be run locally, or deployed to K8S
	gradle clean build
Then,
	java -jar build/libs/samplecontroller-1.0.jar
or, 
	docker build -t sibendu/samplecontroller .
	kubectl apply -f samplecontroller.yaml


kubectl apply -f crd.yaml

kubectl apply -f cr.yaml

kubectl get configmap -> my-config-map created holding my-own-property from the CR

It gets updated when we update the custom resource i.e. cr.yaml
	