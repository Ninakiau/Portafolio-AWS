

# Implementación de Infraestructura de 3 capas en AWS

## 🌍 Roles
Administrador cloud: despliegue de una infraestructura tolerante a fallos y segura en AWS, asegurando alta disponibilidad y segregación de recursos mediante VPC, subnets y servicios de conectividad.

## 🏛️ Arquitectura de 3 Capas
![alt text](/Arquitectura3capas/img/image-7.png)

La arquitectura de 3 capas es un modelo ampliamente utilizado en infraestructura de aplicaciones web, ya que mejora la seguridad, escalabilidad y mantenibilidad del sistema. Se divide en:

- **Capa de Presentación (Frontend):** Es la interfaz con la que interactúan los usuarios. En este proyecto, el Web Server en la subnet pública ejecuta esta capa mediante Apache y PHP, manejando solicitudes HTTP/S de los clientes.

- **Capa de Aplicación (Backend):** Contiene la lógica de negocio y procesamiento de datos. En esta implementación, el App Server ubicado en la subnet privada recibe peticiones del Web Server y las procesa antes de comunicarse con la base de datos.

- **Capa de Datos:** Es el almacenamiento de la información del sistema. En este proyecto, la base de datos en Amazon RDS almacena y gestiona los datos, permitiendo accesos solo desde el App Server y Bastión Host para mayor seguridad.

Este enfoque permite mayor modularidad, facilita la escalabilidad y mejora la seguridad al restringir el acceso directo a los datos solo a la capa de aplicación.


## 🚀 Descripción del Proyecto
El objetivo de este proyecto fue implementar una infraestructura segura y de alta disponibilidad en AWS utilizando una VPC con segmentación en subnets públicas y privadas, asegurando el acceso restringido a los recursos internos. La solución permite una administración eficiente del tráfico de red y protege los datos mediante una correcta configuración de seguridad y acceso.

## 🔧 Skills y Deliverables
### **Servicios Usados:**
- **Networking:** Amazon VPC, Subnets, Internet Gateway, NAT Gateway
- **Computación:** Amazon EC2 (Bastion Host, Web Server, App Server)
- **Bases de Datos:** Amazon RDS (MariaDB)
- **Seguridad:** Security Groups, SSH Key Management
- **Alta Disponibilidad:** Multi-AZ para redundancia y tolerancia a fallos

## 🗺️ Pasos Fundamentales Seguidos
1. **Configuración de la Red (VPC & Subnets):**  
   - Creación de una VPC con 4 subnets (1 pública y 3 privadas) en 2 Availability Zones.
   - Configuración de tablas de ruteo para permitir el acceso a internet desde la subnet pública y restringir la privada.

   
2. **Seguridad y Conectividad:**  
   - Implementación de un Internet Gateway para acceso público.
   - Configuración de un NAT Gateway con Elastic IP para permitir a las subnets 
   privadas acceder a internet de manera segura.

   *Características de la IP elástica:*
![alt text](/Arquitectura3capas//img/image-6.png)

   
   - Definición de grupos de seguridad para controlar el tráfico entre servidores:
     - **Bastion Host:** Permite acceso SSH seguro.
     - **Web Server:** Expone la aplicación al cliente final.
     - **App Server:** Maneja la lógica de negocio.
     - **Database Server:** Restringido solo al App Server.

*Listado de instancias:*

![alt text](/Arquitectura3capas//img/image-4.png)

*Listado de grupos de seguridad:*

![alt text](/Arquitectura3capas//img/image-5.png)

 *Tabla de ruteo:*

![alt text](/Arquitectura3capas/img/image.png)


   3. **Implementación de Servidores (EC2 & RDS):**  

   - Creación de un Bastion Host en la subnet pública para acceso seguro.

   - Instalación y configuración de Apache y PHP en el Web Server.

   ````bash

    #!/bin/bash

    sudo yum update -y

    sudo amazon-linux-extras install -y lamp-mariadb10.2-php7.2 php7.2

    sudo yum install -y httpd

    sudo systemctl start httpd 

    sudo systemctl enable httpd

   ````


   - Configuración de un App Server con MariaDB para la capa lógica de la aplicación.

   ```` bash

   #!/bin/bash

    sudo yum install -y mariadb-server

    sudo service mariadb start 

   ```` 

   - Creación de una instancia RDS con MariaDB restringida al App Server.

   

4. **Automatización y Configuración:**  

   - Uso de User Data para instalar paquetes automáticamente en el Web Server.

   - Carga de claves SSH en el Bastion Host para acceso seguro.



     *Hacemos ping desde Bastion Host para verificar la conexión:*

   ![alt text](/Arquitectura3capas//img/image-2.png)

 



   ````bash

   #Subir claves SSH en Bastion Host:

   scp -i "C:\Users\user\Downloads\labsuser.pem" "C:\Users\user\Downloads\labsuser.pem" ec2-user@ip-pública-bastionHost:/home/ec2-user/



   #Conectarse al App Server desde Bastion Host:

   ssh -i labsuser.pem ec2-user@ip-privada-app-server

   ````

   - Configuración de conexiones remotas a la base de datos usando credenciales seguras.



   ````

   mysql –user=root -password=********* –host=endpoint-base-de-datos

   ```` 

   *Verificamos conección a la base de datos:*

   ![alt text](/Arquitectura3capas//img/image-3.png)

## Estimación de Costos


## 📌 Conclusión
Este proyecto demuestra cómo una arquitectura en la nube bien segmentada y automatizada en AWS puede transformar la infraestructura de una aplicación. La división en subnets públicas y privadas, junto con la implementación de un Bastion Host, permite un control preciso de los accesos y una administración segura. La integración de instancias EC2 para el procesamiento y Amazon RDS para la gestión de datos, combinada con herramientas de automatización y configuraciones de seguridad (Security Groups y NAT Gateway), crea un entorno robusto, escalable y resiliente. Este enfoque integral no solo garantiza la protección de los datos críticos, sino que también facilita la operación, el mantenimiento y la evolución futura de la infraestructura conforme a las demandas del negocio.


