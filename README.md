# LabLB: Laboratorio para crear un Balanceador de Carga
En este tutorial mostraré los pasos para crear un Balanceador de carga en AWS
El objetivo es balancear la carga entre dos instancias 

## A. Preparación
### Launch template 
En este laboratorio vas a utilizar el template que creaste en el laboratorio anterior. LabEC2  

### Security Group
A security group acts as a virtual firewall for your Load Balancer to control inbound and outbound traffic

1. Seleccionar el servicio EC2
2. Seleccionar del menú del lado izquierdo "Security Groups" en la sección Network & Security
3. Seleccionar el botón naranja <Create security group>
4. Sección “Basic details”
Security group name: Lab-LB-SG
description: webserver con balanceador de carga
VPC: default
5. Sección “Inbound rules”
Seleccionar el botón <add a rule>
Adicionar el protocolo HTTP con 0.0.0.0/0
Selecciona el botón <create security group>

## B.  Creación del balanceador de carga
(1) Elastic Load Balancing (ELB) admite cuatro tipos de balanceadores de carga. Puede seleccionar el balanceador de carga adecuado en función de las necesidades de su aplicación. Si tiene que equilibrar la carga de las solicitudes HTTP, le recomendamos que utilice el **balanceador de carga de aplicaciones** (ALB). Para balancear la carga de los protocolos de red y transporte (capa 4: TCP, UDP) y para aplicaciones de rendimiento extremo y baja latencia recomendamos usar el balanceador de carga de red. Si la aplicación está creada dentro de la red clásica de Amazon Elastic Compute Cloud (Amazon EC2), debe usar un balanceador de carga clásico. Si necesita implementar y ejecutar dispositivos virtuales de terceros, puede usar el balanceador de carga Gateway.

### Target Groups
Cada Target Group se utiliza para direccionar solicitudes a uno o varios destinos registrados.

1. En el servicio EC2, selecciona del menú de la izquierda "Target Groups"
2. Selecciona el botón naranaja <create target group>
3. en "Basic configuration" 
selecciona Instances
4. Target group name: Lab-TG
5. Selecciona el botón **next**
6. selecciona el botón **create target group**

### Creación del Application Load Balancer

1. En el servicio EC2, selecciona del menú de la izquierda "Load balancers"
2. Selecciona el botón azul **Create load balancer**
3. Selecciona el botón **create** del "Application Load Balancer"
4. en "Basic Configuration"
Name: Lab-LD
5. en "Network mapping" Selecciona 
VPC: default
Mappings : selecciona todas las zonas de disponibilidad
6. en "Security Groups"
selecciona el security group Lab-LB-SG
7. en  Listeners and Routing
Protocol: HTTP
Port: 80
Default action:  Lab-TG
8. selecciona el botón naranja **Create load Balancer**


### Creación del Auto Scaling Group
1. Selecciona del menú de la izquierda "Launch Template"
2. Selecciona el template "lab-LT"
3. En el botón Actions, selecciona "Create Auto Scaling Group"
4. Auto Scaling group name: lab-AG
5. Selecciona el botón **next**
6. CUIDADO ---- OJO --- en "Network"
Selecciona todas las subnets, deberán verse de color azul
7. Purchase options and instance types: Adhere to launch template
8. Selecciona el botón **next**
9. En "Load balancing"
selecciona Attach to an existing load balancer
10. Choose a target group for your load balancer: Lab-TG
seleccionar **next**
11. en Group size
Desired capacity: 2
Minimum capacity: 2
Maximum capacity: 4
12: Scaling policies: none
13. **next**
14. **next**
15. **next**
16. En Review, selecciona **create auto scaling group**


## Revisión

1.  En el servicio EC2, selecciona del menú de la izquierda Auto Scaling Group
2. Selecciona tu ASG
3. Selecciona, del menú de la barra horizontal, **Instance Management**
4. Revisa las instancias creadas 

1. En el servicio EC2, selecciona del menú de la Load Balancer 
2. Selecciona tu balanceador de carga
3. Busca el DNS name copialo y abrelo en un navegador 
4. busca http://____.amazonaws.com/phpinfo.php, refresca la ventana varias veces y revisa el IP privado, este cambia 



Recuerda borrar todos los servicios creados, nos solamente las instancias.  Deberás eliminar el Balanceador de Carga y Auto Scaling Group y todo lo que creaste en este laboratorio.

Otro ejemplo de creación de un balanceador de carga con uso de CLI lo puedes encontrar en https://docs.aws.amazon.com/elasticloadbalancing/latest/application/tutorial-application-load-balancer-cli.html






(1) https://aws.amazon.com/es/elasticloadbalancing/faqs/?nc=sn&loc=5








