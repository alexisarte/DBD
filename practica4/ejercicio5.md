# Ejercicio 5:

**Localidad(CodigoPostal, nombreL, descripcion, #habitantes)**

**Arbol(nroArbol, especie, años, calle, nro, codigoPostal(fk))**

**Podador(DNI, nombre, apellido, telefono, fnac, codigoPostalVive(fk))**

**Poda(codPoda, fecha, DNI(fk), nroArbol(fk))**

1. Listar especie, años, calle, nro. y localidad de árboles podados por el podador ‘Juan Perez’ y
   por el podador ‘Jose Garcia’.

```sql
SELECT especie, años, calle, nro, nombreL
FROM Arbol INNER JOIN Poda ON (Arbol.nroArbol = Poda.nroArbol)
           INNER JOIN Podador P ON (Poda.DNI = P.DNI)
           INNER JOIN Localidad ON (Arbol.codigoPostal = Localidad.codigoPostal)
WHERE (((P.nombre = 'Juan') AND (P.apellido = 'Perez')) OR ((P.nombre = 'Jose') AND (P.apellido = 'Garcia')));
```

2. Reportar DNI, nombre, apellido, fnac y localidad donde viven podadores que tengan podas
   durante 2018.

```sql
SELECT DISTINCT (Podador.DNI, nombre, apellido, fnac, nombreL)
FROM Podador INNER JOIN Poda ON (Podador.DNI = Poda.DNI)
             INNER JOIN Localidad ON (Podador.codigoPostalVive = Localidad.codigoPostal)
WHERE (Poda.fecha BETWEEN '2018/01/01' AND '2018/12/31');
```

3. Listar especie, años, calle, nro y localidad de árboles que no fueron podados nunca.

```sql
SELECT especie, años, calle, nro, nombreL
FROM Arbol INNER JOIN Localidad ON (Arbol.codigoPostal = Localidad.codigoPostal)
WHERE nroArbol NOT IN (SELECT Arbol.nroArbol
                       FROM Arbol INNER JOIN Poda ON (Arbol.nroArbol = Poda.nroArbol)
                                  INNER JOIN Localidad ON (Arbol.codigoPostal = Localidad.codigoPostal));
```

4. Reportar especie, años, calle, nro y localidad de árboles que fueron podados durante 2017 y
   no fueron podados durante 2018.

```sql
SELECT especie, años, calle, nro, nombreL
FROM Arbol INNER JOIN Poda ON (Arbol.nroArbol = Poda.nroArbol)
WHERE ((Poda.fecha BETWEEN '2017/01/01' AND '2017/12/31') 
        AND (nroArbol NOT IN (SELECT Arbol.nroArbol
                              FROM Arbol INNER JOIN Poda ON (Arbol.nroArbol = Poda.nroArbol)
                                          INNER JOIN Localidad ON (Arbol.codigoPostal = Localidad.codigoPostal)
                                          WHERE ((Poda.fecha BETWEEN '2018/01/01' AND '2018/12/31')))));
```

5. Reportar DNI, nombre, apellido, fnac y localidad donde viven podadores con apellido
   terminado con el string ‘ata’ y que el podador tenga al menos una poda durante 2018.
   Ordenar por apellido y nombre.

```sql
SELECT DISTINCT (Podador.DNI, nombre, apellido, fnac, nombreL)
FROM Podador INNER JOIN Poda ON (Podador.DNI = Poda.DNI)
             INNER JOIN Localidad ON (Podador.codigoPostalVive = Localidad.codigoPostal)
WHERE ((Podador.apellido LIKE '%ata') AND (Poda.fecha BETWEEN '2018/01/01' AND '2018/12/31'));
GROUP BY Podador.DNI, nombre, apellido, fnac, nombreL
HAVING COUNT(*) >= 1
ORDER BY apellido, Podador.nombre
```

6. Listar DNI, apellido, nombre, teléfono y fecha de nacimiento de podadores que solo podaron
   árboles de especie ‘Coníferas’.

```sql
SELECT DISTINCT (Podador.DNI, nombre, apellido, fnac)
FROM Podador INNER JOIN Poda ON (Podador.DNI = Poda.DNI)
             INNER JOIN Arbol ON (Arbol.nroArbol = Poda.nroArbol)
WHERE ((Arbol.especie = 'Coníferas') AND (Podador.DNI NOT IN(SELECT Podador.DNI
                                                             FROM Podador INNER JOIN Poda ON (Podador.DNI = Poda.DNI)
                                                                          INNER JOIN Arbol ON (Arbol.nroArbol = Poda.nroArbol)
                                                             WHERE ((Arbol.especie <> 'Coníferas')))));
```

7. Listar especie de árboles que se encuentren en la localidad de ‘La Plata’ y también en la
   localidad de ‘Salta’.

```sql
SELECT especie
FROM Arbol A INNER JOIN Localidad ON (A.codigoPostal = Localidad.codigoPostal)
WHERE ((nombreL = 'La Plata') AND (A.nroArbol IN (SELECT A.nroArbol
                                                  FROM A INNER JOIN Localidad ON (A.codigoPostal = Localidad.codigoPostal)
                                                  WHERE ((nombreL = 'Salta')))));
```

8. Eliminar el podador con DNI: 22234566.

```sql
DELETE FROM Poda WHERE DNI = 22234566 ;
DELETE FROM Podador WHERE DNI = 22234566;
```
9. Reportar nombre, descripción y cantidad de habitantes de localidades que tengan menos de
   100 árboles.

```sql
SELECT nombreL, descripción, #habitantes
FROM Arbol A INNER JOIN Localidad ON (A.codigoPostal = Localidad.codigoPostal)
GROUP BY nombreL, descripción, #habitantes
HAVING COUNT(*) < 100;
```
