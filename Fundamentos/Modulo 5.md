## **Amazón VPC**

Permite controlar sus recursos de red virtual (ip, creación de subredes, tablas de enrutamiento). 
Permite personalizar las configuraciones de red

Están aisladas lógicamente y pertenecen a una región

Cuando creamos una la asignamos a un bloque CIDR IPv4, estas no permite cambiar el intervalo de direcciones después de crear la VPC, los bloques de CIDR de subredes no se pueden prosponer


## IPv4 Pública vs  Dirección IP elástica

IPv4 --- Se asignana a través de una dirección elástica, se asigna de manera automática a trevés de la confioguración.

Elástica --- Está asociada a cuenta AWS, se puede asignar y reasignar en cualquier momento, pueden haber costos adicionales.


### Elástica 

Si se vuelve a asociar a otra instancia sus atributos también se asocian
Cada instancia de su VPC tiene una interfaz de red predeterminada


## Tablas de enrutamiento y rutas 

Contiene reglas que puede configurar para dirigir el tráfico de red de su subred.
Cada ruta un destino y objetivo

## Información VPC

Estas se pueden conectar entre ellas, pero tiene restricciones:

- Espacios Ip no se pueden superponer
- No se admite interconexión transitiva
- Solo puede tener un recurso de interconexión enter las mismas dos VPC

## Creación Ip elásticas

Hacemos clic en crear y seleccionamos la región.

Luego para editar el VPC, tenemos que lanzar el VPC Wizard, y liego seleccionaremos la opción que queremos configura.

Aquí cambiaremos los parametros que nos hagan falta como el nombre de la subred privada, donde esta alojada la Ip Elastica (A través de la id de esta)

En las VPC siempre tendremos 2 subredes por defecto, una pública y otra privada

A estas les podemos aplicar reglas de tráfico

## Seguridad de las VPC

### Grupos de seguridad

A estos le podemos añadir grupos de seguridad con sus impertinentes reglas

Estos de formma predeterminada deniegan todo el tráfico de entrada y permiten todo el tráfico de salida

Estos también tienen estados

### ACL de Red

Tienen reglas de entrada y salida independientes, cada regla puede permitir o denegar tráfico

Permiten de forma predeterminada todo el tráfico IPv4 de entrada y salida

Estas no tienen estado

<br>

### **Tabla Comparativa**

<br>

![Comparativa](/imgs/TablaVPC.png)


## Creación de VPC y lanzamiento de un servidor web

Se pueden crear scripts como este

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

## Amazon Route 53

> Tipos de Direccionamiento:

 sencillo, turno rotativo, basado en latencia, geolocalización, geoproximidad, trans conmuitación de error, de respuestas con varios valores

 ## CloudFront

 Es un sericio de CDN rápido, mundial y seguro
 Red global de ubicaciones de borde y cachés de borde regionales
 Modelo de autoservicio
 Precios de pago por uso



 # EXAMEN UD 5

 Tamaño más pequeño para subred 28

Tamaño máximo del itnervalo de direcciones IP 16

Para baja latencia amazon CloudFront utiliza las ubicaciones de borde  de AWS

ACL de red es un control de seguridad opcional
