[[Script ventas]]

## BLOQUE 1
#### Actividad 1

```
Muestra un listado de los pedidos (`id`, `fecha`, `estado`) junto con el nombre del cliente que los realizó.
```

```mysql
SELECT c.nombre,
		p.id,
        p.fecha,
        p.estado
FROM pedidos p 
INNER JOIN clientes c
	ON c.id=p.idCliente;
```
#### Actividad 2

```
Obtén un listado de clientes que tengan al menos un pedido asociado. _Nota:_ Al usar `INNER JOIN`, los clientes sin pedidos no aparecerán automáticamente.
```

```mysql
SELECT DISTINCT c.nombre
FROM clientes c 
INNER JOIN pedidos p
	ON c.id=p.idCliente;
```
#### Actividad 3

```
Muestra el nombre de cada producto y el nombre de la gama a la que pertenece.
```

```mysql
SELECT p.nombre,
		g.nombre
FROM productos p
INNER JOIN gamas g
	ON p.idGama=g.id
```
## BLOQUE 2
#### Actividad 4

```
Muestra todos los clientes y, si existen, el `id` de los pedidos asociados a cada uno. Los clientes sin pedidos deben aparecer con `NULL` en los datos del pedido.
```

```mysql
SELECT c.nombre,
		p.id
FROM clientes c
LEFT JOIN pedidos p
	ON c.id=p.idCliente
```
#### Actividad 5

```
Obtén únicamente los clientes que no han realizado ningún pedido.
```

```mysql
SELECT c.nombre
FROM clientes c
LEFT JOIN pedidos p
	 ON c.id=p.idCliente
     WHERE p.id IS NULL;
```
#### Actividad 6

```
Muestra el nombre de los empleados junto con la ciudad y el teléfono de la oficina en la que trabajan. Asegúrate de incluir empleados aunque su oficina no tuviera datos de contacto (si fuera el caso).
```

```mysql
SELECT e.nombre,
		o.ciudad,
        o.telefono
FROM empleados e
LEFT JOIN oficinas o
	 ON e.idOficina=o.id;
```
## BLOQUE 3
#### Actividad 7

```
Muestra los pedidos indicando el ID del pedido, el nombre del cliente y el nombre del empleado que actúa como su representante de ventas.
```

```mysql
SELECT p.id,
		c.nombre,
        e.nombre
FROM pedidos p
INNER JOIN clientes c
	 ON p.idCliente=c.id
INNER JOIN empleados e
		ON c.idEmpleado=e.id;
```
#### Actividad 8

```
Muestra el nombre del empleado, su puesto, y la ciudad y país de la oficina donde trabaja.
```

```mysql
SELECT e.nombre,
       e.puesto,
       o.ciudad,
       o.pais
FROM empleados e
INNER JOIN oficinas o
    ON e.idOficina = o.id;
```
#### Actividad 9

```
Muestra el ID del pedido, el nombre de los productos incluidos en él, la cantidad pedida y el precio unitario guardado en el detalle.
```

```mysql
SELECT pe.id,
       pr.nombre,
       dp.cantidad,
       dp.precioUnidad
FROM pedidos pe
INNER JOIN detallesPedidos dp
    ON pe.id=dp.idPedido
INNER JOIN productos pr
	ON pr.id=dp.idProducto;
```
## BLOQUE 4
#### Actividad 10

```
Muestra un listado de todos los pagos realizados, indicando la fecha, la cantidad y el nombre del cliente que realizó dicho pago.
```

```mysql
SELECT p.fecha,
		p.cantidad,
		c.nombre
FROM pagos p
LEFT JOIN clientes c 
    ON p.idCliente=c.id;
```
#### Actividad 11

```
Obtén un listado de los pagos, mostrando el nombre del cliente que pagó y el nombre del representante de ventas asignado a ese cliente.
```

```mysql
SELECT p.*,
		c.nombre,
		e.nombre
FROM pagos p
LEFT JOIN clientes c 
    ON p.idCliente=c.id
LEFT JOIN empleados e
	ON e.id=c.idEmpleado
```
## BLOQUE 5
#### Actividad 12

```
Muestra cuántos pedidos ha realizado cada cliente, del que se mostrará su id y su nombre.
```

```mysql
SELECT COUNT(p.idCliente) AS cantidadPedidos,
		c.id,
		c.nombre
FROM clientes c
LEFT JOIN  pedidos p
    ON p.idCliente=c.id
GROUP BY c.id,c.nombre;
```
#### Actividad 13

```
Obtén el importe total acumulado de los pagos realizados por cada cliente, del que se mostrará su id y su nombre.
```

```mysql
SELECT SUM(p.cantidad) AS cantidadTotalPagos,
		c.id,
		c.nombre
FROM clientes c
LEFT JOIN  pagos p
    ON p.idCliente=c.id
GROUP BY c.id, c.nombre;
```