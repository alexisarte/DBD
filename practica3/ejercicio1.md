# Ejercicio 1:
**Cliente**(`idCliente`, nombre, apellido, DNI, telefono, direccion)

**Factura**(nroTicket, total, fecha, hora, idCliente(Fk)) 

**Detalle**(nroTicket, idProducto, cantidad, preciounitario) 

**Producto**(idProducto, descripcion, precio, nombreP, stock)

1. Listar nombre, apellido, DNI, teléfono y dirección de clientes con DNI superior a 22222222.

    π nombre, apellido, DNI, teléfono, dirección (σ DNI > 22222222 (Cliente))

2. Listar nombre, apellido, DNI, teléfono y dirección de clientes con DNI superior a 22222222 y que tengan facturas cuyo total no supere los $100000. 

    π nombre, apellido, DNI, teléfono, dirección (σ DNI > 22222222 (Cliente) |x| σ total <= 100000 (Factura))

3. Listar nombre, apellido, DNI, teléfono y dirección de clientes que realizaron compras durante 2020.
    π nombre, apellido, DNI, teléfono, dirección (Cliente |x| σ (fecha >= ‘01/01/2020’) ^ (fecha <= ‘31/12/2020’) (Factura))


4. Listar nombre, apellido, DNI, teléfono y dirección de clientes que no realizaron compras durante 2020. 
    
    π nombre, apellido, DNI, teléfono, dirección (Cliente) - π nombre, apellido, DNI, teléfono, dirección (Cliente |x| σ (fecha >= ‘01/01/2020’) ^ (fecha <= ‘31/12/2020’) (Factura))


5. Listar nombre, apellido, DNI, teléfono y dirección de clientes que solo tengan compras durante 2020. 
    
    π nombre, apellido, DNI, teléfono, dirección (Cliente) - π nombre, apellido, DNI, teléfono, dirección (Cliente |x| σ (fecha < ‘01/01/2020’) v (fecha > ‘31/12/2020’) (Factura))


6. Listar nombre, descripción, precio y stock de productos no vendidos. 
    
    π nombre, descripción, precio, stock  - π nombre, descripción, precio, stock  (Producto |x| Detalle)


7. Listar nombre, apellido, DNI, teléfono y dirección de clientes que no compraron el producto con nombre ‘ProductoX’ durante 2020.
    
    π nombre, apellido, DNI, teléfono, dirección  - π nombre, apellido, DNI, teléfono, dirección  (Cliente |x| ((σ (fecha >= ‘01/01/2020’) ^ (fecha <= ‘31/12/2020’) (Factura) |x| (Detalle |x| σ nombreP = ‘ProductoX’ (Producto))))



8. Listar nombre, apellido, DNI, teléfono y dirección de clientes que compraron el producto con nombre ‘Producto A’’ y no compraron el producto con nombre ‘Producto B’.
    
    π nombre, apellido, DNI, teléfono, dirección (Cliente |x| (Factura |x| (Detalle |x| σ nombreP = ‘Producto A’’ (Producto))))  - π nombre, apellido, DNI, teléfono, dirección  (Cliente |x| (Factura |x| (Detalle |x| σ nombreP = ‘Producto B’’ (Producto))))

9. Listar nroTicket, total, fecha, hora y DNI del cliente, de aquellas facturas donde se haya comprado el producto ‘Producto C’. 

    π nroTicket, total, fecha, hora, DNI (Cliente |x| (Factura |x| (Detalle |x| σ nombreP = ‘Producto C’ (Producto))))


10. Agregar un producto con id de producto 1000, descripción “mi producto”, precio $10000, nombreP “producto Z” y stock 1000. Se supone que el idProducto 1000 no existe. 
    
    π Producto ⇐ Producto U {(10000, “mi producto”, $10000, “producto Z”, 1000)}