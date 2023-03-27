## Opciones de almacenamiento de AWS

>Almancenamiento en bloques

Se cambie el bloque (una parte del archivo)

>Almacenamiento de objetos

Es necesario actualizar todo el archivo

## Amazon EBS

Permite crear volúmenos de almacenamiento individuales y asociarlos a EC2

- Ofrece almacenamiento en bloques
- Se replican automáticamente los volumenes
- Se pueden realizar copias de seguridad automáticas en Amazon S3 (instantáneas)


### Volumenes

- Persisten independientemente de la instancia
- Los volumenes se cobran en función de la cantidad que se aprovisione por mes

### IOPS

> SSD de uso general

Se cobra en funcion de GB

> Magnético

Se cobra en función de la cantidad de solicitudes


## Práctica

Mostrar volumees

>df -h

Create an ext3 file system on the new volume:

> sudo mkfs -t ext3 /dev/sdf

Create a directory for mounting the new storage volume:

>sudo mkdir /mnt/data-store

Mount the new volume:

>sudo mount /dev/sdf /mnt/data-store

To configure the Linux instance to mount this volume whenever the instance is started, you will need to add a line to /etc/fstab.

>echo "/dev/sdf   /mnt/data-store ext3 defaults,noatime 1 2" | sudo tee -a /etc/fstab

## Amazon S3

Puedes crear un bucket en una region AWS

Cualquier cantidad de objetos se pueden cargar en un bucket. A este se accede a través de la URL con el nombre del bucket

## Amazon EFS

- Almacenamiento de archovs en la nube de AWS
- Buen funcionamiento de big data y análisis, flujos de trabajo de precesamiento multimedia
- Sistema de archivos de baja latencia a escala de petabytes
- Almacenamiento compartido
- Capacidad elástica
- Compatibilidad con las versiones 4.0 y 4.1
- Compabilidad con todas las AMI basadas en linux para amazon EC2

## Amazon S3 Glacier

- Servicio de almacenamiento para el archivo de datos a bajo costo y realización de copias de seguridad a largo plazo.
- Configuración del diclo de vida de archivo del contenido de Amazon S3 en Amazon S3 Glacier
- Opciones de recuperación:
    - Standard: de 3 a 5 horas
    - Bulk: de 5 a 12 horas
    - Expedited: de 1 a 5 minutos


### Uso

Servicios web RESTful, SDK Java o .NET, Amazon S3 con políticas de ciclo de vida

La políticas de ciclo de vida de Amazon S3 le permiten eliminar o mover objetos en función de su antigüedad

## Examen

Verdadero

varias zonas de disponibilidad

Todo el mundo entre todas las cuentas

Varias maquinas virtuales

Acceso rápido, solución de cifrado, glace

Falso

Contenedor

Verdadero

Se replican de forma automatica dentro de una zona de disponibilidad y se pueden cifrar en el momento de creacion. Se pueden utilizar como si no estuvieran cifrados