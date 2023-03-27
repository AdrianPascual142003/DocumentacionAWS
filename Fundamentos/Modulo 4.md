## Software como servicio(SaaS)

>MFA de IAM (autentificación multifactor)

simpre minimos permisos

### Tipos de políticas

Hay 2 tipos de politicas

**Identity and Access Management**

Pueden haber muchos usuarios y muchos grupos

Los grupos contienen politicas

Una política concede permisos a un rol de IAM que a su vez este rol esta asignado a una Aplicación la cual a partir de tener el rol podrá acceder al sitio de Amazon S3

### **COMO CREAR UN ROL**

Entramos a crear un rol de EC2 entramos a crearlo y seleccionamos un permiso s3 y lo añadimos al rol, el cual vamos a crear depués de seleccionar estos. Despu´es asignamos un nombre y una desc y lo creamos, luego lo asignamos a la instancia y esta ya obtendrá los permisos

### **COMO CREAR UN GRUPO**

Entramos al sitio para crear un grupo y sleccionamos todas las políticas que queremos en este, luego le introducimos los datos pertinentes, añadimos los usuarios al grupo y ya podremos añadirlo a la instancia.

También podemos crear una política a través de la sintaxis JSON

### Permisos

>PERMISO QUE ACTUALIZA LOS PERMISOS SOLOS 

This group has a Managed Policy associated with it, called AmazonEC2ReadOnlyAccess. Managed Policies are pre-built policies (built either by AWS or by your administrators) that can be attached to IAM Users and Groups. When the policy is updated, the changes to the policy are immediately apply against all Users and Groups that are attached to the policy.

**AWS Organizations** nos permite consolidar varias cuentas AWS para administrar de forma centralizada 

**SCP** nos Ofrece control centralizado sobre las cuentas

**AWS Key management|| AWS KMS** --- Crear y administrar claves de cifrado

**Amazon Cognito** --- Control de acceso y de inicio de sesión y registro de usuarios

### Protecciones

**WAS Shield** nos proporciona Proteccion contra ataques (DDOS)


> CIFRADO DE DATOS EN REPOSO

Cifrado codifica los datos con una clave secreta

>CIFRADO DE DATOS EN TRÁNSITO

Se usa Trasport Layer Security (TLS) cifraolos datos que migran a través de una red

>PROTECCIÓN DE BUCKETS Y OBJETOS AMAZON S3

buckets y objetos son privados y están protegidos

Siempre se asignan los privilegios mínimos