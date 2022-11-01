Ejercicio 4:
PERSONA = (DNI, Apellido, Nombre, Fecha_Nacimiento, Estado_Civil, Genero)
ALUMNO = (DNI, Legajo, Año_Ingreso)
PROFESOR = (DNI, Matricula, Nro_Expediente)
TITULO = (Cod_Titulo, Nombre, Descripción)
TITULO-PROFESOR = (Cod_Titulo, DNI, Fecha)
CURSO = (Cod_Curso, Nombre, Descripción, Fecha_Creacion, Duracion)
ALUMNO-CURSO = (DNI, Cod_Curso, Año, Desempeño, Calificación)
PROFESOR-CURSO = (DNI, Cod_Curso, Fecha_Desde, Fecha_Hasta)

1. Listar DNI, legajo y apellido y nombre de todos los alumnos que tegan año ingreso
inferior a 2014.

```sql
SELECT DNI, legajo, apellido, nombre
FROM PERSONA INNER JOIN ALUMNO ON (PERSONA.DNI = ALUMNO.DNI)
WHERE Año_Ingreso < 2014
```

2. Listar DNI, matricula, apellido y nombre de los profesores que dictan cursos que tengan
más 100 horas de duración. Ordenar por DNI

```sql
SELECT DNI, Apellido, Nombre, Género Fecha de nacimiento
FROM PERSONA INNER JOIN PROFESOR ON (PERSONA.DNI = PROFESOR.DNI)
              INNER JOIN PROFESOR-CURSO ON (PROFESOR.DNI = PROFESOR-CURSO.DNI)
              INNER JOIN CURSO ON (PROFESOR-CURSO.Cod_Curso = CURSO.Cod_Curso)
WHERE CURSO.DURACION > 100;
```

3. Listar el DNI, Apellido, Nombre, Género y Fecha de nacimiento de los alumnos inscriptos
al curso con nombre “Diseño de Bases de Datos” en 2019.

```sql
SELECT DNI, Apellido, Nombre, Género Fecha de nacimiento
FROM PERSONA INNER JOIN PROFESOR ON (PERSONA.DNI = PROFESOR.DNI)
              INNER JOIN PROFESOR-CURSO ON (PROFESOR.DNI = PROFESOR-CURSO.DNI)
              INNER JOIN CURSO ON (PROFESOR-CURSO.Cod_Curso = CURSO.Cod_Curso)
WHERE CURSO.DURACION > 100;
```

4. Listar el DNI, Apellido, Nombre y Calificación de aquellos alumnos que obtuvieron una
calificación superior a 9 en los cursos que dicta el profesor “Juan Garcia”. Dicho listado
deberá estar ordenado por Apellido.
5. Listar el DNI, Apellido, Nombre y Matrícula de aquellos profesores que posean más de 3
títulos. Dicho listado deberá estar ordenado por Apellido y Nombre.
6. Listar el DNI, Apellido, Nombre, Cantidad de horas y Promedio de horas que dicta cada
profesor. La cantidad de horas se calcula como la suma de la duración de todos los
cursos que dicta.
7. Listar Nombre, Descripción del curso que posea más alumnos inscriptos y del que posea
menos alumnos inscriptos durante 2019.
8. Listar el DNI, Apellido, Nombre, Legajo de alumnos que realizaron cursos con nombre
conteniendo el string ‘BD’ durante 2018 pero no realizaron ningún curso durante 2019.
9. Agregar un profesor con los datos que prefiera y agregarle el título con código: 25.
10. Modificar el estado civil del alumno cuyo legajo es ‘2020/09’, el nuevo estado civil es
divorciado.
11. Dar de baja el alumno con DNI 30568989. Realizar todas las bajas necesarias para no
dejar el conjunto de relaciones en estado inconsistente.
