# Ejercicio 7:

**Banda(codigoB, nombreBanda, genero_musical, año_creacion)**

**Integrante (DNI, nombre, apellido, dirección, email, fecha_nacimiento, codigoB(fk))**

**Escenario(nroEscenario, nombre _ escenario, ubicación, cubierto, m2, descripción)**

**Recital(fecha, hora, nroEscenario, codigoB (fk))**

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
  
```sql
SELECT DISTINCT (B.nombreBanda, R.fecha, R.hora, E.nombre_escenario, E.ubicacion)
FROM Banda B INNER JOIN Recital R ON (B.codigoB = R.codigoB) 
             INNER JOIN Escenario E ON (R.nroEscenario = E.nroEscenario)
WHERE (R.fecha = '2018/12/04');
```

4. Listar DNI, nombre, apellido, email de integrantes que hayan tocado en el escenario con
nombre ‘Gustavo Cerati’ y en el escenario con nombre ‘Carlos Gardel’.

```sql  
SELECT DISTINCT (I.DNI, I.nombre, I.apellido, I.email)
FROM Integrante I INNER JOIN Recital R ON (I.codigoB = R.codigoB)
                  INNER JOIN Escenario E ON (R.nroEscenario = E.nroEscenario)
WHERE (E.nombre_escenario = 'Gustavo Cerati') 
INTERSECT (
    SELECT DISTINCT (I.DNI)DISTINCT (I.DNI, I.nombre, I.apellido, I.email)
    FROM Integrante I INNER JOIN Recital R ON (I.codigoB = R.codigoB)
                      INNER JOIN Escenario E ON (R.nroEscenario = E.nroEscenario)
    WHERE (E.nombre_escenario = 'Carlos Gardel')
);
```

5. Reportar nombre, género musical y año de creación de bandas que tengan más de 8
integrantes.

```sql
SELECT DISTINCT (B.nombreBanda, B.genero_musical, B.año_creacion)
FROM Banda B INNER JOIN Integrante I ON (B.codigoB = I.codigoB)
GROUP BY B.codigoB, B.nombreBanda, B.genero_musical, B.año_creacion
HAVING COUNT(I.DNI) > 8;
```

6. Listar nombre de escenario, ubicación y descripción de escenarios que solo tuvieron
recitales con género musical rock and roll. Ordenar por nombre de escenario

```sql
SELECT DISTINCT (E.nombre_escenario, E.ubicacion, E.descripcion)
FROM Escenario E INNER JOIN Recital R ON (E.nroEscenario = R.nroEscenario)
                 INNER JOIN Banda B ON (R.codigoB = B.codigoB)
WHERE (B.genero_musical = 'rock and roll')
EXCEPT (
    SELECT DISTINCT (E.nombre_escenario, E.ubicacion, E.descripcion)
    FROM Escenario E INNER JOIN Recital R ON (E.nroEscenario = R.nroEscenario)
                     INNER JOIN Banda B ON (R.codigoB = B.codigoB)
    WHERE (B.genero_musical <> 'rock and roll')
)
ORDER BY E.nombre_escenario;
```

7. Listar nombre, género musical y año de creación de bandas que hayan realizado recitales
en escenarios cubiertos durante 2018.// cubierto es true, false según corresponda

```sql
SELECT DISTINCT (B.nombreBanda, B.genero_musical, B.año_creacion)
FROM Banda B INNER JOIN Recital R ON (B.codigoB = R.codigoB)
             INNER JOIN Escenario E ON (R.nroEscenario = E.nroEscenario)
WHERE (E.cubierto = true) AND (R.fecha BETWEEN '2018/01/01' AND '2018/12/31');
```

8. Reportar para cada escenario, nombre del escenario y cantidad de recitales durante 2018.

```sql
SELECT DISTINCT (E.nombre_escenario, COUNT(R.nroRecital))
FROM Escenario E INNER JOIN Recital R ON (E.nroEscenario = R.nroEscenario)
WHERE (R.fecha BETWEEN '2018/01/01' AND '2018/12/31')
GROUP BY E.nroEscenario E.nombre_escenario;
```

9. Modificar el nombre de la banda ‘Mempis la Blusera’ a: ‘Memphis la Blusera’.

```sql
UPDATE Banda
SET nombreBanda = 'Memphis la Blusera'
WHERE nombreBanda = 'Mempis la Blusera';
```
