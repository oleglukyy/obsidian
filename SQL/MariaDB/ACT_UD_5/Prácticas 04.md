[[Script ventas]]
## BLOQUE 1
#### Actividad 1

```
Obtén el número total de clientes registrados en la base de datos.
```

```mysql
SELECT count(*) as totalClientes
FROM clientes;
```
#### Actividad 2

```
 Muestra cuántos pedidos hay almacenados en la base de datos.
```

```mysql
SELECT count(*) as totalPedidos
FROM pedidos;
```
#### Actividad 3

```
Calcula el importe total de todos los pagos registrados en la tabla `pagos`.
```

```mysql
SELECT SUM(cantidad) as totalPagos
FROM pagos;
```
#### Actividad 4

```
Calcula el importe medio de los pagos realizados.
```

```mysql
SELECT AVG(cantidad) as mediaPagos
FROM pagos;
```
## BLOQUE 2
#### Actividad 5

```
Muestra cuántos pedidos hay en cada estado distinto (ej: 'Entregado', 'Pendiente').
```

```mysql
SELECT estado,
		COUNT(*) AS cantidad
FROM pedidos
GROUP BY estado;
```
#### Actividad 6

```
Obtén el número de clientes existentes en cada país.
```

```mysql
SELECT pais, 
		count(*) AS totalClientes 
FROM clientes
GROUP BY pais;
```
#### Actividad 7

```
Muestra cuántos productos pertenecen a cada identificador de gama (`idGama`).
```

```mysql
SELECT idGama, 
		count(*) AS cantidad 
FROM productos
GROUP BY idGama;
```
#### Actividad 8

```
Calcula la cantidad media de `stock` disponible para cada `idGama` de productos.
```

```mysql
SELECT idGama, 
		TRUNCATE(AVG(stock),0) AS mediaStock 
FROM productos
GROUP BY idGama;
```
#### Actividad 9

```
Calcula la cantidad media de `stock` disponible para cada `idGama` de productos.
```

```mysql
SELECT idGama, 
		TRUNCATE(AVG(stock),0) AS mediaStock 
FROM productos
GROUP BY idGama;
```

## BLOQUE 3
#### Actividad 9

```
Muestra el número de pedidos agrupados por año de realización (campo `fecha`) y por `estado`.
```

```mysql
SELECT count(*),
		YEAR(fecha), 
		estado
FROM pedidos
GROUP BY YEAR(fecha), estado;
```
#### Actividad 10

```
Obtén, para cada `idCliente`, el número total de pagos realizados.
```

```mysql
SELECT idCliente,
		COUNT(*)
FROM pagos
GROUP BY idCliente;
```
## BLOQUE 4
#### Actividad 11

```
Muestra únicamente los estados de pedido que tengan **más de 10 pedidos** asociados.
```

```mysql
SELECT estado,
		COUNT(*) AS total
FROM pedidos
GROUP BY estado
HAVING total>10;
```
#### Actividad 12

```
Obtén los identificadores de clientes (`idCliente`) que hayan pagado en total **más de 500€**.
```

```mysql
SELECT idCliente,
		SUM(cantidad) AS totalPagado
FROM pagos
GROUP BY idCliente
HAVING totalPagado>500;
```
## BLOQUE 5
#### Actividad 13

```
Calcula el valor total del stock de cada `idGama`, multiplicando el `precio` por el `stock` disponible.
```

```mysql
SELECT SUM(stock)*precio AS valorTotal,
		idGama
FROM productos
GROUP BY idGama;
```
#### Actividad 14

```
Obtén, para cada `idCliente`, la fecha de su pedido más reciente.
```

```mysql
SELECT idCliente,
		MAX(fecha)
FROM pedidos
GROUP BY idCliente;
```
