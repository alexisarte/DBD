# Ejercicio 2

**AGENCIA (RAZON_SOCIAL, dirección, telef, e-mail)**

**CIUDAD (CODIGOPOSTAL, nombreCiudad, añoCreación)**

**CLIENTE (DNI, nombre, apellido, teléfono, dirección)**

**VIAJE( FECHA, HORA, DNI, cpOrigen(fk), cpDestino(fk), razon_social(fk), descripcion)**

//cpOrigen y cpDestino corresponden a la ciudades origen y destino del viaje

1. Listar razón social, dirección y teléfono de agencias que realizaron viajes desde la ciudad de
   ‘La Plata’ (ciudad origen) y que el cliente tenga apellido ‘Roma’. Ordenar por razón social y
   luego por teléfono.

```sql
SELECT AGENCIA.RAZON_SOCIAL, AGENCIA.dirección, AGENCIA.telef
    FROM AGENCIA
    INNER JOIN VIAJE ON (AGENCIA.RAZON_SOCIAL = VIAJE.razon_social)
    INNER JOIN CLIENTE ON (VIAJE.DNI = CLIENTE.DNI)
    INNER JOIN CIUDAD ON (VIAJE.cpOrigen = CIUDAD.CODIGOPOSTAL)
WHERE CIUDAD.nombreCiudad = 'La Plata' AND CLIENTE.apellido = 'Roma'
ORDER BY AGENCIA.RAZON_SOCIAL, AGENCIA.telef;
```

2. Listar fecha, hora, datos personales del cliente, ciudad origen y destino de viajes realizados
   en enero de 2019 donde la descripción del viaje contenga el String ‘demorado’.

```sql
SELECT fecha, hora, C.DNI, nombre, apellido, teléfono, dirección, O.nombreCiudad, D.nombreCiudad
FROM VIAJE V INNER JOIN CIUDAD O ON (V.cpOrigen = O.CODIGOPOSTAL)
    INNER JOIN CIUDAD D ON (V.cpDestino = D.CODIGOPOSTAL)
    INNER JOIN CLIENTE C ON (V.DNI = C.DNI)
WHERE ((V.FECHA BETWEEN '2019/01/01' AND '2019/01/31') AND (V.descripcion LIKE '%demorado%'));
```

3. Reportar información de agencias que realizaron viajes durante 2019 o que tengan dirección
   de mail que termine con ‘@jmail.com’.

```sql
SELECT RAZON_SOCIAL, dirección, telef, email
FROM AGENCIA NATURAL JOIN VIAJE
WHERE ((VIAJE.FECHA BETWEEN '2019/01/01' AND '2019/12/31') OR (AGENCIA.email LIKE '%@jmail.com'));
```

4. Listar datos personales de clientes que viajaron solo con destino a la ciudad de ‘Coronel
   Brandsen’

```sql
SELECT DNI, nombre, apellido, teléfono, dirección FROM CLIENTE NATURAL JOIN VIAJE
    INNER JOIN CIUDAD ON (VIAJE.cpDestino = CIUDAD.CODIGOPOSTAL)
WHERE CIUDAD.nombreCiudad = 'Coronel Brandsen'
EXCEPT
(SELECT DNI, nombre, apellido, teléfono, dirección FROM CLIENTE NATURAL JOIN VIAJE
    INNER JOIN CIUDAD ON (VIAJE.cpDestino = CIUDAD.CODIGOPOSTAL)
WHERE CIUDAD.nombreCiudad <> 'Coronel Brandsen');
```

5. Informar cantidad de viajes de la agencia con razón social ‘TAXI Y’ realizados a ‘Villa Elisa’.

```sql
SELECT RAZON_SOCIAL, dirección, telef, email, COUNT(*)
FROM AGENCIA NATURAL JOIN VIAJE
    INNER JOIN CIUDAD ON (VIAJE.cpDestino = CIUDAD.CODIGOPOSTAL)
WHERE (AGENCIA.RAZON_SOCIAL = 'TAXI Y' AND CIUDAD.nombreCiudad = 'Villa Elisa');
```

6. Listar nombre, apellido, dirección y teléfono de clientes que viajaron con todas las agencias.

```sql
SELECT nombre, apellido, teléfono, dirección
FROM CLIENTE
WHERE NOT EXIST (SELECT *
                    FROM AGENCIA
                    WHERE NOT EXIST (SELECT *
                                        FROM VIAJE
                                        WHERE (CLIENTE.DNI = VIAJE.DNI AND VIAJE.razon_social = AGENCIA.RAZON_SOCIAL)));
```

7. Modificar el cliente con DNI: 38495444 actualizando el teléfono a: 221-4400897.

```sql
UPDATE CLIENTE
SET teléfono = 221-4400897
WHERE DNI = 38495444;
```

8. Listar razon_social, dirección y teléfono de la/s agencias que tengan mayor cantidad de
   viajes realizados.

```sql
SELECT RAZON_SOCIAL, dirección, telef
FROM AGENCIA NATURAL JOIN VIAJE
GROUP BY RAZON_SOCIAL, dirección, telef
HAVING COUNT(*) = (SELECT MAX(COUNT(*))
                    FROM AGENCIA NATURAL JOIN VIAJE
                    GROUP BY RAZON_SOCIAL, dirección, telef);
```

9. Reportar nombre, apellido, dirección y teléfono de clientes con al menos 10 viajes.

```sql
SELECT  nombre, apellido, teléfono, dirección
FROM CLIENTE C
WHERE (SELECT COUNT(*)
        FROM C INNER JOIN VIAJE ON (c.DNI = VIAJE.DNI)
        GROUP BY DNI) >= 10;
```

10. Borrar al cliente con DNI 40325692.
```sql
DELETE FROM CLIENTE
WHERE DNI = 40325692;
```
