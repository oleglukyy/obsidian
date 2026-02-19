
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