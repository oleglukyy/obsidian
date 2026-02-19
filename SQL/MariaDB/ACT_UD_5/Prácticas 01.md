[[Script ventas]]

## BLOQUE 1

#### Actividad 1

```
Inserta una nueva oficina en la tabla `oficinas` con los siguientes datos:

- `codigo`: 'ALM-ES'
- `ciudad`: 'Almería'
- `pais`: 'España'
- `cp`: '04001'
- `telefono`: '+34 950 11 22 33'
- `direccion`: 'C/ Paseo de Almería, 10'
```

```mysql
INSERT INTO oficinas (codigo, ciudad, pais, cp, telefono, direccion) 
VALUES ('ALM-ES','Almería','España','04001','+34 950 11 22 33', 'C/ Paseo de Almería, 10');
```

#### Actividad 2

```
Inserta un nuevo empleado en la tabla `empleados` con los siguientes datos:

- `nombre`: 'Laura'
- `apellido1`: 'Martínez'
- `email`: 'laura.martinez@v.es'
- `puesto`: 'Ven'
- `idOficina`: id de la oficina de Almería creada en el ejercicio anterior

Nota: Antes de realizar la inserción, realiza una consulta SELECT sobre la tabla oficinas para averiguar qué id se le ha asignado a la oficina de Almería.
```

```mysql
SELECT ciudad, codigo
FROM oficinas;

INSERT INTO empleados (nombre, apellido1, email, puesto, idOficina)
VALUES ('Laura','Martínez','laura.martinez@v.es','Ven','ALM-ES');
```

#### Actividad 3

```
Inserta una nueva gama en la tabla `gamas`:

- `nombre`: 'Automatizacion'
- `descripcion`: 'Sensores y sistemas de riego automático.'
```

```mysql
INSERT INTO gamas (nombre , descripcion)
VALUES ('Automatizacion','Sensores y sistemas de riego automático.')
```

## BLOQUE 2

#### Actividad 4

```
Muestra `codigo`, `ciudad`, `telefono` y `pais` de todas las oficinas.
```

```mysql
SELECT codigo, ciudad,telefono,pais 
FROM oficinas;
```

#### Actividad 5

```
Obtén un listado con `nombre`, `apellido1` y `email` de los empleados.

- Requisito: usa un alias para que `apellido1` aparezca como `apellido`.
```

```mysql 
SELECT nombre, apellido1 AS apellido, email 
FROM empleados;
```

#### Actividad 6

```
Muestra los distintos valores del campo `puesto` sin repeticiones.
```

```mysql
SELECT DISTINCT puesto
FROM empleados;
```
#### Actividad 7

```
Muestra `nombre` y `precio` de los productos ordenados por precio de mayor a menor.
```

```mysql
SELECT nombre, precio 
FROM productos 
ORDER BY precio DESC;
```
#### Actividad 8

```
Muestra `nombre` y `stock` de los tres productos con mayor stock.
```

```mysql
SELECT nombre, stock 
FROM productos
ORDER BY stock DESC
LIMIT 3;
```
#### Actividad 9

```
Muestra `nombre`, `precio` y el precio con IVA (21%).
```

```mysql
SELECT nombre, precio , precio*1.21  as precioIVA
FROM productos;
```

## BLOQUE 3

#### Actividad 10

```
Muestra `nombre` y `ciudad` de los clientes cuyo `pais` sea 'España'.
```

```mysql
SELECT nombre, ciudad 
FROM clientes
WHERE pais ='españa';
```

#### Actividad 11

```
Muestra `id` y `fecha` de los pedidos con estado 'Pendiente'.
```

```mysql
SELECT id,fecha
FROM pedidos 
WHERE estado='pendiente';
```

#### Actividad 12

```
Muestra `idCliente`, `forma` y `cantidad` de los pagos superiores a 100€.
```

```mysql
SELECT idCLiente, forma, cantidad
FROM pagos 
WHERE cantidad>100;
```

#### Actividad 13

```
Muestra todos los datos de los productos de la gama 'Ornamentales' (idGama = 2), ordenados por `stock` de mayor a menor.
```

```mysql

```

#### Actividad 14

```
Muestra `nombre`, `ciudad` y `pais` de los clientes que residan en Madrid o Barcelona.
```

```mysql
SELECT nombre, ciudad, pais 
FROM clientes 
WHERE ciudad IN ('Madrid','Barcelona');
```

#### Actividad 15

```
Muestra los productos de la gama 'Herramientas' (idGama = 1) con `precio` superior a 30€.
```

```mysql
SELECT *
FROM productos 
WHERE idGama=1 AND precio>30;
```

#### Actividad 16

```
Muestra `id`, `fecha` y `estado` de los pedidos realizados durante el año 2025.
```

```mysql
SELECT id, fecha, estado
FROM pedidos 
where YEAR(fecha)=2025;
```

#### Actividad 17

```
Muestra `nombre` y `telefono` de los clientes cuyo nombre empiece por 'J'.
```

```mysql
SELECT nombre, telefono
FROM clientes 
where nombre LIKE 'J%';
```

#### Actividad 18

```
Muestra `nombre` de los productos cuyo nombre contenga la palabra 'Tijera'.
```

```mysql
SELECT nombre
FROM productos 
where nombre LIKE 'Tijera%';
```

#### Actividad 19

```
Muestra `nombre` de los clientes sin representante de ventas asignado (`idEmpleado` es NULL).
```

```mysql
SELECT nombre
FROM clientes 
where idEmpleado IS NULL;
```

#### Actividad 20

```
Muestra los productos de la gama 'Aromaticas' (idGama = 3) con precio mayor que 3 o los de la gama 'Frutales' (idGama = 4) con precio mayor que 40.
```

```mysql
SELECT *
FROM productos 
where idGama=3 AND precio>3 
	OR idGama=4 AND precio>40;
```