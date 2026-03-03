[[Prácticas 06 01]]
## BLOQUE 1

#### Actividad 1

```
Listar el nombre y el precio de los productos cuyo precio sea superior a la media de todos los productos del catálogo.
```

```mysql
SELECT nombre, precio
FROM productos
WHERE precio > (SELECT avg(precio) FROM productos)
```
#### Actividad 2

```
Mostrar todos los datos del pago cuya cantidad sea igual al máximo importe registrado en la tabla pagos.
```

```mysql
SELECT * 
FROM pagos 
WHERE cantidad = (SELECT MAX(cantidad) FROM pagos)
```
#### Actividad 3

```
Hallar los productos cuyo stock sea inferior al stock del producto con ID 5.
```

```mysql
SELECT *
FROM productos 
WHERE stock<(SELECT stock FROM productos WHERE id=5)
```
#### Actividad 4

```
Obtener los nombres de los clientes que han realizado algún pago exactamente igual al pago mínimo registrado.
```

```mysql
SELECT c.nombre 
FROM clientes c 
INNER JOIN pagos p 
ON p.idCliente=c.id 
WHERE p.cantidad = (SELECT MIN(cantidad) FROM pagos)
```
#### Actividad 5

```
Mostrar el nombre de las gamas que no tienen ningún producto con stock superior a 200 unidades (usando NOT IN).
```

```mysql
SELECT g.nombre
FROM gamas g
INNER JOIN productos p
	on g.id=p.idGama
WHERE p.id NOT IN(SELECT id from productos where stock>200)
```
#### Actividad 6

```
Seleccionar productos cuyo precio sea superior a cualquiera (ANY) de los precios de los productos de la gama 'Frutales'.
```

```mysql

```
#### Actividad 7

```
Listar los pagos cuya cantidad sea superior a todos (ALL) los pagos realizados por el cliente con ID 1.
```

```mysql
 
```
#### Actividad 8

```
Mostrar las ciudades de las oficinas que tienen empleados con el puesto 'Dir' (usando IN).
```

```mysql
SELECT DISTINCT o.ciudad 
FROM oficinas o
WHERE o.ciudad IN(SELECT o.ciudad FROM oficinas o
					INNER JOIN empleados e
					WHERE e.puesto = 'Dir')
```
#### Actividad 9

```
Mostrar los clientes que han realizado pagos superiores a la media de pagos de los clientes de su misma ciudad.
```

```mysql
SELECT nombre 
FROM clientes c
INNER JOIN pagos p
	on p.idCliente=c.id
WHERE p.cantidad>(SELECT AVG(cantidad) 
                  FROM pagos p1 
                  INNER JOIN clientes c1
                  ON p1.idCliente=c1.id
                  WHERE c1.ciudad=c.ciudad)
```
#### Actividad 10

```
Listar los productos cuyo precio sea superior a la media de precio de los productos de su propia gama.
```

```mysql
SELECT *
FROM productos p
WHERE precio>(SELECT AVG(precio)
              FROM productos p1
             WHERE p1.idGama=p.idGama);
```
#### Actividad 11

```
Mostrar el nombre de los clientes que han realizado más pedidos que la media global de pedidos por cliente.
```

```mysql
SELECT c.nombre 
FROM clientes c
INNER JOIN pedidos p 
	ON c.id=p.idCliente
GROUP BY c.id, c.nombre
HAVING COUNT(p.id)>(SELECT COUNT(*) FROM pedidos)/(SELECT COUNT(*) FROM clientes);
```
#### Actividad 12

```
Listar los empleados que tienen a su cargo clientes que no han realizado ningún pago (usando NOT EXISTS)
```

```mysql
SELECT e.nombre 
FROM empleados e
	INNER JOIN clientes c
    ON c.idEmpleado=e.id
WHERE NOT EXISTS(SELECT 1
                 FROM pagos p
                 WHERE c.id=p.idCliente);
```
