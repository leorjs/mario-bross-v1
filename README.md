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


**[!IMPORTANT]** ⤵
> Hay que modificar el Security Group que se crea automanticamente
  
  ![image](https://github.com/leorjs/mario-bross-v2/assets/119978221/d12c32f4-6e40-484d-996b-accfdea7104a)

  + Se agrega en las reglas del Inbound lo siguiente:
    
    ![image](https://github.com/leorjs/mario-bross-v2/assets/119978221/77dac998-7217-4cab-831c-34e796aaf9a4)


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
  
- En el search del dashboard buscamos el servicio de IAM
      
  ![image](https://github.com/leorjs/mario-bross-v2/assets/119978221/3bab42df-3ffe-448e-a1bc-30b6cb4e77e4)
   
- Luego vamos a la opción Access management --> Roles
  
  ![image](https://github.com/leorjs/mario-bross-v2/assets/119978221/9b4e7878-8e96-460d-b313-5b8ee4961509)

- Creamos un role > Create role
  
  ![image](https://github.com/leorjs/mario-bross-v2/assets/119978221/81af3158-3759-46a0-b1e7-f0c7d3a48b37)

- Seleccionamos la opción AWS service y abajo en Use case seleccionamos --> ec2 --> NEXT
  
  ![image](https://github.com/leorjs/mario-bross-v2/assets/119978221/1a6f1f57-8f97-467b-b4ef-0d2b444a2213)

- Agregamos el permiso de AdministratoAccess

  ![image](https://github.com/leorjs/mario-bross-v2/assets/119978221/8d1c61e1-99af-4dcb-b8e6-92ac0bce5d7b)

- Luego colocamos el nombre del Role y las demás opciones la dejamos por default y le damos --> CREATE

  ![image](https://github.com/leorjs/mario-bross-v2/assets/119978221/3104a047-c3c6-463b-aee8-d6cec2bac579)


</details>
<details>
<summary>Creación EC2 e Instalación de tools</summary>
  
- Launch Instances
  
  + Vamos al dashboard de aws de nuevo y en search buscamos EC2
  + Luego en la pantalla principal de EC2 vemos un cuadro naranja que dice: Launch instance y le damos click
    ![image](https://github.com/leorjs/mario-bross-v2/assets/119978221/487f24d1-221f-4ae1-b010-607dcd9b6a79)
  + Agregamos un nombre a nuestra EC2 y seleccionamos en OS --> UBUNTU
    ![image](https://github.com/leorjs/mario-bross-v2/assets/119978221/e1acb832-65e0-4e9a-a640-2986a6ff9a36)
  + La imagen y el tipo de instance lo dejamos por default
    ![image](https://github.com/leorjs/mario-bross-v2/assets/119978221/153f5c3e-a083-4bcb-9bb2-3d1791b49686)
  + Para el siguiente punto Key pair crea un nuevo key pair con la extensión .PEM y lo descargas, luego lo seleccionas
    ![image](https://github.com/leorjs/mario-bross-v2/assets/119978221/95d4065f-6299-4a14-906b-b4152707559d)
  + Para el siguiente punto --> Network setting le damos edit
      + VPC --> agregamos la VPC que ya habiamos creado
      + Subnet --> dejalo por default pero verifica que este en una subnet public
      + Auto-assing public IP --> cambiar a ENABLE (importante este punto)
      + Firewall (security group) --> Selecciona el SG que antes ya creamos
    ![image](https://github.com/leorjs/mario-bross-v2/assets/119978221/a67f1a4b-fb6e-4cf7-839e-64bf0e18db2b)
  + Luego vamos a la opción Advenced details y buscamos la siguiente opción para tildarlo
    ![image](https://github.com/leorjs/mario-bross-v2/assets/119978221/121f0e5d-e6d3-4daf-bb7c-d399ff67ca4d)
  + Luego al final damos click --> Lanch instance
    
- Ingresar a la EC2
  + Vía ssh
    + Vamos al dashboard de las instance y tildamos nuestra nueva EC2 --> y luego en la parte superior le damos CONNECT
      ![image](https://github.com/leorjs/mario-bross-v2/assets/119978221/f302b5d7-4c17-4f14-8d65-71e86a7743b3)
    + Vamos a la opción EC2 instance Connect y le damos connect, automaticamente entraran a la EC2

  + Vía console AWS
    + Vamos al dashboard de las instance y tildamos nuestra nueva EC2 --> y luego en la parte superior le damos CONNECT
    + Luego vamos a la opción de SSH Client, abajo tendran un example para conectarse.
      + `ssh -i "key-pair.pem" ubuntu@<Reemplazar-la-ip-publica-de-la-ec2>`
      + Nota importante: Deben estar en la ruta donde se encuentra el archivo .pem o en su defecto en el comando agregar el path.
- update OS
  + Al momento de ingresar vía ssh a nuestra EC2 lo primero que debemos hacer es hacer un update del OS
    + command: $`sudo apt update -y`
    
- Instalación de aws cli
  + curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
  + `sudo apt-get install unzip -y`
  + unzip awscliv2.zip
  + sudo ./aws/install
  + `aws --version`  --> verificación
    
- Instalación de docker
  + `apt install docker.io`
  + `usermod -aG docker $USER`
  + `newgrp docker`
    
- Instalación de kubectl
  + curl -LO https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl
  + `sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl`
  + `kubectl version --client` --> Verificación
    
- Instalación de Terraform
  + curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -
  + echo "deb [arch=amd64] https://apt.releases.hashicorp.com jammy main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
  + `sudo apt update && sudo apt install terraform -y`
</details>
<details>
<summary>Implementación dentro de la EC2</summary>
  
- Attach IAM role en la EC2 creado
  + Vamos al dashboard de EC2 y tildamos nuestra EC2 luego vamos a la opción Actions --> Security --> Modify IAM role
    ![image](https://github.com/leorjs/mario-bross-v2/assets/119978221/34987418-40c4-44f3-9d49-78afd7383edf)
  + Luego seleccionamos nuestro Role que previamente ya creamos y le damos Update IAM role
    ![image](https://github.com/leorjs/mario-bross-v2/assets/119978221/4c57dbc4-1c93-40ed-a5af-03cec9810274)

  
- Clonar git
  + mkdir super_mario
  + cd super_mario
  + git clone https://github.com/Aakibgithuber/Deployment-of-super-Mario-on-Kubernetes-using-terraform.git
  + cd Deployment-of-super-Mario-on-Kubernetes-using-terraform/
  + cd EKS-TF
  + Editar el archivo backend.tf file by → vim backend.tf
    - Se debe agregar el nombre del bucket antes creado
  + Cambiar la región en el archivo de provider.tf --> us-east-1
      
- Ejecución del terraform para crear el EKS
  + terraform init
  + terraform validate
  + terraform plan
  + terraform apply --auto-approve
    
- Updatear la configuración de EKS para conectarse al cluster
  + aws eks update-kubeconfig --name EKS_CLOUD --region <REGION-CONFIGURADA>
    
- Creación del deployment y service del mario-bross
  + kubectl apply -f deployment.yaml
  + kubectl apply -f service.yaml
    
- Verificación de los PODs
  + kubectl get all
    
- Buscar el LoadBalancer Ingress para acceder al juego
  + kubectl describe service mario-service
    ![image](https://github.com/leorjs/mario-bross-v2/assets/119978221/a8fd5c83-281d-4102-9065-b5cd5ff7b11a)
  + Copien la url del LoadBalancer Ingress y lo pegan en un brower, y taran tendrán super mario bross corriendo
        

</details>
<details>
<summary>Eliminación de Infraestructura</summary>

- Destruir toda la infraestructura
  + kubectl delete service mario-service
  + kubectl delete deployment mario-deployment
  + cd EKS-TF --> terraform destroy --auto-approve
  + Eliminar la EC2 
  + Eliminar la VPC --> Recuerden eliminar el Elastic IP
  + Eliminar el Bucket S3 antes creado

</details>
