# Ejercicio 2

Se desea modelar la información necesaria para una red social. La red social permite a los usuarios compartir imágenes de diferentes temáticas y realizar comentarios sobre las mismas. De los usuarios se registran: nombre y apellido, usuario en la red social (que es único), clave de acceso y cuentas de correo electrónico, dirección detallada y un teléfono de contacto. 
En la red social, los usuarios pueden subir imágenes, o bien comentar, descargar o compartir una publicación (imagen/es) de otros usuarios vinculados. 
Cuando dos usuarios se vinculan, se debe almacenar información de este vínculo, fecha y hora, tipo de vínculo y opcionalmente una descripción estandarizada en la red social que indica de donde se conocen (lugar de vínculo). En la misma fecha y hora el mismo usuario no podrá generar dos vínculos. 
De las imágenes se debe almacenar: fecha y hora de publicación, temática de la imagen, título, usuario que realiza el posteo y el nombre del archivo correspondiente, además de un conjunto de palabras claves que caracterizan la imagen. Un usuario no puede subir dos imágenes con título idéntico.De las temáticas se registra un nombre único y una descripción asociada. 
De las imágenes se debe poder determinar los usuarios que compartieron la imagen o la descargaron, indicando fecha y hora y si la compartió o descargó. 
Así mismo, los usuarios pueden realizar comentarios en la red social, se debe almacenar un texto, la fecha y hora de creación del comentario, que imagen comentó o a que comentario respondió. Un comentario podría ser respuesta a otro comentario existente. Un usuario no podrá realizar dos comentarios en la misma fecha y hora. 

## Modelo conceptual
![](https://i.imgur.com/RZoudAY.png)

## Modelo lógico
<!-- ![](https://i.imgur.com/dcFKcpd.png) -->
