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
#### Actividad 15

```
Lista los empleados que atienden a clientes situados en países diferentes al país de la oficina donde el empleado trabaja.
```

```mysql
SELECT e.nombre
FROM empleados e 
INNER JOIN clientes c ON c.idEmpleado=e.id
INNER JOIN oficinas o ON o.id=e.idOficina
WHERE o.pais!=c.pais
```
#### Actividad 16

```
Muestra las gamas cuyo precio medio de producto es superior al precio medio de los productos de la gama 'Herramientas'.
```

```mysql
SELECT g.nombre
FROM gamas g 
INNER JOIN productos p ON g.id=p.idGama
HAVING AVG(p.precio)>(SELECT AVG(p1.precio) 
                      FROM productos p1
                      	INNER JOIN gamas g1 ON g1.id=p1.idGama
						
                      	WHERE g1.nombre='Herramientas'
                     	)
```
#### Actividad 17

```
Calcula el promedio de los importes totales de los pedidos (debes sumar cada pedido y luego promediar esos totales).
```

```mysql
SELECT AVG(aux.sumaProductos)
FROM (SELECT SUM(dp1.cantidad*dp1.precioUnidad) as sumaProductos 
    	FROM detallesPedidos dp1
		INNER JOIN pedidos p1 
    		ON  dp1.idPedido=p1.id
    	GROUP BY dp1.idPedido
		) aux
```
#### Actividad 18

```
Muestra los clientes que han comprado **todos** los productos existentes de la gama 'Aromáticas'.
```

```mysql
SELECT c.nombre
FROM clientes c 
INNER JOIN pedidos pe ON pe.idCliente=c.id
INNER JOIN detallesPedidos dp ON dp.idPedido=pe.id
INNER JOIN productos pr ON pr.id=dp.idProducto
INNER JOIN gamas g ON g.id=pr.idGama
WHERE g.nombre="Aromáticas"
GROUP BY c.nombre
HAVING COUNT(DISTINCT pr.id)=(SELECT COUNT(*)
                             FROM productos p1
                              INNER JOIN gamas g1 ON g1.id=p1.idGama 
                             WHERE g1.nombre="Aromáticas")
```
#### Actividad 19

```
Para cada oficina, calcula el porcentaje de sus clientes que tienen pedidos realizados pero ningún pago registrado hasta el momento.
```

```mysql
SELECT  
	o.id AS idOficina,  
	100 * SUM(CASE WHEN pa.idCliente IS NULL THEN 1 ELSE 0 END) /
	COUNT(DISTINCT c.id) AS porcentaje  
FROM oficinas o  
INNER JOIN empleados e ON e.idOficina = o.id  
INNER JOIN clientes c ON c.idEmpleado = e.id  
INNER JOIN pedidos p ON p.idCliente = c.id  
LEFT JOIN pagos pa ON pa.idCliente = c.id  
GROUP BY o.id;
```
#### Actividad 20

```
Lista los productos cuyo precio registrado en la tabla `detallesPedidos` ha sido, en alguna ocasión, inferior al 80% del precio actual de catálogo en la tabla `productos`.
```

```mysql
SELECT DISTINCT dp.idProducto
FROM detallesPedidos dp
INNER JOIN productos pr
ON pr.id=dp.idProducto
WHERE dp.precioUnidad<pr.precio*0.8
```
#### Actividad 21

```
Encuentra aquellos productos que se han vendido en pedidos diferentes con una variación de precio superior al 50% entre el precio más bajo y el más alto registrados en la tabla `detallesPedidos`.
```

```mysql
SELECT dp.idProducto
FROM detallesPedidos dp
GROUP BY dp.idProducto
HAVING MAX(dp.precioUnidad)-MIN(dp.precioUnidad)>MAX(dp.precioUnidad)*0.5
		AND COUNT(DISTINCT dp.idPedido) > 1
```
#### Actividad 22 DUDA SI QUITO G.id

```
Identifica si existe alguna gama de productos donde un solo cliente haya comprado más del 70% de todas las unidades vendidas de esa gama (en comparación con el total de unidades vendidas de dicha gama a todos los clientes).
```

```mysql
REVISAR: 
SELECT g.id
FROM gamas g
INNER JOIN productos pr ON pr.idGama=g.id
INNER JOIN detallesPedidos dp ON dp.idProducto=pr.id
INNER JOIN pedidos pe ON pe.id=dp.idPedido
INNER JOIN clientes c ON c.id=pe.idCliente
GROUP BY g.id,c.id
HAVING SUM(dp.cantidad)>(SELECT SUM(dp1.cantidad) 
                   FROM detallesPedidos dp1
                   INNER JOIN productos pr1 ON pr1.id=dp1.idProducto
                   WHERE pr1.idGama=g.id)*0.7;
                   
NO REVISAR:
FROM productos pr
INNER JOIN detallesPedidos dp ON dp.idProducto = pr.id
INNER JOIN pedidos pe ON pe.id = dp.idPedido
INNER JOIN clientes c ON c.id = pe.idCliente
GROUP BY pr.idGama, c.id
HAVING SUM(dp.cantidad) > (
    SELECT SUM(dp2.cantidad)
    FROM detallesPedidos dp2
    INNER JOIN productos pr2 ON pr2.id = dp2.idProducto
    WHERE pr2.idGama = pr.idGama
) * 0.7;

CUMPLIENDO ENUNCIADO:
SELECT 
    CASE 
        WHEN EXISTS (
            SELECT 1
            FROM productos pr
            INNER JOIN detallesPedidos dp ON dp.idProducto = pr.id
            INNER JOIN pedidos pe ON pe.id = dp.idPedido
            GROUP BY pr.idGama, pe.idCliente
            HAVING SUM(dp.cantidad) > (
                SELECT SUM(dp2.cantidad)
                FROM detallesPedidos dp2
                INNER JOIN productos pr2 ON pr2.id = dp2.idProducto
                WHERE pr2.idGama = pr.idGama
            ) * 0.7
        ) THEN 'Sí existe una gama con un cliente dominante'
        ELSE 'No existe ninguna gama que cumpla la condición'
    END AS Resultado;
```
#### Actividad 23 POR HACER

```
Muestra las oficinas que tienen menos de dos empleados con nivel 'Experto' para una gama específica, a pesar de que esa misma gama representa más del 40% de los pedidos totales de los clientes asociados a esa oficina.
```

```mysql

```
#### Actividad 24
```
 Encuentra el producto más vendido (en cantidad total de unidades) por cada país donde la empresa tiene clientes registrados. El resultado debe mostrar el País, el Nombre del Producto y la Cantidad Total.
```

```mysql
SELECT cl.pais, pr.nombre , SUM(dp.cantidad)  as VentasTotales
FROM productos pr
INNER JOIN detallespedidos dp ON dp.idProducto=pr.id
INNER JOIN pedidos pe ON pe.id=dp.idPedido
INNER JOIN clientes cl ON cl.id=pe.idCliente
GROUP BY cl.pais ,pr.id
HAVING MAX(VentasTotales)
ORDER BY SUM(dp.cantidad) DESC
```
#### Actividad 25
```
 Lista los productos que han tenido un balance negativo de unidades, asumiendo que los pedidos con estado 'Entregado' suman ventas y los pedidos 'Rechazados' restan (devoluciones).
```
```mysql
use ventas;
SELECT pr.id
FROM productos pr
INNER JOIN detallesPedidos dp ON dp.idProducto=pr.id
INNER JOIN pedidos pe ON pe.id=dp.idPedido

GROUP BY pr.id
HAVING SUM(CASE WHEN pe.estado="Entregado" THEN dp.cantidad
          		WHEN pe.estado="Rechazado" THEN -dp.cantidad
          ELSE 0
          END)<0
#### Actividad 27
```
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
