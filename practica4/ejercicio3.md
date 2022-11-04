# Ejercicio 3:

**Club=(codigoClub, nombre, anioFundacion, codigoCiudad(FK))**

**Ciudad=(codigoCiudad, nombre)**

**Estadio=(codigoEstadio, codigoClub(FK), nombre, direccion)**

**Jugador=(DNI, nombre, apellido, edad, codigoCiudad(FK))**

**ClubJugador(codigoClub, DNI, desde, hasta)**

1. Reportar nombre y anioFundacion de aquellos clubes de la ciudad de La Plata que no
poseen estadio.

```sql
SELECT Club.nombre, anioFundacion 
      FROM Club INNER JOIN Ciudad ON (Club.codigoCiudad = Ciudad.codigoCiudad)
WHERE Ciudad.nombre = 'La Plata'
EXCEPT (
  SELECT Club.nombre, anioFundacion 
      FROM Club INNER JOIN Ciudad ON (Club.codigoCiudad = Ciudad.codigoCiudad)
      INNER JOIN Estadio ON (Estadio.codigoClub = Club.codigoCiudad)
WHERE Ciudad.nombre = 'La Plata'
)
```

2. Listar nombre de los clubes que no hayan tenido ni tengan jugadores de la ciudad de
Berisso.

```sql
SELECT Club.nombre
      FROM Club
EXCEPT (
  SELECT Club.nombre 
        FROM Club INNER JOIN ClubJugador ON (Club.codigoClub = ClubJugador.codigoClub)
        INNER JOIN Jugador ON (ClubJugador.DNI = Jugador.DNI)
        INNER JOIN Ciudad ON (Ciudad.codigoCiudad = Jugador.codigoCiudad)
  WHERE Ciudad.nombre = 'Berisso'
)
```


3. Mostrar DNI, nombre y apellido de aquellos jugadores que jugaron o juegan en el club
Gimnasia y Esgrima La PLata.

```sql
SELECT Jugador.DNI, Jugador.nombre, apellido
        FROM Club INNER JOIN ClubJugador ON (Club.codigoClub = ClubJugador.codigoClub)
        INNER JOIN Jugador ON (ClubJugador.DNI = Jugador.DNI)
WHERE Club.nombre = 'Gimnasia y Esgrima La PLata'
```


4. Mostrar DNI, nombre y apellido de aquellos jugadores que tengan más de 29 años y
hayan jugado o juegan en algún club de la ciudad de Córdoba.

```sql
SELECT Jugador.DNI, Jugador.nombre, apellido
        FROM Club INNER JOIN ClubJugador ON (Club.codigoClub = ClubJugador.codigoClub)
        INNER JOIN Jugador ON (ClubJugador.DNI = Jugador.DNI)
        INNER JOIN Ciudad ON (Ciudad.codigoCiudad = Club.codigoCiudad)
WHERE  Jugador.edad > 29 AND Ciudad.nombre = 'Córdoba'
```


5. Mostrar para cada club, nombre de club y la edad promedio de los jugadores que juegan
actualmente en cada uno.

```sql
SELECT Club.nombre, Jugador.nombre AVG(edad) AS PROMEDIO
      FROM Club INNER JOIN ClubJugador ON (Club.codigoClub = ClubJugador.codigoClub)
      INNER JOIN Jugador ON (ClubJugador.DNI = Jugador.DNI)
WHERE ClubJugador.hasta > '25/10/2022'
GROUP BY Club.codigoClub, Club.nombre, Jugador.nombre
```


6. Listar para cada jugador: nombre, apellido, edad y cantidad de clubes diferentes en los
que jugó. (incluido el actual)

```sql
SELECT Jugador.nombre, apellido, edad COUNT(DNI)
      FROM Club INNER JOIN ClubJugador ON (Club.codigoClub = ClubJugador.codigoClub)
      INNER JOIN Jugador ON (ClubJugador.DNI = Jugador.DNI)
WHERE ClubJugador.hasta > '25/10/2022'
GROUP_BY DNI, Club.nombre, Jugador.nombre
```


7. Mostrar el nombre de los clubes que nunca hayan tenido jugadores de la ciudad de Mar
del Plata.

```sql
SELECT Club.nombre
      FROM Club INNER JOIN ClubJugador ON (Club.codigoClub = ClubJugador.codigoClub)
      INNER JOIN Jugador ON (ClubJugador.DNI = Jugador.DNI)
WHERE Jugador.DNI NOT IN (
      SELECT Jugador.DNI
      FROM Club INNER JOIN ClubJugador ON (Club.codigoClub = ClubJugador.codigoClub)
      INNER JOIN Jugador ON (ClubJugador.DNI = Jugador.DNI)
      INNER JOIN Ciudad ON (ClubJugador.DNI = Ciudad.DNI)
      WHERE Club.nombreClub = 'Mar del Plata';
);
```

8. Reportar el nombre y apellido de aquellos jugadores que hayan jugado en todos los
clubes.

```sql
SELECT nombre, apellido
      FROM Jugador 
      WHERE NOT EXISTS (SELECT * 
                        FROM Club 
                        WHERE NOT EXISTS (SELECT * 
                                          FROM Club
                                          WHERE (Club.codigoClub = ClubJugador.codigoClub) 
                                          AND (ClubJugador.DNI = Jugador.DNI)));
```

9. Agregar con codigoClub 1234 el club “Estrella de Berisso” que se fundó en 1921 y que
pertenece a la ciudad de Berisso. Puede asumir que el codigoClub 1234 no existe en la
tabla Club.

```sql
INSERT INTO Club (codigoClub, nombre, anioFundacion, codigoCiudad) 
VALUES ('Jorge Luis', 'Estrella de Berisso', 1921, 221-4400789, (
      SELECT codigoCiudad FROM Ciudad WHERE Ciudad.nombre = Berisso
));
```
