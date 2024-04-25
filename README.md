# Mario Bross versión 2

Super mario bross desplegado dentro de una EC2 usando Terraform + EKS

![super-mario-bross-v2](https://github.com/leorjs/mario-bross-v2/assets/119978221/bd574f6f-89f9-4106-9145-8f74c9f0fe97)



# Herramientas a usar

  + Cuenta AWS
  + Aws CLI
  + Docker
  + Kubectl
  + Terraform
  + Github

# Pasos a seguir
<details>
<summary>Creación VPC</summary>
   
    - EN CONSTRUCCIÖN

</details>

<details>
<summary>Creación Bucket S3</summary>

    - EN CONSTRUCCIÖN
</details>

<details>
<summary>Creación Role IAM</summary>
  
    - EN CONSTRUCCIÖN
</details>
<details>
<summary>Creación EC2</summary>
  
    - Launch Instances
      
      + EN CONSTRUCCIÖN
        
    - update OS
      + sudo apt update -y
        
    - Instalación de aws cli
      + curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
      + sudo apt-get install unzip -y
      + unzip awscliv2.zip
      + sudo ./aws/install
      + aws --version  --> verificación
        
    - Instalación de docker
      + apt install docker.io
      + usermod -aG docker $USER
      + newgrp docker
        
    - Instalación de kubectl
      + curl -LO https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl
      + sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
      + kubectl version --client --> Verificación
        
    - Instalación de Terraform
      + sudo apt install wget -y
      + wget -O- https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
      + echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
      + sudo apt update && sudo apt install terraform -y
</details>
<details>
<summary>Implementación dentro de la EC2</summary>
  
    - Attach IAM role en la EC2 creado
      + EN CONSTRUCCIÖN
      
    - Clonar git
      + mkdir super_mario
      + cd super_mario
      + git clone https://github.com/Aakibgithuber/Deployment-of-super-Mario-on-Kubernetes-using-terraform.git
      + cd Deployment-of-super-Mario-on-Kubernetes-using-terraform/
      + cd EKS-TF
      + Editar el archivo backend.tf file by → vim backend.tf
        - Se debe agregar el nombre del bucket antes creado
          
    - Ejecución del terraform para crear el EKS
      + terraform init
      + terraform validate
      + terraform plan
      + terraform apply --auto-approve
        
    - Updatear la configuración de EKS para conectarse al cluster
      + aws eks update-kubeconfig --name EKS_CLOUD --region us-east-1
        
    - Creación del deployment y service del mario-bross
      + kubectl apply -f deployment.yaml
      + kubectl apply -f service.yaml
        
    - Verificación de los PODs
      + kubectl get all
        
    - Buscar el LoadBalancer Ingress para acceder al juego
      + kubectl describe service mario-service
        

</details>
<details>
<summary>Eliminación de Infraestructura</summary>

    - Destruir toda la infraestructura
      + kubectl delete service mario-service
      + kubectl delete deployment mario-deployment
      + cd EKS-TF --> terraform destroy --auto-approve

</details>
