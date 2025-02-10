AWS Infrastructure Lab

🚀 Descripción

Este laboratorio implementa una infraestructura segura y altamente disponible en AWS, utilizando VPC, subnets, NAT Gateways, EC2, RDS y otros servicios clave.

📌 Arquitectura Implementada

VPC y Redes

Creación de una VPC con 4 subnets (1 pública, 3 privadas) distribuidas en 2 Availability Zones.

Asignación de Elastic IP y configuración de un NAT Gateway para acceso seguro desde subnets privadas.

Implementación de un Internet Gateway y configuración de tablas de ruteo.

Definición de grupos de seguridad para controlar el acceso entre servidores.

Instancias EC2

Bastion Host en subnet pública para acceso seguro a servidores internos.

Web Server con Apache y PHP, configurado vía User Data.

App Server con MariaDB para interacción con la base de datos.

Base de Datos (RDS)

Creación de un subnet group.

Implementación de una instancia MariaDB, accesible solo desde el App Server.

Seguridad y Accesos

Carga de claves SSH en Bastion Host.

Conexión segura desde el Bastion Host a servidores internos.

Pruebas de conectividad mediante ping y acceso a la base de datos.

🛠️ Tecnologías Utilizadas

AWS VPC

EC2

RDS (MariaDB)

NAT Gateway

Internet Gateway

Grupos de Seguridad

📖 Instrucciones

Configurar la VPC con las subnets necesarias.

Lanzar instancias EC2 con las configuraciones adecuadas.

Configurar RDS y conectarlo al App Server.

Asegurar accesos con SSH y realizar pruebas de conectividad.

📌 Contribuciones

Este proyecto fue desarrollado por el equipo para mejorar habilidades en redes, seguridad y arquitectura en la nube.

📜 Licencia

Este repositorio es de uso educativo y está abierto para contribuciones.
