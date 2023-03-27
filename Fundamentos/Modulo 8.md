# Amazon RDS

>Es el servicio de base de datos de AWS

## Desafios de las bases de datos relacionales

- Mantenimiento del servidor y huella energética
- Instalación y parches de software
- Copias de seguridad y alta disponibilidad
- Límites en la escalabilidad
- Seguridad de los datos
- Instalación y parches del SO


Las base de datos están siempre en la subred privada, mientras que la instancia de Amazon EC2 y la aplicación estna en la subred pública

## Réplicas de lectura RDS

Caracteristicas: 
- Ofrece replicación asíncrona
- Se puede promover a principal si es necesario

Funcionalidad: 
- Utilización para cargas de trabajo de base de datos de lectura inversa
- Reasignación de consultas de lectura

## Amazon DynamoDB

Es un servicio de datos NoSQL rápido  y flexible

Contiene:

- Tablas de bases de datos NoSQL
- Almacenamiento ilimitado
- Elementos con atributos diferentes
- Consultas  de baja latencia
- rendimiento escalable de lectura o escritura

## Amazon RedShift

Es una arquitectura de procesamiento en paralelo

## Amazon Aurora

- vBase de datos relacionales de clase empresarial
- Compatibilidad con MySQl o PostgreSQL
- Automatización de las tareas que requieren mucho tiempo, copias de seguridad...

## Examen

dynamo

scan

todas

redshift

elemento fundamental

dynamo

transacciones o consultas complejas

aurora

verdadero

todas