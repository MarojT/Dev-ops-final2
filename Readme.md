# This repository is part of DevOps and Microservices course.  

## Service Mesh with HashiCorp Consul
  
Demo project accompanying a [Consul crash course video](https://www.youtube.com/watch?v=s3I1kKKfjtQ) on YouTube

### Terraform commands to execute the script

```sh
# initialise project & download providers
terraform init

# preview what will be created with apply & see if any errors
terraform plan

# exeucute with preview
terraform apply -var-file terraform.tfvars

# execute without preview
terraform apply -var-file terraform.tfvars -auto-approve

# destroy everything
terraform destroy

# show resources and components from current state
terraform state list
```

#### Get access to EKS cluster
```sh
# install and configure awscli with access creds
aws configure

# check existing clusters list
aws eks list-clusters --region eu-central-1 --output table --query 'clusters'

# check config of specific cluster - VPC config shows whether public access enabled on cluster API endpoint
aws eks describe-cluster --region eu-central-1 --name myapp-eks-cluster --query 'cluster.resourcesVpcConfig'

# create kubeconfig file for cluster in ~/.kube
aws eks update-kubeconfig --region eu-central-1 --name myapp-eks-cluster

# test configuration
kubectl get svc
```

### 📂 Kubernetes
The microservices and supporting configurations are deployed to Kubernetes. Here's a breakdown:

`config.yaml` -> Base service and deployment definitions (pre-Consul).

`config-consul.yaml` -> Modified manifests that include Consul annotations.

`consul-values.yaml` ->   Helm values used to configure Consul installation.

`consul-mesh-gateway.yaml` -> Defines a gateway for cross-cluster or multi-datacenter service communication.

`exported-service.yaml` -> Makes services accessible across clusters.

`service-resolver.yaml` -> Provides fallback or failover behavior for services between environments (e.g., Linode → EKS).

### 📂 Terraform
`main.tf` -> Core configuration combining modules and resources.

`providers.tf` -> Declares used providers like AWS, Helm, and Kubernetes.

`variables.tf` -> Defines configurable input variables.

`terraform.tfvars` -> Contains values for variables such as AWS credentials.

### Screenshorts of the Service Interface
![Home page](![image](https://github.com/user-attachments/assets/98b03045-4b20-4e8c-9378-83ad63f823d3))
![Cart page](![image](https://github.com/user-attachments/assets/1401679c-9770-485b-a0ff-e31505e79732)
![EKS Service page](![image](https://github.com/user-attachments/assets/5954fa61-8721-4b9e-b6e7-d69d8bf59918)
![LKE Service page](![image](https://github.com/user-attachments/assets/e6d15621-30fe-4f8f-abf1-005772e8d885)
)

)

## GCP source codes
This [demo project](https://github.com/GoogleCloudPlatform/microservices-demo) made by Google Cloud Platform.

**Online Boutique** is a cloud-first microservices demo application.  The application is a
web-based e-commerce app where users can browse items, add them to the cart, and purchase them.

| Service                                              | Language      | Description                                                                                                                       |
| ---------------------------------------------------- | ------------- | --------------------------------------------------------------------------------------------------------------------------------- |
| [frontend](/src/frontend)                           | Go            | Exposes an HTTP server to serve the website. Does not require signup/login and generates session IDs for all users automatically. |
| [cartservice](/src/cartservice)                     | C#            | Stores the items in the user's shopping cart in Redis and retrieves it.                                                           |
| [productcatalogservice](/src/productcatalogservice) | Go            | Provides the list of products from a JSON file and ability to search products and get individual products.                        |
| [currencyservice](/src/currencyservice)             | Node.js       | Converts one money amount to another currency. Uses real values fetched from European Central Bank. It's the highest QPS service. |
| [paymentservice](/src/paymentservice)               | Node.js       | Charges the given credit card info (mock) with the given amount and returns a transaction ID.                                     |
| [shippingservice](/src/shippingservice)             | Go            | Gives shipping cost estimates based on the shopping cart. Ships items to the given address (mock)                                 |
| [emailservice](/src/emailservice)                   | Python        | Sends users an order confirmation email (mock).                                                                                   |
| [checkoutservice](/src/checkoutservice)             | Go            | Retrieves user cart, prepares order and orchestrates the payment, shipping and the email notification.                            |
| [recommendationservice](/src/recommendationservice) | Python        | Recommends other products based on what's given in the cart.                                                                      |
| [adservice](/src/adservice)                         | Java          | Provides text ads based on given context words.                                                                                   |
| [loadgenerator](/src/loadgenerator)                 | Python/Locust | Continuously sends requests imitating realistic user shopping flows to the frontend.                                              |
