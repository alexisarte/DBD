### Alquiler de Mobiliario

| Nombre | Cargar Mueble
| ------------- | ------------------------------
| Descripción | Este caso de uso describe el evento en el que el encargado del mobiliario da de alta un mueble. |
| Actores | Encargado de mobiliario |
| Precondiciones | Autenticarse en el sistema |
| Curso normal | Acciones del actor | Acciones del sistema |
|	| Paso 1: El encargado registrado selecciona la opción de iniciar sesión. Paso 3: El encargado ingresa su nombre de usuario y contraseña. | Paso 2: El sistema solicita usuario y contraseña. Paso 4: El sistema verifica los datos ingresados. Paso 5: El sistema registra la sesión iniciada y habilita las acciones del encargado.
| Curso alterno | Acciones del actor | Acciones del sistema |
|| Paso 4: El encargado ingresa un nombre de usuario o contraseña incorrecto. Se notifica la discrepancia. Retoma desde el paso 2 |
| Postcondición | El encargado de mobiliario puede dar de alta un mueble. |
