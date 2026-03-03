[[Script ventas]]
## Bloque 1

#### Actividad 1
```
Obtén un listado de todos los productos (nombre y precio) junto con el nombre de su gama.
```

```mysql
SELECT p.nombre,
	 p.precio,
	 g.nombre
FROM productos p
INNER JOIN gamas g 
	ON g.id=p.idGama;
```
#### Actividad 2
```
Muestra el nombre de los clientes que no han realizado ningún pago registrado hasta la fecha
```

```mysql
SELECT c.nombre
	 FROM clientes c
WHERE NOT EXISTS(SELECT 1 
				FROM pagos p 
				where c.id=p.idCliente);
```

```mysql
SELECT c.nombre
	 FROM clientes c
LEFT JOIN pagos p 
	ON p.idCliente IS NULL
```
#### Actividad 3
```
Calcula cuántos empleados trabajan en cada oficina, mostrando el código de la oficina y la ciudad.
```

```mysql
SELECT o.codigo,
		o.ciudad,
        (SELECT COUNT(id) FROM empleados e WHERE o.id=e.idOficina) as 	cantidad
FROM oficinas o;
```

```mysql
SELECT o.codigo,
		o.ciudad,
        COUNT(e.id)
FROM oficinas o
LEFT JOIN empleados e 
	ON e.idOficina=o.id
GROUP BY o.ciudad, o.codigo ;
```
#### Actividad 4
```
Muestra el nombre de los empleados que tienen nivel 'Experto' en la gama 'Frutales'.
```

```mysql
SELECT e.nombre
FROM empleados e
INNER JOIN empleadosgamas eg
	ON eg.idEmpleado=e.id
INNER JOIN gamas g
	ON g.id=eg.idGama
WHERE eg.nivel='Experto' AND g.nombre='Frutales';
```
#### Actividad 5
```
Muestra el nombre de los empleados que tienen nivel 'Experto' en la gama 'Frutales'.
```

```mysql
SELECT e.nombre
FROM empleados e
INNER JOIN empleadosgamas eg
	ON eg.idEmpleado=e.id
INNER JOIN gamas g
	ON g.id=eg.idGama
WHERE eg.nivel='Experto' AND g.nombre='Frutales';
```