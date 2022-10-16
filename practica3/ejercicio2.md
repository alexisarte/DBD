# Ejercicio 2: 

**Banda(codigoB, nombreBanda, genero_musical, año_creacion)**

**Integrante (DNI, nombre, apellido, dirección, email, fecha_nacimiento,codigoB(fk))**

**Escenario(nroEscenario, nombre _ escenario, ubicación, cubierto, m2, descripción)**

**Recital(fecha, hora, nroEscenario, codigoB(fk))**


1. Listar datos personales de integrantes con apellido ‘Garcia’ o fecha de nacimiento anterior a 2005 que toquen en bandas de rock and roll.

        π DNI, nombre, apellido, dirección, email, fecha_nacimiento (σ (apellido = ‘Garcia’) v (fecha_nacimiento < ‘01/01/2005’) (Integrante) |x| (σ genero_musical = ‘rock and roll’ (Banda)


2. Listar nombre de escenario, ubicación y descripción de escenarios que no tuvieron recitales durante 2019.

        π nombre _ escenario, ubicación, descripción (Escenario) - π nombre _ escenario, ubicación, descripción (Escenario |x| (σ (fecha >= ‘01/01/2019’) ^ (fecha <= ‘31/12/2019’) (Recital)))


3. Listar nombre de escenario, ubicación y descripción de escenarios que tuvieron recitales con género musical rock and roll o tuvieron recitales durante 2020.

        π nombre _ escenario, ubicación, descripción (Escenario |x| ((σ (genero_musical = ‘rock and roll’) (Banda)) |x| Recital))) U π nombre _ escenario, ubicación, descripción (Escenario |x| (Banda |x| (σ (fecha >= ‘01/01/2020’) ^ (fecha <= ‘31/12/2020’) (Recital)) bien creo


4. Listar nombre, género musical y año de creación de bandas que hayan realizado recitales en escenarios cubiertos durante 2019 .// cubierto es true, false según corresponda
    
        π nombreBanda, genero_musical, año_creacion (Banda |x| (σ (fecha >= ‘01/01/2019’) ^ (fecha <= ‘31/12/2019’)  (Recital)) |x| (σ (cubierto) (Escenario))))

5. Listar DNI, nombre, apellido, dirección y email de integrantes nacidos entre 2000 y 2005 y que toquen en bandas con género pop que hayan tenido recitales durante 2020. 

    Integrantes20-25 ⇐ (σ (fecha_nacimiento >= ‘01/01/2000’) ^ (fecha_nacimiento <= ‘31/12/2005’) (Integrante))
    
    Recitales2020 ⇐ (σ (fecha >= ‘01/01/2019’) ^ (fecha <= ‘31/12/2019’)  (Recital))
    
    π DNI, nombre, apellido, dirección, email (Integrantes20-25 |x| ((σ (genero_musical = pop) (Banda)) |x| Recitales2020))


6. Listar DNI, nombre, apellido,email de integrantes que hayan tocado en el escenario con nombre ‘Gustavo Cerati’ y no hayan tocado en el escenario con nombre ‘Carlos Gardel’. 
    
        IntegrantesGustavoCerati ⇐ Integrante |x| (Banda |x| (Recital |x| (σ (nombre _ escenario = ‘Gustavo Cerati’) (Escenario))))
        IntegrantesCarlosGardel ⇐ Integrante |x| (Banda |x| (Recital |x| (σ (nombre _ escenario = ‘Carlos Gardel’) (Escenario))))
        π DNI, nombre, apellido, email (IntegrantesGustavoCerati - IntegrantesCarlosGardel)

7. Modificar el año de creación de la banda de nombre ‘Ratones Paranoicos’ a: 1983.

        δ año_creacion ⇐ 1983 (σ (nombreBanda = ‘Ratones Paranoicos’) (Banda))


8. Reportar nombre, género musical y año de creación de bandas que hayan realizado recitales durante 2019, y además hayan tocado durante 2020.
    
        Bandas2019 ⇐ Banda |x| (σ (fecha >= ‘01/01/2019’) ^ (fecha <= ‘31/12/2019’)  (Recital))
        Bandas2020 ⇐ Banda |x| (σ (fecha >= ‘01/01/2020’) ^ (fecha <= ‘31/12/2020’)  (Recital))
        π nombreBanda, genero_musical, año_creacion (Bandas2019 ∩ Bandas2020)


9. Listar el cronograma de recitales del dia 04/12/2019. Se deberá listar: nombre de la banda que ejecutará el recital, fecha, hora, y el nombre y ubicación del escenario correspondiente. 
    
        π nombreBanda, fecha, hora, nombre _ escenario, ubicación (Banda |x| ((σ (fecha = ‘04/12/2019’) (Recital)) |x| Escenario))