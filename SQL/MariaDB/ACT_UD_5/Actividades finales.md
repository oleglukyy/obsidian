[[Script ventas]]

## BLOQUE 1
#### Actividad 1

```
Obtén un listado de todos los productos (nombre y precio) junto con el nombre de su gama.
```

```mysql
SELECT p.nombre, p.precio,g.nombre
FROM productos p
INNER JOIN gamas g
	ON g.id=p.idGama
```
#### Actividad 2

```
Muestra el nombre de los clientes que no han realizado ningún pago registrado hasta la fecha.
```

```mysql
SELECT nombre
FROM clientes c
LEFT JOIN pagos p
	ON p.idCliente=c.id
WHERE p.idCliente is NULl
```
#### Actividad 3

```
Calcula cuántos empleados trabajan en cada oficina, mostrando el código de la oficina y la ciudad.
```

```mysql
SELECT o.codigo, 
	o.ciudad, 
    COUNT(e.id)
FROM oficinas o
inner join empleados e 
	On e.idOficina=o.id
GROUP BY o.codigo,o.ciudad
```
#### Actividad 4

```
Muestra el nombre de los empleados que tienen nivel 'Experto' en la gama 'Frutales'.
```

```mysql
SELECT e.nombre
FROM empleados e 
INNER JOIN empleadosGamas eg
	ON e.id=eg.idEmpleado
INNER JOIN gamas g
	ON g.id=eg.idGama
WHERE eg.nivel='Experto' AND g.nombre='Frutales'
```
#### Actividad 5

```
Obtén el ID de los pedidos realizados en el año 2025 que aún están en estado 'Pendiente'.
```

```mysql
SELECT p.id
FROM pedidos p
WHERE YEAR(p.fecha)=2025 AND p.estado='Pendiente'
```

## BLOQUE 2
#### Actividad 1 REHACER

```
Muestra el nombre de los clientes que han gastado en total más que la media de gasto de todos los clientes de la empresa.
```

```mysql
SELECT c.nombre
FROM clientes c
INNER JOIN pagos p
	ON c.id=p.idCliente
group BY c.nombre
HAVING SUM(p.cantidad)>(
    SELECT AVG(total)
    FROM (
        SELECT SUM(p2.cantidad) AS total
        FROM pagos p2
        GROUP BY p2.idCliente
    ) t);
```
#### Actividad 2

```
Lista aquellas gamas de productos donde **todos** sus productos tienen un stock actual inferior a la media de stock global de la empresa.
```

```mysql
SELECT g.nombre 
FROM gamas g
INNER JOIN productos p
	ON p.idGama=g.id
GROUP BY g.nombre
HAVING 	MAX(p.stock)
		<
		(SELECT AVG(stock) FROM productos)
```
#### Actividad 3

```
Calcula el ratio de "Clientes por Empleado" para cada oficina, mostrando la ciudad de la oficina y el número medio de clientes que atiende cada uno de sus trabajadores.
```

```mysql
SELECT o.codigo,
       o.ciudad,
       COUNT(c.id) / COUNT(DISTINCT e.id) AS promedio_clientes
FROM oficinas o
JOIN empleados e ON e.idOficina = o.id
LEFT JOIN clientes c ON c.idEmpleado = e.id
GROUP BY o.codigo, o.ciudad;
```
#### Actividad 4

```
Identifica los productos que se han vendido en todos los meses del año 2025 en los que hubo actividad comercial.
```

```mysql
SELECT pr.nombre,
       COUNT(DISTINCT MONTH(pe.fecha)) AS meses
FROM productos pr
JOIN detallesPedidos dp 
    ON pr.id = dp.idProducto
JOIN pedidos pe 
    ON dp.idPedido = pe.id
WHERE YEAR(pe.fecha) = 2025
GROUP BY pr.nombre
HAVING COUNT(DISTINCT MONTH(pe.fecha)) = 12;
```
#### Actividad 5

```
Muestra los empleados que son 'Expertos' en alguna gama pero que nunca han vendido un producto de esa misma gama.
```

```mysql
use ventas;

SELECT DISTINCT e.nombre 
FROM empleados e
INNER JOIN empleadosGamas eg
	ON eg.idEmpleado=e.id
WHERE eg.nivel='Experto' AND NOT EXISTS(
    SELECT 1
    FROM clientes c
    INNER JOIN pedidos pe 
        ON pe.idCliente = c.id
    INNER JOIN detallesPedidos dp 
        ON dp.idPedido = pe.id
    INNER JOIN productos p 
        ON p.id = dp.idProducto
    WHERE c.idEmpleado = e.id
      AND p.idGama = eg.idGama)

```
#### Actividad 6 NO HECHO

```
Para cada pedido, muestra qué porcentaje del importe total del pedido corresponde a cada producto incluido en él.
```

```mysql
SELECT pe.id, pr.nombre, pr.precio
FROM pedidos pe
INNER JOIN detallesPedidos dp
	ON pe.id=dp.idPedido
INNER JOIN productos pr 
	ON pr.id=dp.idProducto
ORDER BY pe.id
```
#### Actividad 7

```
Muestra las ciudades de los clientes ordenadas por el importe total de ventas generadas, incluyendo solo aquellas cuya suma supere los 2000€.
```

```mysql
SELECT c.ciudad , SUM(p.cantidad) AS total_ventas
FROM clientes c
INNER JOIN pagos p
	ON c.id=p.idCliente
GROUP BY c.ciudad
HAVING SUM(p.cantidad)>2000
ORDER BY total_ventas;
```
#### Actividad 8

```
Muestra el nombre de las gamas que no tienen ningún empleado con nivel 'Experto' asignado en ninguna oficina de España.
```

```mysql
SELECT DISTINCT g.nombre 
FROM gamas g 
INNER JOIN empleadosGamas dg
	ON dg.idGama=g.id
WHERE NOT EXISTS(SELECT 1
                      FROM empleados e1
                      INNER JOIN empleadosGamas eg
                      	ON eg.idEmpleado=e1.id
                 	INNER JOIN oficinas o 
                 		ON o.id=e1.idOficina
                      WHERE eg.nivel="Experto" AND eg.idGama=g.id AND o.pais="España")
```
#### Actividad 9

```
Muestra el ID de los clientes que realizaron más pedidos en el año 2025 que en el año 2026.
```

```mysql
SELECT c.id
FROM clientes c 
INNER JOIN pedidos p 
	ON p.idCliente=c.id
GROUP BY c.id
HAVING COUNT(CASE WHEN YEAR(p.fecha)=2025 THEN 1 END)<COUNT(CASE WHEN YEAR(p.fecha)=2026 THEN 1 END);
```
#### Actividad 10

```
Obtén el ID del pedido que contiene el mayor número de productos distintos pertenecientes a gamas diferentes.
```

```mysql
SELECT pe.id,COUNT(DISTINCT pr.idGama)
FROM pedidos pe
INNER JOIN detallesPedidos dp ON dp.idPedido=pe.id
INNER JOIN productos pr ON pr.id=dp.idProducto 
GROUP BY pe.id
ORDER BY COUNT(DISTINCT pr.idGama) DESC
LIMIT 1;
```
#### Actividad 11

```
Lista las oficinas que tienen al menos un empleado asignado para cada una de las 15 gamas existentes.
```

```mysql
SELECT o.id,COUNT(DISTINCT eg.idGama)
FROM oficinas o 
INNER JOIN empleados e ON e.idOficina=o.id
INNER JOIN empleadosGamas eg ON eg.idEmpleado=e.id
GROUP BY o.id
HAVING COUNT(DISTINCT eg.idGama)=15;
```
#### Actividad 12

```
Identifica a los clientes que siempre compran productos de la misma gama (sus pedidos nunca han contenido productos de gamas diferentes).
```

```mysql
FROM clientes c
INNER JOIN pedidos pe ON pe.idCliente=c.id
INNER JOIN detallesPedidos dp ON dp.idPedido=pe.id
INNER JOIN productos pr ON pr.id=dp.idProducto
GROUP BY c.id
HAVING COUNT(DISTINCT pr.idGama)=1;
```
```
#### Actividad 13

```
Muestra el nombre de los productos que representan más del 10% del stock total de su propia gama pero cuyas ventas suponen menos del 1% de las ventas totales de la empresa.
```

```mysql
POR HACER 
```
#### Actividad 14

```
Muestra el nombre del empleado cuyos clientes realizan pagos con la mayor frecuencia (menor tiempo medio entre pagos).
```

```mysql
SELECT e.nombre,AVG(DATEDIFF((SELECT p1.fecha
                            FROM pagos p1
                       		INNER JOIN clientes c1 ON c1.id=p1.idCliente
                            WHERE p1.fecha>p.fecha and c1.id=c.id 
                       		ORDER BY p1.fecha 
                            LIMIT 1
                            ),
                      p.fecha
                   )
          )
FROM empleados e
INNER JOIN clientes c 
	ON c.idEmpleado=e.id
INNER JOIN pagos p 
	ON p.idCliente=c.id
GROUP BY e.nombre
ORDER BY AVG(DATEDIFF((SELECT p1.fecha 
                            FROM pagos p1
                       		INNER JOIN clientes c1 ON c1.id=p1.idCliente
                            WHERE p1.fecha>p.fecha and c1.id=c.id 
                       		ORDER BY p1.fecha 
                            LIMIT 1 
                            ),
                      p.fecha
                   )
          );
```

#### Actividad 26

```
Identifica a los empleados que atienden a clientes que han comprado productos de una gama en la que el propio empleado NO posee ninguna especialidad técnica registrada en la tabla `empleadosGamas`.
```

```mysql
SELECT c.nombre,
		SUM(p.cantidad)-
        (SELECT AVG(p1.cantidad) FROM pagos p1 
		INNER JOIN clientes c1
		ON c1.id=p1.idCliente
		WHERE c1.ciudad=c.ciudad)
FROM clientes c
INNER JOIN pagos p 
	ON p.idCliente=c.id
GROUP BY c.nombre
```
#### Actividad 27

```
Para cada cliente, muestra su nombre y la diferencia entre su gasto total y la media de gasto de los clientes de su misma ciudad.
```

```mysql
SELECT c.nombre,
		SUM(p.cantidad)-
        (SELECT AVG(p1.cantidad) FROM pagos p1 
		INNER JOIN clientes c1
		ON c1.id=p1.idCliente
		WHERE c1.ciudad=c.ciudad)
FROM clientes c
INNER JOIN pagos p 
	ON p.idCliente=c.id
GROUP BY c.nombre
```
#### Actividad 28

```
Encuentra el ID del pedido que contiene productos de más del 50% de las gamas disponibles en la base de datos.
```

```mysql
SELECT DISTINCT idPedido
FROM detallesPedidos dp1
WHERE 
	(
    SELECT COUNT(DISTINCT id) 
	FROM gamas
	)*0.5
    <
    (
    SELECT COUNT(DISTINCT p.idGama)
	FROM productos p
	INNER JOIN detallesPedidos dp2
		ON p.id=dp2.idProducto
    WHERE dp1.idPedido=dp2.idPedido 
    )
    
SELECT dp.idPedido
FROM detallesPedidos dp
JOIN productos p ON dp.idProducto = p.id
GROUP BY dp.idPedido
HAVING COUNT(DISTINCT p.idGama) > (SELECT COUNT(*) FROM gamas) * 0.5;
```
#### Actividad 29

```
Identifica al grupo de clientes (20% del total de clientes) que generan el 80% de los ingresos por pagos de la empresa.
```

```mysql
SELECT nombre
FROM (
    SELECT 
        c.nombre,
        SUM(p.cantidad) AS total_cliente,
        SUM(SUM(p.cantidad)) OVER (ORDER BY SUM(p.cantidad) DESC) AS acumulado,
        (SELECT SUM(cantidad) FROM pagos) * 0.8 AS limite
    FROM clientes c
    JOIN pagos p ON p.idCliente = c.id
    GROUP BY c.id, c.nombre
) t
WHERE acumulado <= limite;
```
#### Actividad 30

```
Calcula una comisión para cada empleado basándose en: 5% de las ventas de productos en los que es 'Experto' y solo un 2% en productos de gamas donde no tiene especialidad.
```

```mysql
SELECT e.nombre,
	SUM(
        CASE 
            WHEN eg.nivel='Experto' AND pr.idGama=eg.idGama THEN 0.05*dp.precioUnidad*dp.cantidad
        	ELSE 0.02*dp.precioUnidad*dp.cantidad
        END
    ) as ventas
				
FROM empleados e
INNER JOIN  empleadosGamas eg
    ON e.id=eg.idEmpleado
INNER JOIN clientes c
    ON c.idEmpleado=e.id 
INNER JOIN pedidos pe
    On c.id=pe.idCliente
INNER JOIN detallesPedidos dp
	ON dp.idPedido=pe.id
INNER JOIN productos pr
	ON pr.id=dp.idProducto
GROUP BY e.nombre;
```
