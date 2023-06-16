# Ejercicio 6: 

**Proyecto(codProyecto, nombrP, descripcion, fechaInicioP, fechaFinP, fechaFinEstimada, DNIResponsable(fk), equipoBackend(fk), equipoFrontend(fk)) //DNIResponsable corresponde a un empleado, equipoBackend y equipoFrontend corresponden a un equipo** 

**Equipo(codEquipo, nombreE, descripcionTecnologias, DNILider(fk)) //DNILider corresponde a un empleado** 

**Empleado(DNI, nombre, apellido, telefono, direccion, fechaIngreso)** 

**Empleado_Equipo(codEquipo, DNI, fechaInicio, fechaFin, descripcionRol)** 


1. Listar nombre, descripción, fecha de inicio y fecha de fin de proyectos ya finalizados que no fueron terminados antes de la fecha de fin estimada.

        π nombrP, descripcion, fechaInicioP, fechaFinP (σ fechaFinP > fechaFinEstimada (σ fechaFinP < '15/06/23' (Proyecto))) 


2. Listar DNI, nombre, apellido, teléfono, dirección y fecha de ingreso de empleados que no hayan sido
responsables de proyectos.

        ResponsablesProyectos ⇐ π DNI, nombre, apellido, telefono, direccion, fechaIngreso (σ DNI = DNIResponsable (Empleado X Proyecto))

        π DNI, nombre, apellido, telefono, direccion, fechaIngreso (Empleado - ResponsablesProyectos)
    
3. Listar DNI, nombre, apellido, teléfono y dirección de todos los empleados que trabajan en el proyecto con nombre ‘Proyecto X’. No es necesario informar responsable y líderes.

        ProyectoX ⇐ π equipoBackend, equipoFrontend (σ nombreP = ‘Proyecto X’ (Proyecto))

        EquiposX ⇐ π codEquipo (σ Equipo.codEquipo = equipoBackend v Equipo.codEquipo = equipoFrontend (Equipo X ProyectoX))

        π DNI, nombre, apellido, telefono, direccion (σ Empleado.DNI = Empleado_Equipo.DNI (Empleado X (σ Empleado_Equipo.codEquipo = EquiposX.codEquipo  (Empleado_Equipo X EquiposX))))

4. Listar nombre de equipo y datos personales de líderes de equipos que no tengan empleados
asignados y trabajen con tecnología ‘Java’.

        EquiposEmpleados ⇐ Empleado X (Empleado_Equipo X Equipo)

        EquiposSinEmpleados ⇐ Equipo - EquiposEmpleados

        π nombreE, DNI, nombre, apellido, telefono, direccion, fechaIngreso (σ Empleado.DNI = DNILider (Empleado X EquiposSinEmpleados))
        
5. Modificar nombre, apellido y dirección del empleado con DNI: 40568965 con los datos que desee.

        Empleado40568965 ⇐ σ DNI = 40568965 (Empleado)

        δ nombre ⇐ ‘Pancracio’ (Empleado40568965)

        δ apellido ⇐ ‘Chuquipoma’ (Empleado40568965)

        δ direccion ⇐ ‘calle 13’ (Empleado40568965)

6. Listar DNI, nombre, apellido, teléfono y dirección de empleados que son responsables de proyectos
pero no han sido líderes de equipo.

        ResponsablesProyectos ⇐ π DNI, nombre, apellido, telefono, direccion (σ DNI = DNIResponsable (Empleado X Proyecto))

        Lideres ⇐ π DNI, nombre, apellido, telefono, direccion(σ DNI = DNILider (Empleado X Equipo))

        π DNI, nombre, apellido, telefono, direccion (ResponsablesProyectos - Lideres)

7. Listar nombre de equipo y descripción de tecnologías de equipos que hayan sido asignados como
equipos frontend y backend.

        Front ⇐ π codEquipo, nombreE, descripcionTecnologias, DNILider(σ codEquipo = equipoFrontend (Equipo X Proyecto))

        Back ⇐ π codEquipo, nombreE, descripcionTecnologias, DNILider(σ codEquipo = equipoBackend (Equipo X Proyecto))
        
        π nombreE, descripcionTecnologias (Front n Back)



