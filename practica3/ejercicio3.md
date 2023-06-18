# Ejercicio 3: 

**Agencia (RAZON_SOCIAL, dirección, telef, e-mail)** 

**Ciudad (CODIGOPOSTAL, nombreCiudad, añoCreación)**

**Cliente (DNI, nombre, apellido, teléfono, dirección)** 

**Viaje(FECHA, HORA, DNI, cpOrigen (Fk), cpDestino (Fk), razon_social(Fk), descripcion) //cpOrigen y cpDestino corresponden a la ciudades origen y destino del viaje** 

1. Eliminar el cliente con DNI:25326992. 
      
        Cliente25326992 ⇐ σ DNI = 25326992 (Cliente)
        Viajes25326992 ⇐ π FECHA, HORA, DNI, cpOrigen (Fk), cpDestino (Fk), razon_social(Fk), descripcion (Cliente25326992 |x| Viaje)
        Viaje ⇐ Viaje - Viajes25326992 
        Cliente ⇐ Cliente - Cliente25326992

2. Listar datos personales de clientes que solo realizaron viajes locales.(En cada viaje realizado coincide la localidad origen con la destino, cpOrigen y cpDestino).

        ClientesConViajesLocales ⇐ Cliente |x| (σ (cpOrigen = cpDestino) (Viaje))
        π DNI, nombre, apellido, teléfono, dirección (ClientesConViajesLocales - ((Cliente |x| Viaje) - ClientesConViajesLocales))


3. Listar información de agencias que no tengan viajes para el cliente DNI:22222222 durante el primer semestre de 2020. 
      
        ViajesPS22222222 ⇐ π razon_social ((σ (DNI >= 22222222) (Cliente)) |x| (σ (FECHA >= ‘01/01/2020’) ^ (FECHA <= ‘31/06/2020’) (Viaje)))
        π RAZON_SOCIAL, dirección, telef, e-mail ((Agencia |x| (Cliente |x| Viaje)) - (Agencia |x| ViajesPS22222222))


4. Listar información de agencias que realizaron viajes durante 2019 y no realizaron viajes durante 2020. 
      
        AgenciasViajes2019 ⇐ Agencia |x| (σ (FECHA >= ‘01/01/2019’) ^ (FECHA <= ‘31/12/2019’) (Viaje)) 
        AgenciasViajes2020 ⇐ Agencia |x| (σ (FECHA >= ‘01/01/2020’) ^ (FECHA <= ‘31/12/2020’) (Viaje))
        π RAZON_SOCIAL, dirección, telef, e-mail (AgenciasViajes2019 - AgenciasViajes2020)


5. Agregar una agencia de viajes con los datos que desee.

        Agencia ⇐ Agencia U {(“Kuelap”, “Tupac Amaru II”, “987654321”, kuelapProducciones@gmail.com)}


6. Listar datos personales de clientes que viajaron con ciudad destino ‘Lincoln’ pero no realizaron viajes con origen ‘La Plata’. 
        
        ClientesDestinoLincoln ⇐ Cliente |x| (σ (cpDestino = CODIGOPOSTAL) (Viaje x (σ (nombreCiudad = “Linconln”) (Ciudad))))
        ClientesOrigenLaPlata ⇐ Cliente |x| (σ (cpOrigen = CODIGOPOSTAL) (Viaje x (σ (nombreCiudad = “La Plata”) (Ciudad))))
        π DNI, nombre, apellido, teléfono, dirección (ClientesDestinoLincoln - ClientesOrigenLaPlata)

7. Listar nombre, apellido, dirección y teléfono de clientes que viajaron con todas las agencias. 
        
        (π DNI, nombre, apellido, teléfono, dirección, RAZON_SOCIAL (Cliente |x| Viaje)) % (π RAZON_SOCIAL (Agencia)))


8. Listar código postal, nombre Ciudad y año creación de ciudades que no recibieron viajes durante 2020.

        CiudadesViajes2020 ⇐ σ (cpDestino = CODIGOPOSTAL) ((σ (FECHA >= ‘01/01/2020’) ^ (FECHA <= ‘31/12/2020’) (Viaje)) x Ciudad)
        π CODIGOPOSTAL, nombreCiudad, añoCreación ((σ (cpDestino = CODIGOPOSTAL) (Viaje x Ciudad)) - π CODIGOPOSTAL, nombreCiudad, añoCreación(CiudadesViajes2020))
9. Reportar información de agencias que realizaron viajes durante 2019 o que tengan dirección igual a ‘General Pinto’. 

        AgenciasConViajes2019 ⇐ π RAZON_SOCIAL, dirección, telef, e-mail (Agencia |x| (σ (FECHA >= ‘01/01/2019’) ^ (FECHA <= ‘31/12/2019’) (Viaje)))
        π RAZON_SOCIAL, dirección, telef, e-mail (AgenciasConViajes2019 U (σ (dirección =  ‘General Pinto’) (Agencia)))


10. Actualizar el teléfono del cliente con DNI: 2789655 a: 221-4400345. 
        
        δ teléfono ⇐ ‘221-4400345’ (σ (DNI =  ‘2789655’) (Cliente)