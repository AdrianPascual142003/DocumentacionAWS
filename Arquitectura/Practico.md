## Creación de un bucket

Entramos a Amazon S3 y le damos a crear, acto seguido seleccionamos el nombre y región

Habilitamos los ACL para que puedan ser visibles por más cuentas y seleccionamos **Bucket owner preferred**

Desmarcamos *Bloquear todo el acceso público*, luego seleccione la casilla que dice *Acepto que la configuración actual puede hacer que este depósito y los objetos dentro se vuelvan públicos*.

Y le damos a Crear bucket.

Seleccionamos el nombre del nuevo bucket.

Seleccionamos la pestaña Propiedades.

Desplázamos hasta el panel Etiquetas.

Elija Editar, luego Agregar etiqueta e ingresamos:

**La Key y el Valor**

Desplácese hasta el panel Alojamiento de sitios web estáticos.

Elegimos Editar

Configuramos los siguientes ajustes:

    Alojamiento web estático: Habilitar

    Tipo de alojamiento: Aloja un sitio web estático

    Documento de índice: index.html
        Nota: Debe ingresar este valor, aunque ya se muestre.

    Documento de error: error.html

Y guardamos cambios

Si accedemos ahora a este nos dirá que no tenemos permisos (está por configurar aún)

### Subir archivos

Volvemos a nuestro bucket y vamos al apartado objetos y subimos los archivos

Los seleccionamos todos y le damos a **upload**

Y esperamos que se suban antes de salirnos

### Hacer públicos los archivos

Seleccionamos los tres objetos.

En el menú Acciones, elija Hacer público a través de ACL.

Se muestra una lista de los tres objetos.

Seleccionamos **Hacer público**

Y ya podremos acceder a nuestra página subida

### Actualizar la Web

En su computadora, cargue el archivo index.html en un editor de texto (por ejemplo, Notepad o TextEdit).

Hacemos algún cambio

Guardamos el archivo.

Volvemos a la consola de Amazon S3 y cargamos el archivo index.html que acabamos de editar.

Seleccione index.html y utilice el menú Acciones para elegir de nuevo la opción Hacer público a través de ACL.

Regrese a la pestaña del navegador web con el sitio web estático y actualice la página.

Y ya veremos los cambios


## Permiso Read-Only

{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::website-992/*"
        }
    ]
}

## Habilitar control de versiones de buckets

Para ello entramos a propiedades del bucket y activamos el control de versiones y le damos a guardar


# Crear un sistema EFS (sistema de archivos)

En el menú Servicios, elija EFS.

Elegimos **Crear sistema de archivos**

En la ventana Crear sistema de archivos, eligimos **Personalizar**

En el Paso 1:

    Desmarque Habilitar copias de seguridad automáticas.

    Gestión del ciclo de vida: Seleccione Ninguno

    En la sección Etiquetas, configure:
    Nombre clave
    Valor: Mi primer sistema de archivos EFS

    Elija Siguiente

Para VPC, seleccione Lab VPC.

Separe el grupo de seguridad predeterminado de cada destino de montaje de zona de disponibilidad seleccionando la casilla de verificación en cada grupo de seguridad predeterminado.

Adjunte el grupo de seguridad de destino de montaje de EFS a cada destino de montaje de zona de disponibilidad mediante:

    Seleccionanmos cada casilla de verificación Grupos de seguridad.

    Eligimos destino de montaje EFS.

    Se crea un destino de montaje para cada subred.

    Sus objetivos de montaje deben parecerse al siguiente ejemplo. El diagrama muestra dos destinos de montaje en la VPC de laboratorio que usan el grupo de seguridad EFS Mount Target. En este laboratorio, debe utilizar la VPC de laboratorio.

    Grupos de seguridad de destino

    Elija Siguiente
    En el Paso 3, elija Siguiente
    En el Paso 4:

    Revisa tu configuración.
    Elija Crear

## Montar sistema EFS en linux

En su sesión SSH, cree un nuevo directorio ingresando 

    sudo mkdir efs

De vuelta en la Consola de administración de AWS, en el menú Servicios, elija EFS.

Elija **Mi primer sistema de archivos EFS**.

En la consola de Amazon EFS, en la esquina superior derecha de la página, elija Adjuntar para abrir las instrucciones de montaje de Amazon EC2.

Copie el comando completo en la sección Uso del cliente NFS.

El comando de montaje debería parecerse a este ejemplo:

*sudo mount -t nfs4 -o nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2,noresvport fs-bce57914.efs.us-west-2.amazonaws.com:/efs*

El comando sudo mount... proporcionado utiliza las opciones de montaje predeterminadas de Linux.

En su sesión SSH de Linux, monte su sistema de archivos de Amazon EFS de la siguiente manera:
    Pegar el comando
    Presionando ENTRAR

Obtenga un resumen completo del uso del espacio en disco disponible y utilizado ingresando:

    sudo df-hT

La siguiente captura de pantalla es un ejemplo de la salida del siguiente comando del sistema de archivos de disco:

    df-hT

Observe el tipo y el tamaño de su sistema de archivos EFS montado.

## Examinar el comportamiento de rendimiento de su nuevo sistema de archivos EFS

Examine las características de rendimiento de escritura de su sistema de archivos ingresando:

    sudo fio --name=fio-efs --filesize=10G --filename=./efs/fio-efs-test.img --bs=1M --nrfiles=1 --direct=1 --sync=0 --rw=write --iodepth=200 --ioengine=libaio

 El comando fio tardará de 5 a 10 minutos en completarse. La salida debería verse como el ejemplo en la siguiente captura de pantalla. Asegúrese de examinar la salida de su comando fio, específicamente la información de estado resumida para esta prueba de ESCRITURA.

## Supervisión del rendimiento mediante el uso de Amazon CloudWatch

En la consola de administración de AWS, en el menú **Services** (Servicios), elija **CloudWatch**.

En el panel de navegación de la izquierda, elija **Metrics** (Métricas).

En la pestaña **All metrics** (Todas las métricas), elija EFS.

Elija **File System Metrics** (Métricas del sistema de archivos).

Seleccione la fila que tiene el nombre de métrica **PermittedThroughput** (Rendimiento permitido).

Es posible que deba esperar de 2 a 3 minutos y actualizar la pantalla varias veces antes de que todas las métricas disponibles, incluida PermittedThroughput (Rendimiento permitido), se calculen y rellenen.

En el gráfico, elija y arrastre la línea de datos. Si no ve el gráfico de líneas, ajuste el intervalo de tiempo del gráfico para que se muestre el periodo durante el cual ejecutó el comando fio.

elegir arrastrar

Detenga el puntero en la línea de datos del gráfico. El valor debe ser 105M.

Rendimiento

El rendimiento de Amazon EFS aumenta a medida que crece el sistema de archivos. Las cargas de trabajo basadas en archivos suelen tener picos. Impulsan altos niveles de rendimiento durante periodos cortos y bajos niveles de rendimiento el resto del tiempo. Debido a este comportamiento, Amazon EFS está diseñado para ampliar a niveles elevados de rendimiento durante ciertos periodos. Todos los sistemas de archivos, independientemente del tamaño, pueden aumentar el rendimiento a 100 MiB/s. Para obtener más información acerca de las características de rendimiento del sistema de archivos de EFS, consulte ladocumentación de Amazon Elastic File System.

En la pestaña **All metrics** (Todas las métricas)** desmarque la casilla PermittedThroughput (Rendimiento permitido).

Seleccione la casilla de verificación DataWriteIOBytes (Bytes de E/S de escritura de datos).

Si no ve DataWriteIOBytes (Bytes de E/S de escritura de datos) en la lista de métricas, utilice la búsqueda File System Metrics (Métricas del sistema de archivos) para encontrarla.

Elija la pestaña **Graphed metrics** (Métricas diagramadas).

En la columna **Statistics** (Estadísticas), seleccione **Sum** (Suma).

En la columna **Period** (Periodo), seleccione 1 Minute (1 minuto).

Detenga el puntero en el pico del gráfico de líneas. Tome este número (en bytes) y divídalo por la duración en segundos (60 segundos). El resultado le da el rendimiento de escritura (B/s) de su sistema de archivos durante la prueba.

# Crear based de datos en amazon RDS

En el menú Services (Servicios), elija RDS.

Elija **Create database** (Crear base de datos).

Si en la parte superior de la pantalla se muestra **Switch to the new database creation flow** (Cambiar al nuevo flujo de creación de bases de datos), elija esa opción.

Debajo de **Engine options** (Opciones del motor), seleccione **MySQL**.

Las opciones incluyen varios casos de uso, desde bases de datos de clase empresarial hasta sistemas de desarrollo y pruebas. En las opciones, podrá notar Amazon Aurora. Aurora es un sistema compatible con MySQL que se rediseñó para la nube. Si su empresa usa bases de datos PostgreSQL o MySQL a gran escala, Aurora puede proporcionar un rendimiento mejorado.

En la sección **Templates** (Plantillas), **seleccione Dev/Test** (Desarrollo y pruebas).

Ahora puede seleccionar la configuración de la base de datos, incluida la versión de software, las clases de instancias, el almacenamiento y la configuración de inicio de sesión. La opción de implementación Multi-AZ crea automáticamente una réplica de la base de datos en una segunda zona de disponibilidad para una alta disponibilidad. Sin embargo, en este laboratorio utilizará una instancia de base de datos única.

En la sección **Settings** (Configuración), configure estas opciones:

    DB instance identifier (Identificador de instancias de bases de datos): inventory-db
    Username (Nombre de usuario): admin
    Password (Contraseña): lab-password
    Confirm password (Confirmar contraseña): lab-password

En la sección **DB instance size** (Tamaño de la instancia de base de datos), configure estas opciones:

    Seleccione **Burstable classes** (includes t classes) (Clases ampliables [incluye las clases t]).
    Seleccione db.t2.micro

Debajo de la sección Connectivity (Conectividad), configure esta opción:Virtual Private Cloud (VPC): Lab VPC.

Amplíe **Additional connectivity configuration** (Configuración de conectividad adicional) y, a continuación, configure lo siguiente:

    Para Existing VPC security groups (Grupos de seguridad de VPC existentes): elija DB-SG. Esta opción estará resaltada.

Amplíe Additional configuration (Configuración adicional) y, a continuación, configure lo siguiente:

    Initial database name (Nombre inicial de la base de datos): inventory
    Borre (desactive) la opciónEnable Enhanced monitoring (Habilitar monitoreo mejorado).

Este es el nombre lógico de la base de datos que utilizará la aplicación.

Elija **Create database** (Crear base de datos) (en la parte inferior de la página).

## Configurar la comunicación de aplicaciones web con una instancia de base de datos

En el menú Services (Servicios), elija **EC2**.

En el panel de navegación izquierdo, elija **Instances** (Instancias).

En el panel central, debería haber una instancia en ejecución que se denomina App Server (Servidor de aplicaciones).

**Seleccione la instancia** App Server (Servidor de aplicaciones).

En la pestaña **Description** (Descripción), copie la **IPv4 Public** IP (IP pública IPv4) al portapapeles.

Sugerencia: Si pasa el cursor por la dirección IP, aparece un icono de copiar. Para copiar el valor que se muestra, elija el icono.

Abra una nueva pestaña del navegador web, pegue la dirección IP en la barra de direcciones y presione Intro.

La aplicación web debería aparecer. No muestra mucha información porque la aplicación aún no está conectada a la base de datos.

Elija **Settings** (Configuración).

Ahora puede configurar la aplicación para usar la instancia de base de datos de RDS que creó anteriormente. Primero recuperará el punto de enlace de la base de datos para que la aplicación sepa cómo conectarse a la base de datos.

Regrese a la **consola de administración de AWS**, pero no cierre la pestaña de la aplicación. Pronto tendrá que volver a ella.

En el menú **Services** (Servicios), elija **RDS**.

En el panel de navegación izquierdo, elija **Databases** (Bases de datos).

Elija inventory-db.

Desplácese hasta la sección **Connectivity & Security** (Conectividad y seguridad) y copie el **Endpoint** (Punto de enlace) en el portapapeles.

Debería ser similar a este ejemplo: inventory-db.crwxbgqad61a.rds.amazonaws.com

Vuelva a la pestaña del navegador con la aplicación de inventario y escriba estos valores:

    Endpoint (Punto de enlace): pegue el punto de enlace que copió anteriormente
    Database (Base de datos): inventory (inventario)
    Username (Nombre de usuario): admin
    Password (Contraseña): lab-password
    Elija Save (Guardar).

Ahora la aplicación se conectará a la base de datos, cargará algunos datos iniciales y mostrará información.

Agregue el inventario, edite y elimine la información de inventario usando la aplicación web.

La información de inventario está almacenada en la base de datos MySQL en Amazon RDS que creó anteriormente en este laboratorio. Esto significa que cualquier error en el servidor de la aplicación no perderá ningún dato. También significa que varios servidores de aplicaciones pueden acceder a los mismos datos.

Inserte nuevos registros en la tabla. Asegúrese de que la tabla tenga 5 registros de inventario o más antes de enviar su trabajo.

Ya lanzó correctamente la aplicación y la conectó a la base de datos.

Opcional: puede acceder a los parámetros guardados en la consola de Systems Manager debajo de Parameter Store (Almacén de parámetros).


# Crear una VPC

Comenzará por utilizar Amazon VPC para crear una nueva nube virtual privada o VPC.

Una VPC es una red virtual dedicada a su cuenta de Amazon Web Services (AWS). Está aislada de manera lógica de otras redes virtuales en la nube de AWS. Puede lanzar recursos de AWS, como instancias de Amazon Elastic Compute Cloud (Amazon EC2), en la VPC. Puede configurar la VPC modificando su intervalo de direcciones IP y crear subredes. Además, puede configurar tablas de enrutamiento, gateways de red y ajustes de seguridad.

En la consola de administración de AWS, en el menú **Services** (Servicios), elija **VPC**.

La consola de VPC ofrece un asistente que puede crear de manera automática varias arquitecturas de VPC. Sin embargo, en este laboratorio, creará los componentes de la VPC de forma manual.

En el panel de navegación izquierdo, elija Your VPCs (Sus VPC).

Se proporciona una VPC predeterminada para que pueda lanzar recursos en cuanto comience a utilizar AWS. También hay una VPC compartida que utilizará más tarde en el laboratorio. Sin embargo, ahora creará su propia Lab VPC.

La VPC tendrá el intervalo de direccionamiento entre dominios sin clases (CIDR) 10.0.0.0/16, que incluye todas las direcciones IP que comienzan con 10.0.x.x. Contiene más de 65 000 direcciones. Posteriormente, repartirá las direcciones entre distintas subredes.

Elija **Create VPC** (Crear VPC) y configure los siguientes ajustes:

    Name tag (Etiqueta de nombre): Lab VPC
    IPv4 CIDR block (Bloque de CIDR IPv4): 10.0.0.0/16
    Elija Create (Crear) y, a continuación, Close (Cerrar).

    Si estas opciones no aparecen, cancele la configuración. En el panel de navegación a la izquierda, asegúrese de que eligió Your VPCs (Sus VPC). A continuación, vuelva a seleccionar Create VPC (Crear VPC).

Seleccione Lab VPC y asegúrese de que sea la única VPC seleccionada.

En la mitad inferior de la página, elija la pestaña **Tags** (Etiquetas).

Las etiquetas son útiles para identificar recursos. Por ejemplo, puede utilizar una etiqueta para identificar centros de costos o entornos diferentes (como desarrollo, prueba o producción).

Elija **Actions** (Acciones) y seleccione **Edit DNS hostnames **(Editar nombres de alojamiento de DNS).

Esta opción asigna un nombre del sistema de nombres de dominio (DNS) fácil de recordar a las instancias EC2 en la VPC, como, por ejemplo:

ec2-52-42-133-255.us-west-2.compute.amazonaws.com

Seleccione **enable** (habilitar) y elija **Save** (Guardar), además de **Close** (Cerrar).

Todas las instancias EC2 que se lancen en la VPC ahora recibirán de manera automática un nombre de alojamiento de DNS. También puede agregar un nombre de DNS más significativo (como aplicación.ejemplo.com) más adelante utilizando Amazon Route 53.

## Crear subredes

### Crear una subred pública

La subred pública se utilizará para los recursos expuestos a Internet.

En el panel de navegación izquierdo, elija **Subnets** (Subredes).

Elija **Create subnet** (Crear subred) y configure los siguientes ajustes:

    Name tag (Etiqueta de nombre): Public Subnet (Subred pública)
    VPC: Lab VPC
    Availability Zone (Zona de disponibilidad): seleccione la primera zona de disponibilidad de la lista (No elija No Preference [Sin preferencia]).
    IPv4 CIDR block (Bloque de CIDR IPv4): 10.0.0.0/24
    Elija Create (Crear) y, a continuación, Close (Cerrar).

    La VPC tiene un bloque de CIDR 10.0.0.0/16, que incluye todas las direcciones IP 10.0.x.x. La subred que acaba de crear tiene un bloque de CIDR 10.0.0.0/24, que incluye todas las direcciones IP 10.0.0.x. Tienen aspecto similar, pero la subred es más pequeña que la VPC debido al /24 en el intervalo del CIDR.

Ahora configurará la subred de manera que se asigne automáticamente una dirección IP pública a todas las instancias lanzadas dentro de ella.

Seleccione **Public Subnet** (Subred pública).

Elija **Actions** (Acciones)  y seleccione **Modify+auto-assign IP settings** (Modificar configuración de asignación automática de IP). Luego, haga lo siguiente:

    Seleccione Auto-assign IPv4 (Asignar automáticamente IPv4).
    Elija Save (Guardar).

    Aunque esta subred se denomina Public Subnet (Subred pública), todavía no es pública. Una subred pública debe tener una gateway de Internet, la cual asociará en la siguiente tarea.


### Crear una subred privada

La subred privada se utilizará para los recursos que deban permanecer aislados de Internet.

Utilice lo que acaba de aprender para crear otra subred con los siguientes ajustes:

    Name tag (Etiqueta de nombre): Private Subnet (Subred privada)
    VPC: Lab VPC
    Availability Zone (Zona de disponibilidad): seleccione la primera zona de disponibilidad de la lista (No elija No Preference [Sin preferencia]).
    IPv4 CIDR block (Bloque de CIDR IPv4): 10.0.2.0/23

El bloque de CIDR 10.0.2.0/23 incluye todas las direcciones IP que comienzan con 10.0.2.x y 10.0.3.x. Esto representa un tamaño el doble de grande en comparación con el de la subred pública, ya que la mayoría de los recursos deben mantenerse privados, a menos que deban ser accesibles en particular desde Internet.

Su VPC ahora tiene dos subredes. Sin embargo, la subred pública está totalmente aislada y no puede comunicarse con recursos fuera de la VPC. A continuación, configurará la subred pública de manera que se conecte a Internet a través de una gateway de Internet.

## Crear una gateway de Internet

Una gateway de Internet es un componente de VPC al cual se aplica escalado horizontal, es redundante y tiene alta disponibilidad. Además, permite la comunicación entre las instancias en la VPC y el Internet. No implica riesgos de disponibilidad ni restricciones de ancho de banda para el tráfico de red.

Una gateway de Internet cumple dos propósitos:

proporcionar un objetivo en las tablas de enrutamiento que se conecte a Internet
realizar la conversión de direcciones de red (NAT) para las instancias a las que se les asignaron direcciones IPv4 públicas

En esta tarea, creará una gateway de Internet para que el tráfico de Internet pueda obtener acceso a la subred pública.

En el panel de navegación izquierdo, elija **Internet Gateways** (Gateways de Internet).

Elija **Create internet gateway** (Crear gateway de Internet) y configure los siguientes ajustes:

    Name tag (Etiqueta de nombre): Lab IGW (Gateway de Internet de laboratorio)
    Elija Create (Crear) y, a continuación, Close (Cerrar).

Ahora puede asociar la gateway de Internet a su Lab VPC.

Seleccione  **Lab IGW** (Gateway de Internet de laboratorio) y asegúrese de que sea la única gateway seleccionada.

Elija **Actions** (Acciones) y, a continuación, **Attach to VPC** (Asociar a VPC) para luego configurar los siguientes ajustes:

    VPC: en la lista, seleccione Lab VPC.
    Elija Attach (Asociar).

Esta acción asociará la gateway de Internet a su Lab VPC. Aunque haya creado una gateway de Internet y la haya asociado a su VPC, también debe configurar la tabla de enrutamiento de la subred pública para que utilice la gateway de Internet.

## Configurar tablas de enrutamiento

Una tabla de enrutamiento contiene un conjunto de reglas, denominadas rutas, que se utilizan para determinar hacia dónde se dirige el tráfico de red. Cada subred de una VPC debe estar asociada a una tabla de enrutamiento porque la tabla controla el direccionamiento de la subred. Una subred solo se puede asociar a una tabla de enrutamiento a la vez, pero puede asociar varias subredes a la misma tabla de enrutamiento.

Para utilizar una gateway de Internet, la tabla de enrutamiento de una subred debe contener una ruta que dirija el tráfico con destino a Internet a la gateway de Internet. Si una subred está asociada a una tabla de enrutamiento que tiene una ruta hacia una gateway de Internet, se conoce como subred pública.

En esta tarea, realizará lo siguiente:

crear una tabla de enrutamiento pública para el tráfico con destino a Internet
agregar una ruta a la tabla de enrutamiento para dirigir el tráfico con destino a Internet hacia la gateway de Internet
asociar la subred pública a la nueva tabla de enrutamiento

En el panel de navegación izquierdo, elija **Route Tables** (Tablas de enrutamiento).

Se muestran varias tablas de enrutamiento, pero solo hay una tabla de enrutamiento asociada a Lab VPC. Esta tabla de enrutamiento dirige el tráfico a destinos locales, por lo que se denomina tabla de enrutamiento privada.

En la columna VPC ID (ID de VPC), seleccione  la tabla de enrutamiento que muestra Lab VPC (puede expandir la columna para ver los nombres).

En la columna **Name** (Nombre), elija , escriba el nombre **Private Route Table** (Tabla de enrutamiento privada) y elija .

En la mitad inferior de la página, elija la pestaña **Routes** (Rutas).

Solo existe una ruta y esta muestra que todo el tráfico destinado a 10.0.0.0/16 (que es el intervalo de la Lab VPC) se dirigirá a destinos locales. Esta ruta permite que todas las subredes de una VPC se comuniquen entre sí.

Ahora, creará una nueva tabla de enrutamiento pública para enviar el tráfico público a la gateway de Internet.

Elija **Create route table** (Crear tabla de enrutamiento) y defina los siguientes ajustes:

    Name tag (Etiqueta de nombre): Public Route Table (Tabla de enrutamiento pública)
    VPC: Lab VPC
    Elija Create (Crear) y, a continuación, Close (Cerrar).

Seleccione  **Public Route Table** (Tabla de enrutamiento pública) y asegúrese de que sea la única tabla de enrutamiento seleccionada.

En la pestaña **Routes** (Rutas), elija **Edit routes** (Editar rutas).

Ahora, agregará una ruta para dirigir el tráfico con destino a Internet (0.0.0.0/0) hacia la gateway de Internet.

Elija **Add rule** (Agregar ruta) y, luego, configure los siguientes ajustes:

    Destination (Destino): 0.0.0.0/0
    Target (Objetivo): seleccione Internet Gateway (Gateway de Internet) y, a continuación, seleccione en la lista Lab IGW (Gateway de Internet de laboratorio).
    Elija Save routes (Guardar rutas) y, a continuación, Close (Cerrar).

El último paso consiste en asociar esta nueva tabla de enrutamiento a la subred pública.

Elija la pestaña **Subnet Associations** (Asociaciones de subredes).

Elija **Edit subnet associations** (Editar asociaciones de subredes).

Seleccione  la fila de **Public Subnet** (Subred pública).

Elija **Save** (Guardar).

La subred pública ahora es pública porque tiene una entrada de tabla de enrutamiento que envía tráfico a Internet a través de la gateway de Internet.

Para resumir, puede crear una subred pública siguiendo los pasos que se indican a continuación:

    crear un gateway de Internet
    crear una tabla de enrutamiento
    agregar una ruta a la tabla de enrutamiento que dirija el tráfico de 0.0.0.0/0 a la gateway de Internet
    asociar la tabla de enrutamiento a una subred que, en consecuencia, se convierte en una subred pública


## Crear un grupo de seguridad para el servidor de aplicaciones

Un grupo de seguridad actúa como un firewall virtual para que las instancias controlen el tráfico entrante y saliente. Los grupos de seguridad operan en el nivel de la interfaz de red elástica de la instancia, pero no en el nivel de la subred. Por lo tanto, cada instancia puede tener su propio firewall que controle el tráfico. Si no especifica un grupo de seguridad determinado en el momento del lanzamiento, la instancia se asigna automáticamente al grupo de seguridad predeterminado de la VPC.

En esta tarea, creará un grupo de seguridad que permita a los usuarios acceder al servidor de aplicaciones a través de HTTP.

En el panel de navegación izquierdo, elija **Security Groups** (Grupos de seguridad).

Elija **Create security group** (Crear grupo de seguridad) y configure los siguientes ajustes:

    Security group name (Nombre del grupo de seguridad): App-SG (Grupo de seguridad de aplicación)
    Description (Descripción): Allow HTTP traffic (Permitir tráfico HTTP)
    VPC: Lab VPC
    Elija Create (Crear) y, a continuación, Close (Cerrar).

Seleccione **App-SG** (Grupo de seguridad de aplicación).

Elija la pestaña **Inbound Rules** (Reglas de entrada).

La configuración de **Inbound Rules** (Reglas de entrada) determina qué tráfico puede llegar a la instancia. Lo configurará de manera que se permita el tráfico HTTP (puerto 80) proveniente de cualquier lugar de Internet (0.0.0.0/0).

Elija **Edit rules** (Editar reglas).

Elija **Add Rule** (Agregar regla) y, luego, configure los siguientes ajustes:

    Type (Tipo): HTTP
    Source (Origen): Anywhere (Cualquiera)
    Description (Descripción): Allow web access (Permitir acceso web)
    Elija Save rules (Guardar reglas) y, a continuación, Close (Cerrar).

Utilizará **App-SG** (Grupo de seguridad de aplicación) en la siguiente tarea.

## Lanzar un servidor de aplicaciones en la subred pública

Para probar que la VPC está configurada correctamente, lanzará ahora una instancia EC2 en la subred pública. Además, confirmará que puede acceder a la instancia EC2 desde Internet.

En el menú **Services** (Servicios), elija EC2.

Elija **Launch Instance** (Lanzar instancia) y seleccione **Launch Instance** (Lanzar instancia) en la lista desplegable. Configure las siguientes opciones:

    Paso 1 (elegir AMI):
        AMI: Amazon Linux 2

    Paso 2 (elegir el tipo de instancia):
        Instance type (Tipo de instancia): t2.micro

    Paso 3 (configurar los detalles de la instancia):
        Network (Red): Lab VPC
        Subnet (Subred): Public Subnet (Subred pública)
        IAM role (Rol de IAM): Inventory-App-Role
        User Data (Datos de usuario) (en  Advanced Details [Detalles avanzados]):


```bash
#!/bin/bash

# Install Apache Web Server and PHP

yum install -y httpd mysql

amazon-linux-extras install -y php7.2

# Download Lab files

wget https://aws-tc-largeobjects.s3-us-west-2.amazonaws.com/ILT-TF-200-ACACAD-20-EN/mod6-guided/scripts/inventory-app.zip

unzip inventory-app.zip -d /var/www/html/

# Download and install the AWS SDK for PHP

wget https://github.com/aws/aws-sdk-php/releases/download/3.62.3/aws.zip

unzip aws -d /var/www/html

# Turn on web server

chkconfig httpd on

service httpd start
```

    Paso 4 (agregar almacenamiento):
        Utilice la configuración predeterminada (sin cambios).

    Paso 5 (agregar etiquetas):
        Elija Add Tag (Agregar etiqueta).
        Key (Clave): Name (Nombre)
        Value (Valor): App Server (Servidor de aplicaciones)

    Paso 6 (configurar grupo de seguridad):
        Select an existing security group (Seleccionar un grupo de seguridad existente): App-SG (Grupo de seguridad de aplicación)

        Recibirá esta advertencia: You will not be able to connect to the instance (No podrá conectarse a la instancia). Esta advertencia es aceptable porque no se conectará a la instancia. Toda la configuración se realiza a través del script de datos de usuario.
        Haga clic en Continue (Continuar).

    Paso 7 (revisar):
        Lanzar

En la ventana Select an existing key pair or create a new key pair (Seleccionar un par de claves existente o crear un nuevo par de claves), realice lo siguiente:

Seleccione Proceed without a key pair (Continuar sin un par de claves).
Seleccione  I acknowledge that… (Confirmo que…).
Elija Launch Instances (Lanzar instancias).

Una página de estado le notificará que se están lanzando las instancias.

Elija View Instances (Ver instancias).
Espere a que el servidor de aplicaciones se lance completamente. Debe mostrar el siguiente estado:

Instance State (Estado de instancia):  running (en ejecución)

Puede elegir actualizar  de manera ocasional para ver la pantalla más actualizada.

Seleccione  App Server (Servidor de aplicaciones).
Copie la dirección IP pública IPv4 de la pestaña Description (Descripción).
Abra una nueva pestaña del navegador web con esa dirección IP.

# Automatización de servicios

## Implementar una capa de red

Se trata de una recomendación de las prácticas recomendadas para implementar una infraestructura en capas. Las capas comunes son las siguientes:

Red (Amazon VPC)
Base de datos
Aplicación

De esta forma, las plantillas se pueden volver a utilizar entre sistemas. Por ejemplo, puede implementar una topología de red común entre entornos de desarrollo, pruebas y producción, o implementar una base de datos estándar para varias aplicaciones.

En esta tarea, implementará una plantilla de AWS **CloudFormation** que crea una capa de redes a través de Amazon VPC.

Haga clic derecho en el siguiente enlace y descargue la plantilla lab-network.yaml en su equipo.

Si desea ver cómo se definen los recursos de AWS, puede abrir la plantilla en un editor de texto.

Las plantillas se pueden escribir en el lenguaje de notación de objetos JavaScript (JSON) o YAML Ain't Markup Language (YAML). YAML es un lenguaje de marcado similar a JSON, pero es más fácil de leer y editar.

En la consola de administración de AWS, encontrará el menú Services (Servicios), donde debe seleccionar CloudFormation.

Si ve este mensaje, haga clic en Try it out now and provide us feedback (Pruébelo ahora y envíenos sus comentarios):

**Nueva consola**

Elija Create stack (crear pila) y configure estos ajustes.

**Paso 1: Especificar la plantilla**

    Template source (Origen de la plantilla): Upload a template file (Cargar un archivo de plantilla)
    Upload a template file (Cargar un archivo de plantilla): haga clic en Choose file (Elegir archivo) y seleccione el archivo lab-network.yaml que descargó.
    Elija Next (Siguiente).

**Paso 2: Crear pila**
    Stack name (Nombre de la pila): lab-network
    Elija Next (Siguiente).

**Paso 3: Configurar las opciones de la pila**

    En la sección Tags (Etiquetas), escriba estos valores.
        Key (Clave): application
        Value (Valor): inventory

    Elija Next (Siguiente).

**Paso 4: Revisar lab-network**

    Elija Create stack (crear pila).

AWS CloudFormation ahora utilizará la plantilla para generar un pila de recursos en la cuenta de AWS.

Las etiquetas especificadas se propagan automáticamente a los recursos que se crean, lo que facilita la identificación de los recursos utilizados por las aplicaciones particulares.

Haga clic en la pestaña Stack info (Información de la pila).

Espere a que **Status** (estado) cambie a **CREATE_COMPLETE.**

Si es necesario, haga clic en Refresh (Actualizar) cada 15 segundos para actualizar la pantalla.

A partir de ahora, puede examinar los recursos que se crearon.

Elija la pestaña Resources (Recursos).

Verá una lista de los recursos creados por la plantilla.

Si la lista está vacía, actualice la lista seleccionando Refresh (Actualizar).

Haga clic en la pestaña **Events** (Eventos) y desplácese a través del registro de eventos.

El registro de eventos muestra (desde los eventos más recientes a los menos recientes) las actividades realizadas por AWS CloudFormation. Algunos ejemplos incluyen empezar a crear un recurso y, luego, completar la creación. Todo error que se produzca durante la creación de la pila se mostrará en esta pestaña.

Elija la pestaña **Outputs** (Resultados).

Una pila de CloudFormation puede proporcionar información de los resultados, como el ID de los recursos específicos y enlaces a los recursos.

Se enumeran dos resultados.

    PublicSubnet: El ID de la subred pública que se creó (por ejemplo: _subnet-08aafd57f745035f1__
    VPC: El ID de la VPC que se creó (por ejemplo: vpc-08e2b7d1272ee9fb4)

Los resultados también se pueden utilizar para proporcionar valores a otras pilas. Esto se muestra en la columna Export name (Nombre de exportación). En este caso, la VPC y los ID de subred reciben nombres de exportación para que otras pilas puedan recuperar los valores. Estas otras pilas pueden crear recursos dentro de la VPC y la subred que se acaban de crear. Utilizará estos valores en la siguiente tarea.

Elija la pestaña **Template** (Plantilla).

Esta pestaña muestra la plantilla que se utilizó para crear la pila, es decir, la plantilla que cargó mientras creaba la pila. Examine la plantilla y vea los recursos que se crearon. También explore la sección Outputs (Resultados) al final (esta sección define qué valores exportar).

## Implementar una capa de aplicación

Ahora que ha implementado la capa de red, ha llegado el momento de implementar una capa de aplicación que contenga una instancia de Amazon Elastic Compute Cloud (Amazon EC2) y un grupo de seguridad.

La plantilla de AWS CloudFormation importará la VPC y los ID de subred desde la sección Outputs (Resultados) de la pila de CloudFormation existente. A continuación, utilizará esta información para crear el grupo de seguridad en la VPC y la instancia EC2 en la subred.

Haga clic derecho en el siguiente enlace y descargue la plantilla en su equipo: lab-application.yaml

Si desea ver cómo se definen los recursos, puede abrir la plantilla en un editor de texto.

En el panel de navegación izquierdo, elija Stacks (Pilas).

Seleccione Create stack > With new resources (standard) (Crear pila > Con nuevos recursos [estándar]), y, a continuación, configure estos ajustes.

**Paso 1:** **Especificar la plantilla**

    Template source (Origen de la plantilla): Upload a template file (Cargar un archivo de plantilla)
    Upload a template file (Cargar un archivo de plantilla): Haga clic en Choose file (Seleccionar archivo) y seleccione el archivo lab-application.yaml que descargó.
    Elija Next (Siguiente)

**Paso 2: Crear pila**

    Stack name (Nombre de la pila): lab-application
    NetworkStackName (Nombre de la pila de red): lab-network
    Seleccione Next (Siguiente)

El parámetro **Network Stack Name** (Nombre de la pila de red) indica a la plantilla el nombre de la pila que creó primero (lab-network) para que esta pueda recuperar los valores de la sección Outputs (Resultados).

**Paso 3:** **Configurar las opciones de la pila**

    En la sección Tags (Etiquetas), escriba estos valores.
    Key (clave): application
    Value (valor): inventory
    Seleccione Next (Siguiente)

**Paso 4: Revisar la aplicación de laboratorio**

    Haga clic en Create stack (Crear pila)

Mientras se está creando la pila, examine los detalles en la pestaña Events (Eventos) y en la pestaña Resources (Recursos). Puede monitorear el progreso del proceso de creación de recursos y el estado del recurso.

En la pestaña Stack info (Información de la pila), espere a que Status (Estado) cambie a *CREATE_COMPLETE.*

La aplicación ya está lista.

Elija la pestaña **Outputs** (Resultados).

Copie la URL que se muestra y, a continuación, abra una nueva pestaña del navegador web, pegue la URL y presione INTRO.

La pestaña del navegador abrirá la aplicación, que se está ejecutando en el servidor web que creó esta nueva pila de **CloudFormation**.

Una pila de CloudFormation puede utilizar valores de referencia de otra pila de CloudFormation. Por ejemplo, esta parte de la plantilla lab-application hace referencia a la plantilla lab-network:

```bash
    WebServerSecurityGroup:

    Type: AWS::EC2::SecurityGroup

    Properties:

        GroupDescription: Enable HTTP ingress

        VpcId:

        Fn::ImportValue:

            !Sub ${NetworkStackName}-VPCID
```

La última línea utiliza el nombre de pila de red que proporcionó (lab-network) cuando se creó la pila. Importa el valor de lab-network-VPCID desde Outputs (Resultados) de la primera pila. A continuación, inserta el valor en el campo del ID de VPC de la definición del grupo de seguridad. El resultado es que el grupo de seguridad se crea en la VPC creada por la primera pila.

Este es otro ejemplo, que está en la plantilla CloudFormation que acaba de utilizar para crear la pila de aplicaciones. Este código de plantilla coloca la instancia EC2 en la subred creada por la pila de red:
```bash
    SubnetId:

    Fn::ImportValue:

    !Sub ${NetworkStackName}-SubnetID
```

Toma el ID de subred de la pila lab-network (red del laboratorio) y lo utiliza en la pila lab-application (aplicación del laboratorio) para lanzar la instancia en la subred pública que creó la primera pila.

## Actualizar una pila

AWS CloudFormation también puede actualizar una pila que se ha implementado. Cuando actualiza una pila, AWS CloudFormation solo modificará o reemplazará los recursos que se están modificando. Cualquier recurso que no modifique quedará como está.

En esta tarea, actualizará la pila lab-application (aplicación del laboratorio) para modificar un parámetro en el grupo de seguridad.

En primer lugar, deberá examinar la configuración actual en el grupo de seguridad.

En la consola de administración de AWS, encontrará el menú **Services** (Servicios), donde debe elegir **EC2**.

En el panel de navegación izquierdo, elija Security Groups (Grupos de seguridad).

Elija la casilla de **verificación** de lab-application-WebServerSecurityGroup....

Elija la pestaña **Inbound rules** (Reglas de entrada).

El grupo de seguridad actualmente solo tiene una regla. La regla permite el tráfico HTTP.

Ahora volverá a AWS **CloudFormation** para actualizar la pila.

En el menú **Services** (Servicios), elija CloudFormation.

Haga clic derecho en este enlace y descargue la plantilla actualizada lab-application2.yaml en su equipo.

Esta plantilla tiene una configuración adicional para permitir el tráfico entrante de Secure Shell (SSH) en el puerto 22:

    - IpProtocol: tcp

    FromPort: 22

    ToPort: 22

    CidrIp: 0.0.0.0/0

En la lista Stacks (Pilas) de la consola de AWS CloudFormation, elija lab-application.

Elija **Update** (Actualizar) y configure estos ajustes.

    Elija Replace current template (Reemplazar plantilla actual).
    Template source (Origen de la plantilla): Upload a template file (Cargar un archivo de plantilla)
    Upload a template file (Cargar un archivo de plantilla): Haga clic en Choose file (Elegir archivo) y seleccione el archivo lab-application2.yaml que descargó.

Elija **Next** (Siguiente) en cada una de las tres pantallas siguientes para avanzar a la página Review lab-application (Revisar lab-application).

En la sección Change set preview (Vista previa del conjunto de cambios) de la parte inferior de la página, AWS CloudFormation muestra los recursos que se actualizarán

Vista previa del conjunto de cambios

Esta vista previa del conjunto de cambios indica que AWS CloudFormation modificará WebServerSecurityGroup sin necesidad de reemplazarlo (Replacement = False). Este conjunto de cambios significa que al grupo de seguridad se le aplicará un cambio menor y que no será necesario cambiar ninguna referencia al grupo de seguridad.

Elija **Update stack** (Actualizar pila).

En la pestaña Stack info (Información de la pila), espere a que Status (Estado) cambie a UPDATE_COMPLETE.

Si es necesario, seleccione **Refresh** (Actualizar) cada 15 segundos para actualizar el estado.

A partir de ahora, puede **verificar el cambio**.

Regrese a la consola de Amazon EC2 y, en el panel de navegación izquierdo, seleccione Security Groups (Grupos de seguridad).

En la lista **Security Groups** (Grupos de seguridad), seleccione lab-application-WebServerSecurityGroup.

La pestaña **Inbound rules** (Reglas de entrada) debe mostrar una regla adicional que permita el tráfico SSH a través del puerto TCP 22.

Esta subtarea demuestra cómo se pueden implementar los cambios en un proceso repetible y documentado. Las plantillas de AWS CloudFormation se pueden almacenar en un repositorio de código fuente (como AWS CodeCommit). De esta manera, puede mantener versiones y un historial de las plantillas y la infraestructura que se implementó.

## Explorar las plantillas con AWS CloudFormation Designer

AWS CloudFormation Designer es una herramienta gráfica para crear, ver y modificar plantillas de AWS CloudFormation. Con Designer, puede diagramar los recursos de la plantilla mediante una interfaz que permite arrastrar y soltar y, a continuación, editar su información a través del editor JSON y YAML integrado.

Independientemente de si es un usuario principiante o avanzado de AWS CloudFormation, Designer puede ayudarlo a ver rápidamente la interrelación entre los recursos de una plantilla. También le permite modificar plantillas fácilmente.

En esta tarea, obtendrá experiencia práctica con **Designer**.

En el menú **Services** (Servicios), elija **CloudFormation**.

En el panel de navegación izquierdo, elija **Designer**.

Sugerencia: Es posible que necesite expandir el panel de navegación con un clic en el ícono de menú.

Seleccione el menú **File** (Archivos), seleccione **Open > Local file** (Abrir > Archivo local) y seleccione la plantilla lab-application2.yaml que descargó previamente.

Designer mostrará una representación gráfica de la plantilla:

**CloudFormation Designer**

En lugar de diagramar una arquitectura típica, Designer es un editor visual para plantillas de AWS CloudFormation, por lo que diseña los recursos definidos en una plantilla y sus relaciones entre sí.

Experimente con las características de Designer. Algunas de las cosas que puede intentar son estas:

    Haga clic en los recursos en pantalla. El panel inferior mostrará, a continuación, la parte de la plantilla que define los recursos.
    Intente arrastrar un nuevo recurso, desde el panel Resource types (Tipos de recursos) de la izquierda, hasta el área de diseño. La definición del recurso se insertará automáticamente en la plantilla.
    Pruebe arrastrar los círculos conectores de recursos para crear relaciones entre los recursos.
    Abra la plantilla lab-network.yaml que descargó anteriormente en el laboratorio y explore sus recursos en Designer.

## Eliminar la pila

Cuando los recursos ya no son necesarios, AWS CloudFormation puede eliminar aquellos creados para la pila.

También se puede especificar una política de eliminación (deletion policy) en relación con los recursos. Puede conservar o (en algunos casos) hacer una copia de seguridad de un recurso cuando se elimina su pila. Esta característica es útil para conservar bases de datos, volúmenes de disco o cualquier recurso que pueda ser necesario después de eliminar la pila.

La pila lab-application se configuró para tomar una instantánea de un volumen de disco de Amazon Elastic Block Store (Amazon EBS) antes de que se elimine. El código de la plantilla que hace posible esta configuración es el siguiente:

```bash
DiskVolume:

Type: AWS::EC2::Volume

Properties:

    Size: 100

    AvailabilityZone: !GetAtt WebServerInstance.AvailabilityZone

    Tags:

    - Key: Name

        Value: Web Data

DeletionPolicy: Snapshot
```

El campo **DeletionPolicy** (Política de eliminación) en la línea final dirige a AWS CloudFormation a crear una instantánea del volumen de disco antes de que se elimine.

Ahora, **eliminará** la pila lab-application y veremos los resultados de esta política de eliminación.

Vuelva a la consola principal de AWS CloudFormation seleccionando el enlace **Close** (Cerrar) en la parte superior de la página de Designer (seleccione **Leave page** [Abandonar la página] si se le solicita).

**En la lista de pilas, elija el enlace lab-application.**

Seleccione **Delete** (Eliminar).

Seleccione **Delete stack** (Eliminar pila).

Puede monitorear el proceso de eliminación en la pestaña Events (Eventos) y actualizar la pantalla seleccionando Refresh (Actualizar) ocasionalmente. También puede ver una entrada del registro de eventos que indica que se está creando la instantánea de EBS.

Espere a que se elimine la pila. Desaparecerá de la lista de pilas.

Se eliminó la pila de aplicaciones __, pero la pila de red permaneció intacta. Este caso refuerza la idea de que diferentes equipos (por ejemplo, el equipo de red o el equipo de aplicaciones) podrían administrar sus propias pilas.

Ahora comprobará que se creó una instantánea del volumen de EBS antes de que se eliminara el volumen de EBS.

En el menú **Services** (Servicios), seleccione **EC2**.

En el panel de navegación izquierdo, seleccione **Snapshots** (Instantáneas).

Debería ver una instantánea con un tiempo de inicio en los últimos minutos.