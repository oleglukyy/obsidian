[[Script ventas]]

## BLOQUE 1

#### Actividad 1

```
Muestra el nombre y el precio de los productos cuyo precio sea superior al precio medio de todos los productos de la base de datos.
```

```mysql
SELECT nombre, precio
FROM productos
WHERE precio>(SELECT AVG(precio) FROM productos)
```

#### Actividad 2

```
Muestra todos los datos del pago cuyo importe (cantidad) sea igual al máximo importe registrado en la tabla de pagos.
```

```mysql
SELECT *
FROM pagos
WHERE cantidad = (SELECT MAX(cantidad) FROM PAGOS)
```

#### Actividad 3

```
Obtén el producto con el stock más alto de toda la tabla.
```

```mysql
USE ventas;
SELECT *
FROM productos
WHERE stock = (SELECT MAX(stock) FROM productos)
```

## BLOQUE 2

#### Actividad 4

```
Muestra los nombres de los clientes que aparecen en la tabla de pagos.
```

```mysql
SELECT DISTINCT c.nombre
FROM clientes c
	INNER JOIN pagos p
    ON c.id=p.idCliente;
```

**otra solucion**

```mysql
SELECT DISTINCT nombre
FROM clientes
WHERE id IN (SELECT idCliente FROM pagos)
```

#### Actividad 5

```
Obtén los nombres de los clientes que no han realizado ningún pago.
```

```mysql
SELECT DISTINCT nombre
FROM clientes
WHERE id NOT IN (SELECT idCliente FROM pagos)
```

## BLOQUE 3

#### Actividad 6

```
Muestra los productos cuyo precio sea superior al precio medio de su propia gama (idGama).
```

```mysql
SELECT nombre
FROM productos p
WHERE precio>(SELECT AVG(p.precio) from productos p1 where p1.idGama=p.idGama)
```

#### Actividad 7

```
Obtén los pagos cuyo importe sea superior a la media de pagos realizada por ese mismo cliente (idCliente).
```

```mysql
SELECT *
FROM pagos p1
WHERE cantidad>(SELECT AVG(p2.cantidad) FROM pagos p2 where p1.idCliente=p2.idCliente)
```

## BLOQUE 4

#### Actividad 8

```
Muestra los clientes para los que exista al menos un pedido asociado en la tabla pedidos.
```

```mysql
SELECT *
FROM clientes c
WHERE EXISTS(SELECT 1  from pedidos p where c.id=p.idCliente )
```

#### Actividad 9

```
Obtén los clientes para los que no exista ningún pedido asociado.
```

```mysql
SELECT *
FROM clientes c
WHERE NOT EXISTS(SELECT 1  from pedidos p where c.id=p.idCliente )
```