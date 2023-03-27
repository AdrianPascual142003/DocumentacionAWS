## Categorias AWS

- Amazon EC2 --- Servicio, instancias, máquinas virtuales

- AWS Lambda --- sin servidor, basado en funciones, bajo coste

- ECS, EKS, WS Fargate, ECR --- Basado en contenedores y instancias

- AWS Ekastic Beanstalk --- PaaS, para aplicaciones web

## Amazon EC2

Elastic computer, proporciona máquinas vistuales. se pueden lanzar instancias de cualquier tamaño en todo el mundo y se puede controlar el tráfico desde y hacia las instancias

## AMI 

Son imagenes de máquinas amazon, estas contienen un sistema operativo.

Puedes crear una AMI  en cualquier momento y exportarla para transladarla.

### Tipo de instancia

Los recursos y la velocidad de red, depende del tipo seleccionado, es decir, cuanto mejor sea el tipo de instancia mejores recursos tendrá

### Configuración de Red

A esta se le asigna una dirección ip pública

### Asociar roles

A estas se les pueden asociar los roles que se deseen, si se asocian a la instancia EC2 se mantendrán en estos a la hora de exportar la AMI.

También se poueden asociar roles a una instancia que ya exista.

### Scripts

Puedes especificar un script de datos de usuarios durante el lanzamiento de la instancia, el script se ejecuta la primera vez que se inicia la instancia.

### Especificar el almacenamiento

Hay que configurar el volumen raíz, ademas se pueden añadir almacenamiento extra.

> Amazon Elastic Block Store (EBS)

Ofrece volumenes de nivel de Bloques persistentes, al detener la instancia no se pierden los datos.

> Almacén de instancias de Amazon EC2

Funcionan cuando la instancia esta activa, una vez detenida, se pierde la información

### Agregar etiquetas

Se utilizan para marcar algun recurso, se usa para asociar metadatos, hacen que todo sea más práctico de localizar

### Gurpos de seguridad

Son conjuntos de reglas de firewall que controlan el tráfico de una instancia

### Par de claves

Se utilizan para conectarse de forma segura a la instancia.

 > Windows

 Se utiliza para inciar en modo administrador, ya que esta será la contraseña

 > Linux

 Se utilizaran para hacer conexiones SSH de forma segura, permitiendole conectarse a esta

 ## Lanzar instancias EC2 con la línea de comandos

 Se puede utilizar para crear una instancia a través de la programación.

## Metadatos EC2

Son los datos que hay sobre una instancia.

Estos se pueden ver si accedemos con la ip pública de la máquina accediendo a 192.12.12.12 **/latest/meta-data**.

Estos se pueden usar para configurar o administrar una instancia en ejecución, asi como configurar script para que lean los metadatos.

## Docker y Kubernetes

Para hacer uso de Kubernetes tenemos Amazon EKS, este permite ejecutar Kubernetes en AWS. EKS admite contenedores tanto de Linux como Windows.

## AWS Labbda

Ejectua código sin servidor, basicamente es un servicio de informática sin servidor. Funciona de la siguiente manera: 

Carga el código y lo ejecuta

Tiene mucha compatibilidad con los diferentes códigos de programación

## EXAMEN

Las instancias cuando sean necesarias

Comprar instanacias reservadas

Todas

Instancias dedicadas

Lambda

Elastic Beanstalk

Ejecutar 4 reservadas y luego 8 demanda

Falso

Reservadas

Tipo de instancia e imagen de esta