[[Script ventas]]

## BLOQUE 1
#### Actividad 1

```
Muestra el nombre de todos los clientes en mayúsculas.
```

```mysql
SELECT UPPER(nombre)
FROM clientes;
```
#### Actividad 2

```
Muestra el nombre del cliente y su ciudad en minúsculas.
```

```mysql
SELECT LOWER(ciudad)
FROM clientes;
```

#### Actividad 3

```
Muestra el nombre completo de los empleados concatenando `nombre` y `apellido1`, separados por un espacio.
```

```mysql
SELECT CONCAT_WS(' ', nombre, apellido1)
FROM empleados;
```
#### Actividad 4

```
Muestra una única columna con la dirección completa de cada oficina, concatenando la `direccion`, `ciudad` y `pais`, separados por comas.
```

```mysql
SELECT CONCAT_WS(' ', direccion, ciudad, pais)
FROM oficinas;
```

#### Actividad 5

```
Muestra el nombre de la ciudad y sus tres primeras letras como abreviatura (en mayúsculas).
```

```mysql
SELECT ciudad, UPPER(LEFT(ciudad,3))
FROM oficinas;
```

#### Actividad 6

```
Muestra el nombre de cada producto y la longitud de su nombre.
```

```mysql
SELECT nombre, LENGTH(REPLACE(nombre,' ','') as longitud
FROM productos;
```

## BLOQUE 2
#### Actividad 7

```
Muestra el nombre del producto, su `precio` y el precio con IVA (21%), redondeado a dos decimales.
```

```mysql
SELECT nombre, precio, ROUND(precio*1.21, 2) AS precioIVA
FROM productos;
```
#### Actividad 8

```
Muestra el nombre del producto y su `precio` truncado o redondeado a 0 decimales.
```

```mysql
SELECT nombre, TRUNCATE(precio,0)
FROM productos;
```
#### Actividad 9

```
Muestra el nombre del producto y el resto de dividir su `stock` entre 10.
```

```mysql
SELECT nombre, MOD(stock,10)
FROM productos;
```
## BLOQUE 3
#### Actividad 10

```
Muestra el `id` del pedido y el nombre del mes en que se realizó la compra.
```

```mysql
SELECT id, MONTH(fecha)
FROM pedidos;
```
#### Actividad 11

```
Muestra el `id` del pedido, la `fecha` del pedido y la fecha actual del sistema.
```

```mysql
SELECT id, fecha, CURDATE()
FROM pedidos;
```
#### Actividad 12

```
Muestra el `id` del pedido y el año en que se realizó.
```

```mysql
SELECT id, YEAR(fecha)
FROM pedidos;
```
#### Actividad 13

```
Muestra el `id` y la `fecha` de los pedidos realizados en el mes de marzo (independientemente del año).
```

```mysql
SELECT id, fecha
FROM pedidos
WHERE MONTH(fecha)=3;
```
#### Actividad 14

```
Muestra la `fecha` del pedido en formato `DD/MM/AAAA`.
```

```mysql
SELECT DATE_FORMAT(fecha,'%d-%m-%Y')
FROM pedidos;
```
#### Actividad 15

```
Muestra el `id` del pedido y el número de días transcurridos desde la `fecha` del pedido hasta hoy.
```

```mysql
SELECT DATEDIFF(CURDATE(),fecha)
FROM pedidos;
```
#### Actividad 16

```
Calcula una fecha de entrega estimada sumando 7 días a la `fecha` del pedido.
```

```mysql
SELECT fecha, ADDDATE(fecha, 7) as fechaEstimada
FROM pedidos;
```
## BLOQUE 4
#### Actividad 17

```
Muestra el nombre del cliente y el `id` de su representante de ventas. Si no tiene representante (es NULL), debe aparecer el texto 'Sin asignar'.
```

```mysql
SELECT nombre, IFNULL(idEmpleado, 'sin asignar') 
FROM clientes;
```
#### Actividad 18

```
Muestra el nombre del cliente y su localización concatenando `ciudad` y `pais`. Asegúrate de que si algún campo es NULL, el resultado no sea nulo.
```

```mysql
SELECT nombre, CONCAT(IFNULL(ciudad,'sin asignar'), ' ', IFNULL(pais,'sin asignar')) as localizacion 
FROM clientes;
```