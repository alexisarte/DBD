# Ejercicio 5: 

**Club=(codigoClub, nombre, anioFundacion, codigoCiudad(FK))** 

**Ciudad=(codigoCiudad, nombre)** 

**Estadio=(codigoEstadio, codigoClub(FK), nombre, direccion)** 

**Jugador=(dni, nombre, apellido, edad, codigoCiudad(FK))** 

**ClubJugador(codigoClub, dni, desde, hasta)** 


1. Reportar nombre y año de fundación de clubes de la ciudad de La Plata, además del nombre y dirección del estadio del mismo.
    
        ClubsLaPlata ⇐ Club |x| (π codigoCiudad (σ ciudad = ‘La Plata’ (Ciudad)))
        π club.nombre, anioFundacion, estadio.nombre, direccion ((σ Club.codigoClub = Estadio.codigoClub) (Estadio)) x ClubsLaPlata)

2. Listar datos personales de jugadores actuales del club River Plate que hayan jugado en el club Boca Juniors. 
                    
        JugadoresRiver ⇐ σ hasta = null (((σ nombre = ‘River Plate’ (Club)) |x| ClubJugador) |x| Jugador)
        ExJugadoresBoca ⇐ σ hasta != null(((σ nombre = ‘Boca Juniors’ (Club)) |x| ClubJugador) |x| Jugador)
        π nombre, apellido, edad (JugadoresRiver ∩ ExJugadoresBoca)                   

3. Listar información de todos los clubes donde se desempeñó el jugador: Marcelo Gallardo. Indicar nombre, año de fundación y localidad del club.
      
        MarceloGallardo ⇐ σ (nombre = ‘Marcelo’ ∧ apellido = ‘Gallardo’) (Jugador)
        ClubesGallardo ⇐ MarceloGallardo |x| ClubJugador |x| Club
        π nombre, anioFundacion, ciudad.nombre (σ ClubesGallardo.codigoCiudad = Ciudad.codigoCiudad (ClubesGallardo x Ciudad))

4. Reportar dni, nombre y apellido de aquellos jugadores que no tengan más de 25 años y jueguen en algún club de la ciudad de Junín. 
          
        JugadoresMenores25 ⇐ σ edad < 25 (Jugador)
        JugadoresJuninMenores25 ⇐ (JugadoresMenores25 |x| ClubJugador) |x| (π codigoCiudad (σ nombre = ‘Junin’ (Ciudad)))
        π dni, nombre, apellido (JugadoresJuninMenores25)

5. Mostrar el nombre de los clubes que tengan jugadores de la ciudad de Chivilcoy mayores de 25 años.
          
        Chivilcoy ⇐ π codigoCiudad (σ nombre = ‘Chivilcoy’ (Ciudad))
        JugadoresChivilcoyMayores25 ⇐ σ edad > 25 ((Chivilcoy |x| Jugador) |x| ClubJugador)
        π nombre (JugadoresChivilcoyMayores25 |x| Club)

6. Reportar el nombre y apellido de aquellos jugadores que hayan jugado en todos los clubes. 

        (π nombre, apellido, codigoClub (Jugador |x| ClubJugador |x| (π codigoClub (Club)))) % Club

7. Listar nombre de los clubes que no hayan tenido ni tengan jugadores de la ciudad de La Plata. 
  
        JugadoresLaPlata ⇐ π dni ((π codigoCiudad (σ nombre = ‘La Plata’ (Ciudad))) |x| (π codigoCiudad, dni (Jugador)))
        ClubesJugadoresLaPlata ⇐ π nombre ((JugadoresLaPlata |x| ClubJugador) |x| Club)
        π nombre (π nombre (Club) - ClubesJugadoresLaPlata)

8. Mostrar dni, nombre y apellido de aquellos jugadores que jugaron o juegan en el club: Club Atlético Rosario Central. 

        ClubRosarioCentral ⇐ π codigoClub (σ nombre = ‘Club Atlético Rosario Central’ (Club))
        π dni, nombre, apellido ((ClubRosarioCentral |x| ClubJugador) |x| Jugador)

9. Eliminar al jugador cuyo dni es: 24242424.

        ClubJugador ⇐ ClubJugador - (σ dni = 24242424(ClubJugador))
        Jugador ⇐ Jugador - (σ dni = 24242424 (Jugador))
