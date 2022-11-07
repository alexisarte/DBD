# Ejercicio 7:

**Banda(codigoB, nombreBanda, genero_musical, año_creacion)**

**Integrante (DNI, nombre, apellido,dirección,email, fecha_nacimiento,codigoB(fk))**

**Escenario(nroEscenario, nombre _ escenario, ubicación,cubierto, m2, descripción)**

**Recital(fecha,hora,nroEscenario, codigoB (fk))**

1. Listar DNI, nombre, apellido, dirección y email de integrantes nacidos entre 1980 y 1990 y
hayan realizado algún recital durante 2018.

```sql
SELECT DISTINCT (I.DNI, nombre, apellido, direccion, email)
FROM Integrante I INNER JOIN Recital R ON (I.codigoB = R.codigoB)
WHERE ((I.fecha_nacimiento BETWEEN '1980/01/01' AND '1990/12/31') AND (R.fecha BETWEEN '2018/01/01' AND '2018/12/31'));
```
2. Reportar nombre, género musical y año de creación de bandas que hayan realizado
recitales durante 2018, pero no hayan tocado durante 2017 .

```sql
SELECT DISTINCT (B.nombreBanda, B.genero_musical, B.año_creacion)
FROM Banda B INNER JOIN Recital R ON (B.codigoB = R.codigoB)
WHERE (R.fecha BETWEEN '2018/01/01' AND '2018/12/31')
EXCEPT (
    SELECT DISTINCT (B.nombreBanda, B.genero_musical, B.año_creacion)
    FROM Banda B INNER JOIN Recital R ON (B.codigoB = R.codigoB)
    WHERE (R.fecha BETWEEN '2017/01/01' AND '2017/12/31')
);
```
3. Listar el cronograma de recitales del dia 04/12/2018. Se deberá listar: nombre de la banda
que ejecutará el recital, fecha, hora, y el nombre y ubicación del escenario correspondiente.
4. Listar DNI, nombre, apellido,email de integrantes que hayan tocado en el escenario con
nombre ‘Gustavo Cerati’ y en el escenario con nombre ‘Carlos Gardel’.
5. Reportar nombre, género musical y año de creación de bandas que tengan más de 8
integrantes.
6. Listar nombre de escenario, ubicación y descripción de escenarios que solo tuvieron
recitales con género musical rock and roll. Ordenar por nombre de escenario
7. Listar nombre, género musical y año de creación de bandas que hayan realizado recitales
en escenarios cubiertos durante 2018.// cubierto es true, false según corresponda
8. Reportar para cada escenario, nombre del escenario y cantidad de recitales durante 2018.
9. Modificar el nombre de la banda ‘Mempis la Blusera’ a: ‘Memphis la Blusera’.
