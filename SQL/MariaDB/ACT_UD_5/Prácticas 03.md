
[[Script ventas]]
## BLOQUE 1

#### Actividad 1

```
Muestra un listado de los pedidos en el que, además de la información básica, aparezca una descripción textual más comprensible del estado actual.

La descripción debe basarse exclusivamente en los estados que existan realmente en la tabla pedidos (ej: 'Entregado', 'Pendiente', 'Enviado', 'Rechazado') y contemplar también posibles valores no esperados.
```

```mysql
SELECT *,
	case estado
        WHEN 'Rechazado' THEN 'el pedido ha sido rechazado'
        WHEN 'Enviado' THEN 'el pedido ha sido enviado'
        ELSE 'el pedido  ha llegado a su destino'
    END AS estadoDesc
FROM pedidos;
```

#### Actividad 2

```
Muestra los clientes indicando, mediante un texto descriptivo, si tienen o no una persona responsable de sus ventas asignada (basándote en el campo idEmpleado).
```

```mysql
SELECT *,
	case estado
        WHEN 'Rechazado' THEN 'el pedido ha sido rechazado'
        WHEN 'Enviado' THEN 'el pedido ha sido enviado'
        ELSE 'el pedido  ha llegado a su destino'
    END AS estadoDesc
FROM pedidos;
```

## BLOQUE 2
#### Actividad 3

```
Clasifica los productos en distintas categorías comerciales en función de su `precio`.

Las categorías deben permitir distinguir claramente productos económicos (ej: < 20€), intermedios (20€ - 100€) y de gama alta (> 100€).
```

```mysql
SELECT precio,
			CASE
				WHEN precio<20 THEN 'económico'
            	WHEN precio BETWEEN 20 AND 100 THEN 'intermedio'
            	WHEN precio>100 THEN 'gama alta'
            	ELSE 'desconocido'
            END AS gama
FROM productos;
```

#### Actividad 4
```
Genera un listado de productos indicando el **estado operativo del stock**, diferenciando situaciones como ausencia de existencias (0), niveles críticos o bajos (< 20) y disponibilidad suficiente.
```

```mysql
SELECT nombre,
		CASE 
        	WHEN stock=0 THEN 'ausencia'
            WHEN stock<=20 THEN 'critico'
            WHEN stock>20 THEN 'suficiente'
            ELSE 'desconocido'
         END AS 'estado operativo'
FROM productos;
```
## BLOQUE 3
#### Actividad 5
```
Muestra los productos indicando su precio final tras aplicar el IVA (21%) y una **valoración textual** que permita identificar si el producto puede considerarse "Accesible" o "Inversión" según ese precio final.
```

```mysql
SELECT nombre,
		CASE 
        	WHEN precio*1.21<100 THEN 'Accesible'
            WHEN precio*1.21>=100 THEN 'Inversion'
            ELSE 'desconocido'
         END AS 'valoracion textual'
FROM productos;
```
## BLOQUE 4
#### Actividad 6
```
Muestra los pedidos ordenados según una **prioridad de atención lógica**, definida por el campo `estado` y no por su orden alfabético.

Considera que los pedidos 'Pendientes' tienen prioridad 1, los 'Enviados' prioridad 2 y los 'Entregados' o 'Rechazados' prioridad 3.
```

```mysql
SELECT *
FROM pedidos
ORDER BY CASE estado
			WHEN 'Pendiente' THEN 1
            WHEN 'Enviado' THEN 2
            WHEN 'Entregado' OR 'Rechazado' THEN 3
         	ELSE '4'
         END;
```
## BLOQUE 5
#### Actividad 7
```
Elabora una consulta que genere un informe mostrando cuántos pedidos existen de cada tipo de estado.

Este ejercicio requiere combinar lógica condicional con funciones de agregación y está pensado como un **desafío** para afianzar la comprensión avanzada de `CASE`.
```

```mysql
SELECT estado,
		count(*)
FROM pedidos
GROUP BY estado;
```

```mysql
SELECT 
	SUM(CASE WHEN estado='Enviado' THEN 1 ELSE 0 END )AS Enviado,
    SUM(CASE WHEN estado='Pendiente' THEN 1 ELSE 0 END )AS Pendiente,
    SUM(CASE WHEN estado='Entregado' THEN 1 ELSE 0 END )AS Entregado,
    SUM(CASE WHEN estado='Rechazado' THEN 1 ELSE 0 END )AS Rechazado
FROM pedidos;
```