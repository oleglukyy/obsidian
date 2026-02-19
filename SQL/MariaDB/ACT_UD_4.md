[[mySQL]]
# UD 4
## BLOQUE 1

#### Actividad 1

```
Diagrama E/R (textual)

ENTIDAD Autor
- id
- nombre
- nacionalidad

ENTIDAD Libro
- id 
- titulo
- anioPublicacion

RELACIÓN Escribe
- Autor (1) —— (N) Libro
Tareas

No se evaluarán aún acciones referenciales ni restricciones avanzadas.
Crea la base de datos biblioteca_db.
Crea las tablas necesarias en SQL.
Implementa la relación 1:N de forma correcta (sin acciones referenciales aún).
```

```mysql
drop DATABASE if EXISTS biblioteca_db;
create DATABASE biblioteca_db;
use biblioteca_db;
create table autores(id INT, 
                     nombre varchar(50),
                     nacionalidad varchar(50),
                     PRIMARY KEY(id)
                    );
create table libros(id int,
                   titulo varchar(255),
                   anioPublicacion varchar(255),
                   PRIMARY KEY(id)
                   );
CREATE TABLE escribe(idAutor int NOT null,
                     idLibro int NOT null,
                     CONSTRAINT idAutor FOREIGN KEY(idAutor) REFERENCES autores(id), 
                     CONSTRAINT idLibro FOREIGN KEY(idLibro) REFERENCES libros(id)
                    );     
```

#### Actividad 2

```
Diagrama E/R (textual)

ENTIDAD Aula
- id
- nombre
- planta

ENTIDAD Curso
- id
- nombre

RELACIÓN SeImparteEn
- Aula (1) —— (N) Curso
Tareas

Crea la base de datos instituto_db.
Crea las tablas necesarias en SQL.
Implementa la relación 1:N.
```

```mysql
drop DATABASE if EXISTS instituto_db;
create DATABASE instituto_db;
use instituto_db;
create table aulas(id INT, 
                     nombre varchar(255),
                     planta INT,
                     PRIMARY KEY(id)
                    );
create table cursos(id int,
                    nombre varchar(255),
                    idAula INT,
                   	PRIMARY KEY(id),
                    CONSTRAINT idAula FOREIGN KEY idAula REFERENCES aulas(idAula)
                   );
```

## BLOQUE 2
#### Actividad 3

```
Diagrama E/R (textual)

ENTIDAD Departamento
- id
- nombre
- ciudad

ENTIDAD Empleado
- id
- dni (alternativo)
- nombre
- fechaNacimiento
- sueldo

RELACIÓN Pertenece
- Departamento (1) —— (N) Empleado
Regla: si se borra un departamento, los empleados quedan sin departamento.
Tareas

No es necesario definir acción para ON UPDATE.
Crea la base de datos rrhh_db.
Implementa PK y FK.
Define UNIQUE para dni.
Aplica la acción referencial ON DELETE SET NULL.
Nota técnica: Recuerda que para que funcione SET NULL, el campo de la clave ajena (en la tabla Empleado) debe admitir valores NULL (no puede ser NOT NULL).

```

```mysql
drop DATABASE if EXISTS rrhh_db;
create DATABASE rrhh_db;
use rrhh_db;
create table departamentos(id INT, 
                     nombre varchar(255),
                     ciudad varchar(255),
                     PRIMARY KEY(id)
                    );
create table empleados(id int,
                       dni char(9) UNIQUE,
                       nombre varchar(255),
                       fechaNacimiento DATE,
                       sueldo DECIMAL(10,2),
                       idDepartamento INT,
                       PRIMARY KEY(id) ,
                       CONSTRAINT idDepartamento FOREIGN KEY (idDepartamento) REFERENCES departamentos(id)
                       ON DELETE SET NULL
                   );
```

#### Actividad 4 

```
**Diagrama E/R (textual)**

`ENTIDAD Cliente - id - pasaporte (alternativo) - nombre  ENTIDAD Habitacion - id - numero - piso - precioNoche  RELACIÓN Reserva - Cliente (N) —— (M) Habitacion Atributos: fechaEntrada, noches Identificador: (idCliente, idHabitacion, fechaEntrada)`

**Tareas**

- Crea la base de datos `hotel_db`.
- Implementa la relación N:M con tabla intermedia.
- Aplica `UNIQUE`, `CHECK` y `DEFAULT`.
- Define acciones referenciales coherentes.
- Incluye un comentario SQL justificando brevemente cada acción ON DELETE.
```

```mysql
drop DATABASE if EXISTS hotel_db;
create DATABASE hotel_db;
use hotel_db;
create table clientes(id INT, 
                     pasaporte varchar(255) UNIQUE,
                     nombre varchar(255),
                     PRIMARY KEY(id)
                    );
create table habitaciones(id int,
                       numero int,
                       piso int,
                       precioNoche decimal check (precioNoche>0),
                       PRIMARY KEY(id)
                   );
create table reservas(idCliente int,
                      idHabitacion int,
                      fechaEntrada date default CURRENT_DATE,
                      noches int,
                      PRIMARY KEY(idCliente, idHabitacion, fechaEntrada),
                      CONSTRAINT idCliente FOREIGN key(idCliente) REFERENCES clientes(id)
                      ON DELETE SET NULL,
                      -- Establecemos set null para que en caso de que se de de baja el cliente,
                      -- sigamos teniendo el registro de la reserva pero no al cliente
                      CONSTRAINT idHabitacion FOREIGN key(idHabitacion) REFERENCES habitaciones(id)
                      );
```

#### Actividad 5

```
**Diagrama E/R (textual)**

`ENTIDAD Cliente - id (pk) - email (alternativo) - nombre - fechaAlta  ENTIDAD Direccion - id - calle - ciudad - cp  RELACIÓN Tiene - Cliente (1) —— (N) Direccion  ENTIDAD Producto - id - nombre - precio - stock  ENTIDAD Pedido - id - fechaPedido - estado  RELACIÓN Realiza - Cliente (1) —— (N) Pedido  RELACIÓN EnviaA - Direccion (1) —— (N) Pedido  RELACIÓN LineaPedido - Pedido (1) —— (N) LineaPedido - Producto (1) —— (N) LineaPedido Atributos: cantidad, precioUnitario Identificador: (idPedido, idProducto)`

**Tareas**

- Crea la base de datos `tienda_db`.
- Implementa todas las tablas necesarias.
- Aplica `UNIQUE`, `CHECK` y `DEFAULT` cuando proceda.
- Define claves ajenas y acciones referenciales coherentes.
```

```mysql
drop database if exists tienda_db;
create database tienda_db;
use tienda_db;
create table clientes(id int,
                    email varchar(255) unique,
                    nombre varchar(255),
                    fechaAlta date DEFAULT CURRENT_DATE,
                    PRIMARY KEY(id));
create table direcciones(id int,
                      calle varchar(255),
                      ciudad varchar(255),
                      cp char(5),
                      idCliente int,
                      PRIMARY KEY(id),
                      FOREIGN KEY(idCliente) REFERENCES clientes(id));
create  table productos(id int,
                      nombre varchar(255),
                      precio decimal(10,2),
                      stock int check(stock>0));
create table pedidos(id int,
                    fechaPedido date DEFAULT CURRENT_DATE,
                    estado varchar(255) default 'procesado',
                    idCliente int,
                    idDireccion int,
                    PRIMARY KEY(id),
                    FOREIGN KEY(idCliente) REFERENCES clientes(id),
                    FOREIGN KEY(idDireccion) REFERENCES direcciones(id));
create table lineasPedido(idPedido int,
                         idProducto int,
                         cantidad int check(cantidad>0),
                         precioUnitario DECIMAL(10,2) check(precioUnitario>0),
                         PRIMARY KEY(idPedido, idProducto),
                         FOREIGN KEY(idPedido) REFERENCES pedidos(id),
                         FOREIGN KEY(idProducto) REFERENCES productos(id));
```

## BLOQUE 3

```
Partiendo de la Actividad 5

**Tareas**

- Añade un nuevo atributo a una entidad.
- Cambia el nombre de un atributo existente.
- Cambia el tipo de un atributo.
- Añade una restricción nueva con `ALTER TABLE`.
- Añade una clave ajena que no se definió inicialmente.

```

```mysql
use tienda_db;
alter table clientes add column(contraseña varchar(255));

alter table clientes change contraseña contrasena varchar(255);

alter table clientes modify email varchar(50);

ALTER TABLE clientes ADD UNIQUE (id);


```