# Explorar Usuarios y Grupos (AWS AMI)

Para explorar los usuarios y grupos, hemos de entrar al menu **Services** y seleccionar **IAM**

En el panel de navegación de la izquierda, elija **Users**.

Al seleccionar un usuario, podremos ver los permisos que este tiene

Para ver los grupos, entraremos en el apartado de **Users Groups**

Dentro del grupo podremos ver los **usuarios** que tiene en un apartado y en el otro podremos ver los **permisos** de este grupo

En el apartado de permisos, podremos añadir o remover los permisos deseados

# Crear una VPC y lanzar un servidor WEB

> Una VPC es una red virtual prácticamente idéntica a una red tradicional que podría operar en su propio centro de datos. Una vez creada una VPC, podrá agregar las subredes.

Para crear una VPC, entraremos en el menu de VPC. Y seleccionaremos la opción de crear una VPC.

Dentro de este pondremos los siguientes paramentros:

- Nombre del proyecto
- El bloque de CIDR Ipv4 (Determine la IP inicial y el tamaño de la VPC mediante la notación CIDR.)
- Bloque de CIDR IPv6
- Número de zonas de disponibilidad (cantidad de zonas de disponibilidad en las que se desea aprovisionar subredes)
- Cantidad de subredes públicas
- Cantidada de subredes privadas

Después lo que haremos será configurar los bloques de CIDR de subredes, es decir las ips que habran

- Gateways NAT ($)
- Opciones de DNS

Si queremos ver la vista previa de las subreds la tenemos ubicada a la derecha

## Creación de subredes

Para crear subredes, vamos al panel de subredes y le damos a crear subred.

Una vez llegados a aquí seleccionaremos el VPC del cual queremos partir.

Ahora rellenaremos los campos de la subred (nombre, zona de disponibilidad y el bloque CIDR Ipv4)

## Tablas de enrutamiento

Para ver estas tablas lo que tendremos que hacer será entrar al apartado de la izquierda llamado **Route tables**

## Creación un grupo de seguridad VPC

Entramos a crear un grupo de seguridad en el apartado de la izquierda, luego le asignamos un nombre y descripción. Luego seleccionamos la VPC a la cual queremos asignarle el grupo.

Y ahora simplemente le damos a **Add rule** para añadir la regla y le damos a crear

## Lanzar una instancia con un Web Server

Entramos al servicio EC2, en el cual crearemos una instancia y seleccionaremos el tipo de instancia que queramos y también elegiremos el sistema operativo

Seguido de este, seleccionaremos el par de llaves

Y configuraremos la red (si queremos enlazar una VPC), junto al grupo de seguridad.

Finalmente cargaremos un script entrando en el apartado **Advanced details** y en el cuadro de **User data box** cargaremos el siguiente script

```bash
#!/bin/bash

# Install Apache Web Server and PHP

yum install -y httpd mysql php

# Download Lab files

wget https://aws-tc-largeobjects.s3.us-west-2.amazonaws.com/CUR-TF-100-ACCLFO-2-9026/2-lab2-vpc/s3/lab-app.zip

unzip lab-app.zip -d /var/www/html/

# Turn on web server

chkconfig httpd on

service httpd start
```

# Amazon EC2

## Actualizar los grupos de seguridad

Para crearla lo que haremos sera entrar a grupos de seguridad (**Security Groups**) y allí.

Si queremos crear uno para el puerto 80, seleccionaremos el HTTP. Y le pondremos de origen quien se quiere conectar, en este caso el origen sera *Anywhere-IPv4*

Y finalmente le daremos a agragar regla

Finalmente entraremos a la instancia y le asignaremos el grupo de seguridad que acabamos de crear.

## Cambiar tamaño de la instancia (tipo de instancia y Volumen EBS)

Primero que nada tendremos que detener la instancia, seleccionar la instancia y darle a **Actions** y luego a **Instance settings** y finalmente a **Change instance type**.

Aqui podremos cambiar el tipo de instancia y una vez este cambiada le daremos a aplicar.

### Cambiar tamaño Volumen EBS

Entraremos a **Storage* y seleccionaremos el nombre del Volumen (ID)

Y le daremos a **Actions** donde seleccionaremos la opción de **Modify Volume**.
Despues donde dice *size* cambiaremos el tamaño al desead y haremos click en **Modify**.

# Trabajo con EBS

## Creación de un volumen EBS

Para ello tendremos que entrar al servicio EC2, en el cual lo que deberemos de hacer es entrar en el apartado de **volumes** y le damos click al botón de crear.

Aquí seleccionaremos los siguientes apartados:

- Tipo de volumen
- Tamaño
- Zona de disponibilidad
- ID de la instantánea (por si se quiere crear un volumen a partir de una instantánea)
- Cifrado (si queremos cifrarlo o no)
- Etiquetas (por ejemplo name -> para poner el nombre)

Finalmente le damos a crear

Luego seleccionaremos el volumen y le daremos a **actions** y luego a **Atatch volume** para poder asignarlo a una instancia


## Montar volumen en linux

Primero lo que haremos será darle formato

```bash
sudo mkfs -t ext3 /dev/sdf
```

Despues lo que haremos sera crear un directorio donde montaremos el volumen

```bash
sudo mkdir /mnt/data-store
```

Finalmente montamos el volumen+

```bash
sudo mount /dev/sdf /mnt/data-store
```

## Crear un EBS Snapshot

> Un Snapshot es una copia instantanea de un volumen o estado de un sistema en un estado determinado 

A la hora de hacer la instantánea, hay que indicar la descripción junto a las etiquetas (funcionan igual que a la hora de crear un volumen)

## Crear un volumen a partir de instantánea

Para crear un volumen a partir de una instantánea funciona de la misma manera que a la hora de crear el volumen. Solo que a la hora de crearlo, hemos de indicar en el apartado de instantanea cual queremos usar.

Y finalmente lo asignamos



















# Creación de un servidor de bases de datos

## Creación grupo de seguridad para la base de datos

Asignamos un nombre al grupo, y seguido de este una descripción. Finalmente seleccionamos a que VPC queremos asignarle el grupo

Una vez asignada la VPC, procederemos a añadir la regla de entrada, a esta le asignaremos el tipo (en nuestro caso MYSQL/AURORA. Seguido de este el origen (en nuestro caso escribiremos sg y seleccionaremos la Web Security Group). Y hacemos clic en crear

## Cración de una subnet de grupo DB

>Una subnet de grupo DB se usa para decirle a RDS qué subredes se pueden usar para la base de datos.

Primero que nada tendremos que acceder al menú RDS donde seleccionaremos la opción **Subnet groups**, que esta en el menú del lateral izquierdo. Y le daremos a crear un grupo, aqui asignaremos los siguientes datos:

- Nombre
- Descripción
- VPC (Que VPC queremos utilizar)

Después bajaremos hasta donde dice **Add Subnets**, aqui introduciremos los siguientes datos

- Zona de disponiblidad (Podemos seleccionar más de una)
- Subredes (Podemos seleccionar varios)

Finalmente le damos click en crear

## Crear una instancia DB en RDS 

Para crear una base de datos lo que haremos será en el panel izquierdo seleccionar **Databases**. Y le daremos a crear, una vez entrado en el menú seleccionaremos si queremos un método de creación estándar o sencilla.

Dentro seleccionaremos las siguientes opciones:

-Tipo de motor (Mysql, Aurora, MariaDB, Oracle...)
- Seleccionaremos si queremos algún filtro
- Nombre de la instancia
- Nombre se usuario maestro (amin)
- Luego pondremos la contraseña
- Seleccionaremos el tipo de instancia
- Configuración de Almacenamiento (SSD)
- Configuracioón de Conectividad (VPC)
    - Seleccionamos el grupo de subredes de la base de datos
    - Seleccionamos el grupo de seguridad
- Configuraciín de autentificación
- Supervisión
- Configuración adicional (Opciones de la base de datos)

Finalmente le damos al botón de crear

Depués de habilitar

- Copias automáticas
- Cifrado
- Monitoreo

Una vez creada, haremos click en el nombre y copiaremos el endpoint, que será algo parecido a este

lab-db.cfi1ewb6dmff.us-east-1.rds.amazonaws.com.

Luego nos conectaremos al servidor web haciendo uso del endpoint, el nombre de la base de datos, usuario y contraseña


## Creacion grupos de destino / groups target (balanceador de carga)

Para ello hacemos click en crear grupo y alli seleccionamos el tipo:

- Instancias
- Direcciones ip
- Lambda function

Luego asignaremos el nombre del grupo y seleccionaremos la VPC

Finalmnente le daremos **next** y **create**

## Crear balanceador de carga

Para ello entraremos a la opción del menú de EC2 y seleccionaremos la opción de crear balanceador de carga

Una vez aqui dentro seleccionaremos el tipo (en este caso de aplicación) y le daremos a crear.

Después asignaremos el nombre y la VPC

Y seleccionaremos las subredes las dos como públicas

Una vez llegados a **security groups** seleccionaremos el grupo creado anteriormente y quitaremos el default, seguido de esto pondremos a escuchar el puerto 80 del grupo

Finalmente le daremos click a crear y a visualizar

## Autolanzamiento

### Crear configuración de lanzamiento

Primero que nada haremos click en crear grupo de configuración de lanzamiento en el menú y le daremos click a crear.

Luego le asignaremos el nombre y seleccionaremos la AMI (imagen creada anteriormente), seleccionaremos el tipo de instancia (t2.micro)

Y en configuración adicional activamos el monitoreo de CloudWatch

En grupos de seguridad seleccionaremos el grupo de seguridad creado anteriormente

Despúes elegiremos el par de llaves existentes y seleccionaremos el nuestro, finalmente marcamos el consentimiento y lo creamos

### Crear grupo de seguridad

Para crearlo una vez cerado el lanzamiento, lo seleccionaremos y haremos click en **actions**, aquí seleccionaremos **Create Auto Scaling group**

Una vez dentro asignaremos un nombre, seleccionamos VPC y zona de disponibilidad (seleccionaremos las 2 subredes privadas)

Seleccionamos asociar a un balanceador de carga existente, y seleccionaremos el grupo creado anteriormente

Y habilitamos la recopilación de métricas de grupo en CloudWatch

Configuramos el tamaño del grupo

Creamos una politica de escalado y con **CPU average** y 60 **value**, le damos a siguiente 2 veces y añadimos una etiqueta

    Key: Name
    Value: Lab Instance

Seleccionamos siguiente y crear

## Verificar balanceador de carga

Entraremos en instancias y deberia de aparecer las dos instancias del balanceador

Luego entraremos en **target groups** y dentro de este deberia de haber 2 instancias

Luego en **Load Balancers** copiaremos la DNS y probaremos si funciona

## Probar balanceadores de carga

Para ello entraremos en servicios y seleccionaremos **CloudWatch** y luego en *All alarms* (donde saldran las dos alarmas)

Si no vemos las alarmas

     Note: Please follow these steps only if you do not see the alarms in 60 seconds.

    - On the  Services  menu, click EC2.
    - In the left navigation pane, choose Auto Scaling Groups. 
    - Select  Lab Auto Scaling Group.
    - In the bottom half of the page, choose the Automatic Scaling tab.
    - Select  LabScalingPolicy.
    - Click Actions  and Edit.
    - Change the Target Value to 50.
    - Click Update
    - On the  Services  menu, click CloudWatch.
    - In the left navigation pane, click All alarms and verify you see two alarms.


Y en CloudWatch Management nos mostrará cual de las que hay está en estado alto o en modo alarma