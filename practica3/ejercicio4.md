# Ejercicio 4:


**Equipo(codigoE, nombreE, descripcionE)** 

**Integrante (DNI, nombre, apellido, ciudad,email, telefono,codigoE(fk))** 

**Laguna(nroLaguna, nombreL, ubicación, extension, descripción)** 

**TorneoPesca(codTorneo, fecha, hora, nroLaguna(fk), descripcion)**

**Inscripcion(codTorneo, codigoE, asistio, gano) // asistio y gano son true o false según corresponda** 


1. Listar DNI, nombre, apellido y email de integrantes que sean de la ciudad ‘La Plata’ y estén inscriptos en torneos que se disputaron durante 2019. 

        π DNI, nombre, apellido, email (((σ (ciudad = ‘La Plata’) (Integrante)) |x| Equipo)) |x| (Inscripcion |x|  (σ (FECHA >= ‘01/01/2019’) ^ (FECHA <= ‘31/12/2019’) (TorneoPesca)))


2. Reportar nombre y descripción de equipos que solo se hayan inscripto en torneos de 2019.
Torneos2019 ⇐ Equipo |x| (Inscripcion |x| (σ (fecha >= ‘01/01/2019’) ^ (fecha <= ‘31/12/2019’) (TorneoPesca)))

        π nombreE, descripcionE (Torneos2019 - ((Equipo |x| Inscripcion |x| TorneoPesca) - Torneos2019))


3. Listar nombre, ubicación, extensión y descripción de lagunas que hayan tenido torneos durante 2019 y no hayan tenido torneos durante 2020. 

        LagunasTorneos2019 ⇐ Laguna |x|  (π nroLaguna (σ (fecha >= ‘01/01/2019’) ^ (fecha <= ‘31/12/2019’) (TorneoPesca)))

        LagunasTorneos2020 ⇐ Laguna |x|  (π nroLaguna (σ (fecha >= ‘01/01/2020’) ^ (fecha <= ‘31/12/2020’) (TorneoPesca)))

        π nombreL, ubicación,extension, descripción (LagunasTorneos2019 - LagunasTorneos2020)

4. Listar para la laguna con nombre ‘laguna x’, nombre y descripción de equipos ganadores de torneos que se disputaron durante 2019 en la mencionada laguna. 

        Ganadores2019 ⇐ σ (gano) (Equipo |x| (Inscripcion |x| (σ (fecha >= ‘01/01/2019’) ^ (fecha <= ‘31/12/2019’) (TorneoPesca))))

        π nombreE, descripcionE ((σ (nombreL = ‘laguna x’) (Laguna)) |x| Ganadores2019)


5. Reportar nombre, y descripción de equipos que tengan inscripciones en todas las lagunas.

        π nombreE, descripcionE (((Equipo |x| Inscripcion) |x| (π nroLaguna (TorneoPesca))) % Laguna)


6. Eliminar el equipo con código: 10000. 
          
        Equipo ⇐ Equipo - (σ codigoE = 10000 (Equipo))
        
7. Listar nombreL, ubicación,extensión y descripción de lagunas que no tuvieron torneos.

        π nombreL, ubicación,extension, descripción (Laguna) - π nombreL, ubicación,extension, descripción (Laguna |x| TorneoPesca)

8. Reportar nombre, y descripción de equipos que tengan inscripciones a torneos a disputarse durante 2019, pero no tienen inscripciones a torneos de 2020. 

        Torneos2019 ⇐ Equipo |x| (Inscripcion |x| (σ (fecha >= ‘01/01/2019’) ^ (fecha <= ‘31/12/2019’) (TorneoPesca)))

        Torneos2020 ⇐ Equipo |x| (Inscripcion |x| (σ (fecha >= ‘01/01/2020’) ^ (fecha <= ‘31/12/2020’) (TorneoPesca)))

        π nombreE, descripcionE (Torneos2019 - Torneos2020))


9. Listar DNI, nombre, apellido, ciudad y email de integrantes que asistieron o ganaron algún torneo que se disputó en la laguna con nombre: ‘Laguna Brava’.

        TorneosLagunaBrava ⇐ π codTorneo, nroLaguna (TorneoPesca) |x| (π nroLaguna (σ (nombreL = ‘Laguna Brava’) (Laguna)))

        IntegrantesLagunaBrava ⇐ σ (gano v asistio) (Integrante |x| (Equipo |x| (Inscripcion |x| TorneosLagunaBrava)))

        π DNI, nombre, apellido, ciudad, email (σ (gano v asistio) (Integrante |x| (Equipo |x| (Inscripcion |x| TorneosLagunaBrava))))  