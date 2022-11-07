# Ejercicio 6:

**Técnico (codTec, nombre, especialidad) // técnicos**

**Repuesto (codRep, nombre, stock, precio) // repuestos**

**RepuestoReparacion (nroReparac, codRep, cantidad, precio)** //repuestos utilizados en
reparaciones.

**Reparación (nroReparac, codTec, precio_total, fecha)** //reparaciones realizadas.

1. Listar todos los repuestos, informando el nombre, stock y precio. Ordenar el
resultado por precio.

```sql
SELECT nombre, stock, precio
FROM Repuesto
ORDER BY precio;
```

2. Listar nombre, stock, precio de repuesto que participaron en reparaciones durante
2019 y además no participaron en reparaciones del técnico ‘José Gonzalez’.

```sql
SELECT DISTINCT Repuesto.nombre, Repuesto.stock, Repuesto.precio
FROM Repuesto R INNER JOIN RepuestoReparacion RR ON (R.codRep = RR.codRep)
                INNER JOIN Reparacion ON (RR.nroReparac = Reparacion.nroReparac)
WHERE (Reparacion.fecha BETWEEN '2019/01/01' AND '2019/12/31')
EXCEPT (
        SELECT DISTINCT Repuesto.nombre, Repuesto.stock, Repuesto.precio
        FROM Repuesto R INNER JOIN RepuestoReparacion RR ON (R.codRep = RR.codRep)
                        INNER JOIN Reparacion ON (RR.nroReparac = Reparacion.nroReparac)
                        INNER JOIN Tecnico ON (Reparacion.codTec = Tecnico.codTec)
        WHERE (Tecnico.nombre = "Jose Gonzalez")
);
```

3. Listar el nombre, especialidad de técnicos que no participaron en ninguna
reparación. Ordenar por nombre ascendentemente.

```sql
SELECT Técnico.nombre, especialidad
FROM Técnico LEFT JOIN Reparación ON (Técnico.codTec = Reparación.codTec)
WHERE Reparación.codTec IS NULL
ORDER BY Técnico.nombre;
```

4. Listar el nombre, especialidad de técnicos solo participaron en reparaciones durante 2018.

```sql
SELECT Técnico.nombre, especialidad
FROM Técnico INNER JOIN Reparación ON (Técnico.codTec = Reparación.codTec)
WHERE (Reparación.fecha BETWEEN '2018/01/01' AND '2018/12/31')
EXCEPT (
    SELECT Técnico.nombre, Técnico.especialidad
    FROM Tecnico INNER JOIN Reparacion (Técnico.codTec = Reparacion.codTec)
    WHERE NOT(Reparacion.fecha BETWEEN "01/01/2018" AND "31/12/2018")
);
```

5. Listar para cada repuesto nombre, stock y cantidad de técnicos distintos que lo
utilizaron. Si un repuesto no participó en alguna reparación igual debe aparecer en
dicho listado.

```sql
SELECT Repuesto.nombre, stock, COUNT(DISTINCT Reparación.codTec) AS cantidad_tecnicos
FROM Repuesto LEFT JOIN RepuestoReparacion RR ON (Repuesto.codRep = RR.codRep)
              LEFT JOIN Reparación ON (RR.nroReparac = Reparación.nroReparac)
GROUP BY Repuesto.nombre, stock;
```

6. Listar nombre y especialidad del técnico con mayor cantidad de reparaciones
realizadas y el técnico con menor cantidad de reparaciones.

```sql
SELECT Técnico.nombre, especialidad
FROM Técnico INNER JOIN Reparación ON (Técnico.codTec = Reparación.codTec)
GROUP BY Técnico.nombre, especialidad
HAVING COUNT(Reparación.nroReparac) >= ALL (
    SELECT COUNT(Reparación.nroReparac)
    FROM Técnico INNER JOIN Reparación ON (Técnico.codTec = Reparación.codTec)
    GROUP BY Técnico.nombre, especialidad
    )
    OR
    COUNT(Reparación.nroReparac) <= ALL (
    SELECT COUNT(Reparación.nroReparac)
    FROM Técnico INNER JOIN Reparación ON (Técnico.codTec = Reparación.codTec)
    GROUP BY Técnico.nombre, especialidad
    );
```

7. Listar nombre, stock y precio de todos los repuestos con stock mayor a 0 y que
dicho repuesto no haya estado en reparaciones con precio_total superior a 10000.

```sql
SELECT Repuesto.nombre, stock, precio
FROM Repuesto LEFT JOIN RepuestoReparacion RR ON (Repuesto.codRep = RR.codRep)
              LEFT JOIN Reparación ON (RR.nroReparac = Reparación.nroReparac)
WHERE (stock > 0) AND (Reparación.precio_total <= 10000);
```

8. Proyectar precio, fecha y precio total de aquellas reparaciones donde se utilizó algún
repuesto con precio en el momento de la reparación mayor a $1000 y menor a
$5000.
9. Listar nombre, stock y precio de repuestos que hayan sido utilizados en todas las
reparaciones
10. Listar fecha, técnico y precio total de aquellas reparaciones que necesitaron al
menos 10 repuestos distintos.
