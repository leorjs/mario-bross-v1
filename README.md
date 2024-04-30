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


- Acceder a la consola de aws y buscar el servicio de VPC

![image](https://github.com/leorjs/mario-bross-v2/assets/119978221/99343f49-668d-4f98-8ba4-2ba84bc00270)

- Luego en la parte superior derecho vamos a <create vpc>

![image](https://github.com/leorjs/mario-bross-v2/assets/119978221/fbe648ed-5a84-4cec-9e37-892a314a228a)

- Crearemos una VPC standar

![image](https://github.com/leorjs/mario-bross-v2/assets/119978221/a173abde-52ba-4cfd-9c77-2d54b9d236ea)
![image](https://github.com/leorjs/mario-bross-v2/assets/119978221/96b20b3b-a47a-4fad-83e0-4a871b44454f)

- Preview Map

![image](https://github.com/leorjs/mario-bross-v2/assets/119978221/85f939b3-3652-4709-bbad-219f09db7087)

</details>

<details>
<summary>Creación Bucket S3</summary>

- En la consola de AWS en la parte superior izquierda colocar <s3>
- Click a la opción <Create bucket> esta en la parte derecha de la consola
  + Bucket type --> General purpose
  + Bucket name --> super-mario-bross-ec2-v1 (Este nombre del bucket se agregara en el terraform)
  + Object Ownership --> ACLS disable (recommended)
  + Block Public Acces setting for this bucket
    - Tildar la opción --> Block all public access (normalment esta tildado)
  + Bucket Versioning --> Disable
  + Default encrytion
    - Server-side encryption with Amazon S3 managed keys (SSE-S3) >> Tildar
    - Bucket Key --> Disable
- click --> Create bucket
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
  + curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -
  + echo "deb [arch=amd64] https://apt.releases.hashicorp.com jammy main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
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
