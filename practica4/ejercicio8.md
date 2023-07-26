Ejercicio 8- Dadas las siguientes relaciones

**Equipo(codigoE, nombreE, descripcionE)** 

**Integrante (DNI, nombre, apellido, ciudad,email, telefono,codigoE(fk))** 

**Laguna(nroLaguna, nombreL, ubicación, extension, descripción)** 

**TorneoPesca(codTorneo, fecha, hora, nroLaguna(fk), descripcion)**

**Inscripcion(codTorneo, codigoE, asistio, gano) // asistio y gano son true o false según corresponda** 
corresponda

1. Listar DNI, nombre, apellido y email de integrantes que sean de la ciudad ‘La Plata’ y estén inscriptos en torneos a disputarse durante 2019.

```sql
SELECT 
```

2. Reportar nombre y descripción de equipos que solo se hayan inscripto en torneos de 2018.
3. Listar DNI, nombre, apellido,email y ciudad de integrantes que asistieron a torneos en la
laguna con nombre ‘La Salada, Coronel Granada’ y su equipo no tenga inscripciones a
torneos a disputarse en 2019.
4. Reportar nombre, y descripción de equipos que tengan al menos 5 integrantes.Ordenar por
nombre y descripción.
5. Reportar nombre, y descripción de equipos que tengan inscripciones en todas las lagunas.
6. Eliminar el equipo con código:10000.
7. Listar nombreL, ubicación,extensión y descripción de lagunas que no tuvieron torneos.
8. Reportar nombre, y descripción de equipos que tengan inscripciones a torneos a disputarse
durante 2019, pero no tienen inscripciones a torneos de 2018.
9. Listar DNI, nombre, apellido, ciudad y email de integrantes que ganaron algún torneo que se
disputó en la laguna con nombre: ‘Laguna de Chascomús’.
