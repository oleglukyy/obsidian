[[mySQL]]
-- ======================================================
-- SCRIPT CORREGIDO: VENTAS (UD5)
-- ======================================================

DROP DATABASE IF EXISTS ventas;
CREATE DATABASE ventas CHARACTER SET utf8mb4 COLLATE utf8mb4_spanish_ci;
USE ventas;

-- ======================================================
-- 1. ESTRUCTURA DE TABLAS (ENTIDADES)
-- ======================================================

CREATE TABLE oficinas (
  id INT AUTO_INCREMENT PRIMARY KEY,
  codigo VARCHAR(10) NOT NULL UNIQUE,
  ciudad VARCHAR(60) NOT NULL,
  pais VARCHAR(60) NOT NULL,
  cp VARCHAR(10),
  telefono VARCHAR(30),
  direccion VARCHAR(120)
);

CREATE TABLE empleados (
  id INT AUTO_INCREMENT PRIMARY KEY,
  nombre VARCHAR(50) NOT NULL,
  apellido1 VARCHAR(50) NOT NULL,
  apellido2 VARCHAR(50),
  email VARCHAR(100) NOT NULL UNIQUE,
  puesto VARCHAR(60),
  idOficina INT NOT NULL,
  FOREIGN KEY (idOficina) REFERENCES oficinas(id)
);

CREATE TABLE clientes (
  id INT AUTO_INCREMENT PRIMARY KEY,
  nombre VARCHAR(80) NOT NULL,
  contacto VARCHAR(80),
  telefono VARCHAR(30),
  ciudad VARCHAR(60),
  pais VARCHAR(60),
  idEmpleado INT,
  FOREIGN KEY (idEmpleado) REFERENCES empleados(id)
);

CREATE TABLE gamas (
  id INT AUTO_INCREMENT PRIMARY KEY,
  nombre VARCHAR(60) NOT NULL UNIQUE,
  descripcion VARCHAR(255)
);

CREATE TABLE productos (
  id INT AUTO_INCREMENT PRIMARY KEY,
  nombre VARCHAR(120) NOT NULL,
  precio DECIMAL(10,2) NOT NULL,
  stock INT NOT NULL DEFAULT 0,
  idGama INT NOT NULL,
  FOREIGN KEY (idGama) REFERENCES gamas(id)
);

CREATE TABLE pedidos (
  id INT AUTO_INCREMENT PRIMARY KEY,
  fecha DATE NOT NULL,
  estado VARCHAR(30) NOT NULL,
  idCliente INT NOT NULL,
  FOREIGN KEY (idCliente) REFERENCES clientes(id)
);

CREATE TABLE pagos (
  id INT AUTO_INCREMENT PRIMARY KEY,
  forma VARCHAR(40) NOT NULL,
  cantidad DECIMAL(12,2) NOT NULL,
  fecha DATE NOT NULL,
  idCliente INT NOT NULL,
  FOREIGN KEY (idCliente) REFERENCES clientes(id)
);

-- ======================================================
-- 2. ESTRUCTURA DE TABLAS (RELACIONES N:M)
-- ======================================================

CREATE TABLE detallesPedidos (
  idPedido INT NOT NULL,
  idProducto INT NOT NULL,
  cantidad INT NOT NULL,
  precioUnidad DECIMAL(10,2) NOT NULL,
  numeroLinea SMALLINT NOT NULL,
  PRIMARY KEY (idPedido, idProducto),
  FOREIGN KEY (idPedido) REFERENCES pedidos(id),
  FOREIGN KEY (idProducto) REFERENCES productos(id)
);

CREATE TABLE empleadosGamas (
  idEmpleado INT NOT NULL,
  idGama INT NOT NULL,
  nivel VARCHAR(20) DEFAULT 'Básico',
  PRIMARY KEY (idEmpleado, idGama),
  FOREIGN KEY (idEmpleado) REFERENCES empleados(id),
  FOREIGN KEY (idGama) REFERENCES gamas(id)
);

-- ======================================================
-- 3. CARGA DE DATOS 
-- ======================================================

INSERT INTO oficinas (codigo, ciudad, pais, cp, telefono, direccion) VALUES
('MAD-01','Madrid','España','28001','911','Gran Vía 1'), ('BCN-01','Barcelona','España','08001','931','Pelai 12'),
('SEV-01','Sevilla','España','41001','954','Constitución 3'), ('VAL-01','Valencia','España','46001','963','Colón 5'),
('MAL-01','Málaga','España','29001','952','Larios 2'), ('BIL-01','Bilbao','España','48001','944','Gran Vía 8'),
('ZAR-01','Zaragoza','España','50001','976','Alfonso 10'), ('MUR-01','Murcia','España','30001','968','Escultor 5'),
('PAL-01','Palma','España','07001','971','Mallorca 2'), ('LPA-01','Las Palmas','España','35001','928','Triana 15'),
('SCT-01','Tenerife','España','38001','922','Castillo 20'), ('COR-01','Córdoba','España','14001','957','Cruz Conde 4'),
('ALI-01','Alicante','España','03001','965','Maisonnave 1'), ('VAD-01','Valladolid','España','47001','983','Santiago 8'),
('VIG-01','Vigo','España','36201','986','Príncipe 12'), ('GIJ-01','Gijón','España','33201','985','Corrida 5'),
('GRA-01','Granada','España','18001','958','Reyes Católicos'), ('ELC-01','Elche','España','03201','966','Corredora 1'),
('OVI-01','Oviedo','España','33001','984','Uría 10'), ('BAD-01','Badalona','España','08901','934','Mar 3'),
('MAD-02','Madrid','España','28005','912','Paseo Castellana 40'), 
('BCN-02','Barcelona','España','08010','932','Av. Diagonal 100'),
('SEV-02','Sevilla','España','41010','955','C/ Betis 14'),
('VAL-02','Valencia','España','46010','964','Av. del Cid 22'),
('BIL-02','Bilbao','España','48005','945','Alameda Mazarredo'),
('MAD-03','Madrid','España','28015','913','C/ Princesa 5'),
('BCN-03','Barcelona','España','08020','933','Via Laietana 8'),
('MAL-02','Málaga','España','29010','953','Av. Andalucía 50'),
('MUR-02','Murcia','España','30010','969','C/ Platería 2'),
('SEV-03','Sevilla','España','41005','956','C/ Sierpes 1');

INSERT INTO empleados (nombre, apellido1, apellido2, email, puesto, idOficina) VALUES
('Ana', 'García', 'Ruiz', 'ana.garcia@v.es', 'Dir', 1),
('Javier', 'López', 'Sanz', 'javier.lopez@v.es', 'Ven', 1),
('Marta', 'Sánchez', 'Gómez', 'marta.sanchez@v.es', 'Ven', 2),
('Luis', 'Pérez', 'Murillo', 'luis.perez@v.es', 'Ven', 3),
('Elena', 'Gómez', 'Vila', 'elena.gomez@v.es', 'Ven', 4),
('Raúl', 'Ruiz', 'Cano', 'raul.ruiz@v.es', 'Ven', 5),
('Sofía', 'Marín', 'Díaz', 'sofia.marin@v.es', 'Ven', 6),
('Diego', 'Sanz', 'Torres', 'diego.sanz@v.es', 'Ven', 7),
('Paula', 'Cano', 'Blanco', 'paula.cano@v.es', 'Ven', 8),
('Mario', 'Gil', 'Ramos', 'mario.gil@v.es', 'Ven', 9),
('Clara', 'Díaz', 'Castro', 'clara.diaz@v.es', 'Ven', 10),
('Hugo', 'Ramos', 'Ortiz', 'hugo.ramos@v.es', 'Ven', 11),
('Sara', 'Vila', 'Medina', 'sara.vila@v.es', 'Ven', 12),
('Iker', 'Blanco', 'Moya', 'iker.blanco@v.es', 'Ven', 13),
('Nora', 'Torres', 'Soler', 'nora.torres@v.es', 'Ven', 14),
('Leo', 'Castro', 'Marín', 'leo.castro@v.es', 'Ven', 15),
('Alba', 'Ortiz', 'Pérez', 'alba.ortiz@v.es', 'Ven', 16),
('Pau', 'Rubio', 'García', 'pau.rubio@v.es', 'Ven', 17),
('Eva', 'Medina', 'López', 'eva.medina@v.es', 'Ven', 18),
('Marc', 'Moya', 'Sánchez', 'marc.moya@v.es', 'Ven', 19),
('Julia', 'Soler', 'Ruiz', 'julia.soler@v.es', 'Ven', 20),
('Antonio', 'Jiménez', 'Marín', 'antonio.jim@v.es', 'Ven', 21),
('Isabel', 'Vázquez', NULL, 'isabel.vaz@v.es', 'Ven', 21),
('Pedro', 'Heredia', 'Sanz', 'pedro.her@v.es', 'Ven', 22),
('Lucía', 'Garrido', 'Vila', 'lucia.gar@v.es', 'Ven', 22),
('Manuel', 'Ortega', 'Gil', 'manuel.ort@v.es', 'Ven', 23),
('Rosa', 'Lorenzo', NULL, 'rosa.lor@v.es', 'Ven', 23),
('Alberto', 'Navarro', 'Torres', 'alberto.nav@v.es', 'Ven', 24),
('Carmen', 'Romero', 'Díaz', 'carmen.rom@v.es', 'Ven', 25),
('Fernando', 'Cano', 'Ruiz', 'fernando.can@v.es', 'Ven', 26),
('Beatriz', 'Méndez', 'Moya', 'beatriz.men@v.es', 'Ven', 26),
('Santiago', 'Rojas', NULL, 'santiago.roj@v.es', 'Ven', 27),
('Patricia', 'Soto', 'Gómez', 'patricia.sot@v.es', 'Ven', 27),
('Iván', 'Guerrero', 'López', 'ivan.gue@v.es', 'Ven', 28),
('Mónica', 'Peña', 'Sánchez', 'monica.pen@v.es', 'Ven', 29),
('Joaquín', 'Domínguez', 'Vila', 'joaquin.dom@v.es', 'Ven', 30),
('Esther', 'Morales', NULL, 'esther.mor@v.es', 'Ven', 30),
('Daniel', 'Prieto', 'Castro', 'daniel.pri@v.es', 'Ven', 1),
('Verónica', 'Hidalgo', 'Ortiz', 'veronica.hid@v.es', 'Ven', 1),
('Ángel', 'Herrero', 'Blanco', 'angel.her@v.es', 'Ven', 2),
('Lorena', 'Gallardo', 'Medina', 'lorena.gal@v.es', 'Ven', 2),
('Ricardo', 'Pascual', 'Ramos', 'ricardo.pas@v.es', 'Ven', 3),
('Silvia', 'Bernal', NULL, 'silvia.ber@v.es', 'Ven', 3),
('Óscar', 'León', 'Díaz', 'oscar.leo@v.es', 'Ven', 4),
('Bárbara', 'Vargas', 'Moya', 'barbara.var@v.es', 'Ven', 5),
('Felipe', 'Franco', 'Ruiz', 'felipe.fra@v.es', 'Ven', 6),
('Inés', 'Crespo', 'Gómez', 'ines.cre@v.es', 'Ven', 7),
('Adrián', 'Esteban', 'Sanz', 'adrian.est@v.es', 'Ven', 8),
('Cristina', 'Ibáñez', 'Vila', 'cristina.iba@v.es', 'Ven', 9),
('Marcos', 'Arias', NULL, 'marcos.ari@v.es', 'Ven', 10),
('Raquel', 'Mora', 'Gil', 'raquel.mor@v.es', 'Ven', 11);

INSERT INTO clientes (nombre, contacto, telefono, ciudad, pais, idEmpleado) VALUES
('EcoShop', 'Juan P.', '9551', 'Sevilla', 'España', 4), ('TechCorp', 'John S.', '9132', 'Madrid', 'España', 2),
('BCN Data', 'Jordi S.', '9323', 'Barcelona', 'España', 3), ('Global Inc', 'José T.', '9504', 'Almería', 'España', NULL),
('BioMarket', 'Lucía R.', '9635', 'Valencia', 'España', 5), ('NetFlow', 'David M.', '9526', 'Málaga', 'España', 6),
('SteelInc', 'Ane L.', '9447', 'Bilbao', 'España', 7), ('AgroZar', 'Pedro S.', '9768', 'Zaragoza', 'España', 8),
('Levante', 'Mamen C.', '9689', 'Murcia', 'España', 9), ('Island', 'Toni V.', '9710', 'Palma', 'España', 10),
('Canary', 'Yeray R.', '9281', 'Las Palmas', 'España', 11), ('Atlantic', 'Guaci T.', '9222', 'Tenerife', 'España', 12),
('South Garden', 'Carmen V.', '9573', 'Córdoba', 'España', 13), ('MediSun', 'Paco B.', '9654', 'Alicante', 'España', 14),
('Castilla', 'Lola D.', '9835', 'Valladolid', 'España', 15), ('Galicia Mar', 'Santi C.', '9866', 'Vigo', 'España', 16),
('Norte Flor', 'Pelayo O.', '9857', 'Gijón', 'España', 17), ('Sierra Nevada', 'Fátima R.', '9588', 'Granada', 'España', 18),
('Palmeras Elche', 'Asun M.', '9669', 'Elche', 'España', 19), ('Capital G', 'Antón M.', '9840', 'Oviedo', 'España', 20),
('Gran Vía Shop', 'Luis G.', '9111', 'Madrid', 'España', 2), ('Rambla Flor', 'Marta P.', '9333', 'Barcelona', 'España', 3),
('Triana Verde', 'Rocío L.', '9555', 'Sevilla', 'España', 4), ('Turia Plant', 'Marc V.', '9666', 'Valencia', 'España', 5),
('Castellana 2', 'Andrés S.', '9112', 'Madrid', 'España', 21), ('Diagonal B', 'Carla M.', '9334', 'Barcelona', 'España', 22),
('Betis Garden', 'Hugo J.', '9556', 'Sevilla', 'España', 23), ('Giralda Flor', 'Sara F.', '9557', 'Sevilla', 'España', 30),
('Alfonso X', 'Felipe D.', '9777', 'Zaragoza', 'España', 8), ('Larios Mal', 'Pilar G.', '9558', 'Málaga', 'España', 6),
('French Garden', 'Jean L.', '331', 'París', 'Francia', 2), ('London Bloom', 'Mark T.', '442', 'Londres', 'Reino Unido', 3),
('Roma Verde', 'Giulia M.', '393', 'Roma', 'Italia', 5), ('Berlin Plant', 'Hans S.', '494', 'Berlín', 'Alemania', 7),
('Lisboa Flor', 'Joao P.', '351', 'Lisboa', 'Portugal', NULL), ('Porto Sun', 'Ana F.', '352', 'Oporto', 'Portugal', 10),
('Marseille Garden', 'Luc P.', '332', 'Marsella', 'Francia', NULL), ('Munich Green', 'Otto K.', '495', 'Munich', 'Alemania', 12),
('Milano Bio', 'Enzo R.', '394', 'Milán', 'Italia', 14), ('Napoli Sun', 'Sofia B.', '395', 'Nápoles', 'Italia', 16),
('Viveros Sol', 'Elena B.', '101', 'Madrid', 'España', 26), ('Pueblo Nuevo', 'Raúl F.', '102', 'Madrid', 'España', 37),
('Jardín Real', 'Sofía C.', '103', 'Madrid', 'España', 38), ('Verde BCN', 'Marc T.', '104', 'Barcelona', 'España', 27),
('Sants Garden', 'Laia O.', '105', 'Barcelona', 'España', 39), ('Gracia Flor', 'Pau R.', '106', 'Barcelona', 'España', 40),
('Sevilla Este', 'Lola S.', '107', 'Sevilla', 'España', 35), ('Nervión Plant', 'Juan M.', '108', 'Sevilla', 'España', 36),
('Macarena Flor', 'Asun P.', '109', 'Sevilla', 'España', 13), ('Cartuja Verde', 'Luis R.', '110', 'Sevilla', 'España', 30),
('Vigo Centro', 'Eva M.', '111', 'Vigo', 'España', 16), ('Gijón Mar', 'Nora T.', '112', 'Gijón', 'España', 17),
('Oviedo Antiguo', 'Iker B.', '113', 'Oviedo', 'España', 49), ('Murcia Sur', 'Mamen D.', '114', 'Murcia', 'España', 48),
('Valencia Port', 'Marc M.', '115', 'Valencia', 'España', 43), ('Bilbao Viejo', 'Ane G.', '116', 'Bilbao', 'España', 46),
('Granada Albaicín', 'Fátima Z.', '117', 'Granada', 'España', 18), ('Elche Palmeral', 'Paco E.', '118', 'Elche', 'España', 19),
('Alicante Puerto', 'Lola A.', '119', 'Alicante', 'España', 14), ('Badalona Mar', 'Santi B.', '120', 'Badalona', 'España', 20),
('Toledo Imperial', 'Carlos V.', '121', 'Toledo', 'España', NULL), ('Segovia Flor', 'Isabel C.', '122', 'Segovia', 'España', 41),
('Avila Garden', 'Teresa A.', '123', 'Ávila', 'España', 42), ('Burgos Green', 'Cid R.', '124', 'Burgos', 'España', 43),
('León Bio', 'Guzmán B.', '125', 'León', 'España', 44), ('Salamanca Uni', 'Fray L.', '126', 'Salamanca', 'España', 45),
('Cáceres Old', 'Pizarro F.', '127', 'Cáceres', 'España', 46), ('Badajoz Border', 'Vasco N.', '128', 'Badajoz', 'España', 47),
('Huelva Luz', 'Colón C.', '129', 'Huelva', 'España', 48), ('Cádiz Playa', 'Falla M.', '130', 'Cádiz', 'España', 49),
('Almería Desierto', 'Bisbal D.', '131', 'Almería', 'España', 50), ('Jaén Olivar', 'Machado A.', '132', 'Jaén', 'España', 31),
('Córdoba Flor', 'Góngora L.', '133', 'Córdoba', 'España', 32), ('Málaga Sun', 'Picasso P.', '134', 'Málaga', 'España', 33),
('Huesca Pirineos', 'Arnal M.', '135', 'Huesca', 'España', 34), ('Teruel Amantes', 'Diego I.', '136', 'Teruel', 'España', 35),
('Cuenca Casas', 'Julián C.', '137', 'Cuenca', 'España', 36), ('Guadalajara Alcarria', 'Camilo J.', '138', 'Guadalajara', 'España', 37),
('Albacete Navaja', 'Andrés I.', '139', 'Albacete', 'España', 38), ('Ciudad Real Quijote', 'Cervantes M.', '140', 'Ciudad Real', 'España', 39),
('Santander Bahía', 'Severiano B.', '141', 'Santander', 'España', 40), ('Logroño Rioja', 'Espartero B.', '142', 'Logroño', 'España', 41),
('Pamplona SanFermin', 'Hemingway E.', '143', 'Pamplona', 'España', 42), ('Vitoria Green', 'Celedón P.', '144', 'Vitoria', 'España', 43),
('San Sebastian Concha', 'Chillida E.', '145', 'San Sebastián', 'España', 44), ('Soria Machado', 'Leonor I.', '146', 'Soria', 'España', 45),
('Zamora Románica', 'Viriato L.', '147', 'Zamora', 'España', 46), ('Palencia Románico', 'Jorge M.', '148', 'Palencia', 'España', 47),
('Pontevedra Lerez', 'Valle I.', '149', 'Pontevedra', 'España', 48), ('Lugo Muralla', 'Rosalía C.', '150', 'Lugo', 'España', 49);

INSERT INTO gamas (nombre, descripcion) VALUES
('Herramientas', 'Equipos manuales y mecánicos para el mantenimiento.'),
('Ornamentales', 'Plantas y flores destinadas a la decoración de exteriores.'),
('Aromáticas', 'Variedad de plantas con propiedades culinarias o fragancias.'),
('Frutales', 'Árboles y arbustos productores de frutas comestibles.'),
('Césped', 'Diferentes variedades de semillas y tepes para cobertura.'),
('Riego', 'Sistemas, mangueras y programadores para el control de agua.'),
('Macetas', 'Recipientes de diversos materiales y tamaños para cultivo.'),
('Sustratos', 'Tierras y medios de cultivo preparados para el crecimiento.'),
('Abonos', 'Fertilizantes orgánicos y químicos para nutrición vegetal.'),
('Plagas', 'Productos destinados al control de insectos y enfermedades.'),
('Mobiliario', 'Elementos de decoración y confort para zonas exteriores.'),
('Iluminación', 'Soluciones lumínicas para jardín y zonas de paso.'),
('Estanques', 'Bombas, filtros y accesorios para entornos acuáticos.'),
('Huerto', 'Semillas y plantones de verduras y hortalizas.'),
('Cactus', 'Colección de plantas suculentas y xerófitas.');

INSERT INTO productos (nombre, precio, stock, idGama) VALUES
('Tijera Poda A1', 15.50, 120, 1), ('Pala Cuadrada', 22.00, 45, 1), ('Rastrillo Metálico', 18.25, 60, 1),
('Azada Reforzada', 25.00, 30, 1), ('Hacha de Mano', 28.50, 15, 1), ('Serrucho Curvo', 19.90, 40, 1),
('Escoba de Jardín', 12.00, 85, 1), ('Tijera Cortasetos', 35.00, 20, 1), ('Transplantador', 8.50, 150, 1),
('Cultivador Manual', 10.00, 90, 1), ('Horca de Cava', 32.00, 12, 1), ('Maza de Goma', 14.50, 25, 1),
('Cortafrio Profesional', 9.95, 100, 1), ('Palote Forjado', 27.00, 18, 1),
('Rosa Roja Classic', 5.50, 250, 2), ('Margarita Blanca', 3.20, 400, 2), ('Tulipán Holandés', 4.50, 300, 2),
('Orquídea Phalaenopsis', 18.00, 50, 2), ('Clavel del Poeta', 2.80, 500, 2), ('Hortensia Azul', 12.50, 80, 2),
('Geranio Común', 4.00, 220, 2), ('Pensamiento Variado', 1.50, 600, 2), ('Petunia Colgante', 3.50, 180, 2),
('Begonia de Flor', 5.25, 140, 2), ('Crisantemo Blanco', 6.00, 90, 2), ('Azalea Rosa', 14.00, 35, 2),
('Gardenia Aromática', 16.50, 25, 2), ('Camelia Japónica', 22.00, 15, 2),
('Menta Poleo', 2.50, 150, 3), ('Albahaca Genovesa', 2.50, 150, 3), ('Romero Común', 3.00, 120, 3),
('Tomillo Limonero', 2.80, 130, 3), ('Lavanda Officinalis', 4.50, 90, 3), ('Perejil Rizado', 1.80, 200, 3),
('Cebollino Fino', 2.20, 110, 3), ('Orégano Silvestre', 3.20, 85, 3), ('Salvia Común', 3.80, 70, 3),
('Cilantro Fresco', 2.00, 140, 3), ('Eneldo Fino', 2.50, 60, 3), ('Hierbabuena Dulce', 2.70, 160, 3),
('Laurel en Maceta', 8.50, 40, 3), ('Melisa Limón', 3.50, 50, 3),
('Manzano Golden', 45.00, 25, 4), ('Peral Conferencia', 42.00, 30, 4), ('Limonero 4 Estaciones', 38.00, 40, 4),
('Naranjo Navel', 40.00, 35, 4), ('Olivo Centenario', 150.00, 5, 4), ('Cerezo Picota', 48.00, 15, 4),
('Ciruelo Claudia', 35.00, 20, 4), ('Melocotonero Rojo', 39.00, 18, 4), ('Higuera Común', 32.00, 22, 4),
('Albaricoquero', 41.00, 12, 4), ('Granado Dulce', 36.00, 28, 4), ('Vid Moscatel', 15.00, 100, 4),
('Kaki Persimon', 44.00, 10, 4), ('Nispero Japonés', 37.00, 14, 4),
('Semilla Césped Inglés 1kg', 12.50, 200, 5), ('Césped Rústico 5kg', 45.00, 50, 5), ('Repoblador Rápido 1kg', 14.00, 80, 5),
('Tepes Natural m2', 9.50, 500, 5), ('Césped de Sombra 1kg', 15.00, 65, 5), ('Grama Fina 1kg', 18.00, 40, 5),
('Abono Césped Primavera', 22.00, 100, 5), ('Antimusgo Césped', 16.50, 75, 5), ('Escarificador Manual', 28.00, 15, 5),
('Semilla Trébol Blanco', 8.00, 120, 5), ('Césped Deportivo 5kg', 48.00, 30, 5), ('Abono Otoño 2kg', 12.00, 90, 5),
('Rodillo Compactador', 55.00, 8, 5), ('Semilla Dicondra', 24.00, 20, 5),
('Difusor Emergente', 8.50, 150, 6), ('Aspersor Circular', 12.00, 80, 6), ('Manguera Reforzada 20m', 25.50, 60, 6),
('Programador Digital', 45.00, 40, 6), ('Kit Riego Goteo', 32.00, 55, 6), ('Electroválvula 9V', 28.00, 35, 6),
('Conector Rápido', 2.50, 500, 6), ('Pistola 7 Posiciones', 14.00, 90, 6), ('Gotero Autocompensante', 0.50, 2000, 6),
('Arqueta Riego', 18.00, 30, 6), ('Sensor de Lluvia', 22.50, 25, 6), ('Carro Portamanguera', 38.00, 20, 6),
('Reductor de Presión', 15.00, 45, 6), ('Tubería Polietileno 50m', 42.00, 15, 6),
('Maceta Barro 20cm', 4.50, 300, 7), ('Maceta Barro 40cm', 18.00, 120, 7), ('Jardinera Plástico 60cm', 12.50, 150, 7),
('Macetero Resina Gris', 35.00, 40, 7), ('Plato Barro 20cm', 1.50, 400, 7), ('Soporte Metálico Pared', 8.00, 90, 7),
('Maceta Colgante Blanca', 6.50, 180, 7), ('Cubremaceta Mimbre', 14.00, 60, 7), ('Hidrojardinera Rectangular', 55.00, 15, 7),
('Maceta Piedra Natural', 85.00, 8, 7), ('Pack 10 Macetas Semillero', 3.00, 250, 7), ('Macetero Alto Fibra', 42.00, 22, 7),
('Bowl Cerámica Azul', 28.00, 35, 7), ('Mini Maceta Cactus', 1.20, 500, 7),
('Sustrato Universal 50L', 12.50, 400, 8), ('Sustrato Orquídeas 5L', 6.50, 120, 8), ('Sustrato Cactus 5L', 5.80, 150, 8),
('Fibra de Coco 10L', 9.00, 200, 8), ('Mantillo Orgánico 20L', 8.50, 300, 8), ('Tierra de Castaño 10L', 7.20, 80, 8),
('Perlita Saco 5L', 5.00, 100, 8), ('Vermiculita 5L', 6.50, 70, 8), ('Turba Rubia 20L', 11.00, 130, 8),
('Sustrato Bonsái 2L', 4.50, 90, 8), ('Corteza Pino Decorativa', 10.00, 250, 8), ('Arcilla Expandida 5L', 7.50, 140, 8),
('Humus de Lombriz 10L', 9.50, 180, 8), ('Sustrato Semilleros 10L', 5.50, 160, 8),
('Abono Líquido Universal 1L', 8.50, 200, 9), ('Abono Granulado Azul 5kg', 18.00, 120, 9), ('Varitas Fertilizantes Pack', 4.50, 300, 9),
('Abono Guano Natural 1kg', 12.00, 150, 9), ('Revitalizante Orquídeas', 6.20, 80, 9), ('Quelato de Hierro 100g', 7.50, 100, 9),
('Abono Floración Líquido', 9.90, 140, 9), ('Abono Ácidos Cítricos', 11.00, 60, 9), ('Bioestimulante Algas', 14.50, 50, 9),
('Abono Rosales 1kg', 7.80, 110, 9), ('Corrector Carencias 250ml', 13.00, 45, 9), ('Abono Clavos Pack 20', 5.50, 180, 9),
('Salitre Potásico 1kg', 10.50, 70, 9), ('Sulfato Amónico 5kg', 15.00, 40, 9),
('Insecticida Polivalente', 11.50, 150, 10), ('Fungicida Sistémico', 14.00, 120, 10), ('Acaricida Concentrado', 12.80, 80, 10),
('Trampa Avispas', 9.50, 200, 10), ('Raticida Cebo Fresco', 8.00, 300, 10), ('Aceite de Neem 100ml', 10.50, 90, 10),
('Jabón Potásico 500ml', 7.50, 140, 10), ('Antilimacos Gránulos', 6.80, 180, 10), ('Insecticida Hormigas', 5.50, 250, 10),
('Fungicida Cobre 500g', 13.00, 70, 10), ('Azufre Micronizado 1kg', 9.20, 100, 10), ('Trampa Polilla Tomate', 15.50, 40, 10),
('Gel Cucarachas', 12.00, 110, 10), ('Repelente Perros/Gatos', 16.00, 55, 10),
('Silla Madera Teka', 85.00, 40, 11), ('Mesa Redonda Exterior', 145.00, 15, 11), ('Banco Forja Clásico', 190.00, 10, 11),
('Tumbona Resina Blanca', 65.00, 30, 11), ('Sombrilla Lateral 3m', 120.00, 12, 11), ('Conjunto Ratán 4p', 450.00, 5, 11),
('Balancín Jardín 3 plazas', 280.00, 8, 11), ('Pérgola Metal 3x3', 220.00, 10, 11), ('Cojín Tumbona Rayas', 25.00, 60, 11),
('Funda Protectora Mesa', 18.00, 45, 11), ('Arcón Resina 300L', 95.00, 20, 11), ('Hamaca Algodón', 45.00, 25, 11),
('Sofá Exterior 2 plazas', 320.00, 6, 11), ('Mesita Auxiliar Cristal', 55.00, 14, 11),
('Foco Solar LED 10W', 32.00, 80, 12), ('Guirnalda Exterior 10m', 25.00, 120, 12), ('Farola Jardín 2m', 110.00, 15, 12),
('Aplique Pared Moderno', 42.00, 40, 12), ('Baliza Solar Pack 4', 18.00, 200, 12), ('Proyector RGB Mando', 55.00, 25, 12),
('Farolillo Portátil LED', 14.00, 95, 12), ('Antorcha Efecto Fuego', 12.50, 150, 12), ('Sobremuro Clásico', 38.00, 35, 12),
('Tira LED 5m IP65', 29.90, 70, 12), ('Lámpara Flotante Piscina', 45.00, 30, 12), ('Estaca LED Negra', 9.50, 180, 12),
('Sensor Movimiento Ext', 22.00, 50, 12), ('Transformador 12V 60W', 35.00, 20, 12),
('Bomba Estanque 2000L/h', 85.00, 20, 13), ('Filtro Presión UV 10k', 165.00, 12, 13), ('Lona Estanque 4x5m', 120.00, 15, 13),
('Kit Oxigenador Solar', 45.00, 25, 13), ('Red Recogehojas Fina', 12.00, 60, 13), ('Foco Sumergible LED', 38.00, 35, 13),
('Cascada Acero Inox', 140.00, 8, 13), ('Pack 10 Nenúfares Mix', 35.00, 40, 13), ('Comida Peces Estanque', 9.50, 100, 13),
('Antialgas Estanque 1L', 18.00, 45, 13), ('Skimmer de Superficie', 75.00, 10, 13), ('Termómetro Flotante', 8.00, 30, 13),
('Balsa Preformada 150L', 95.00, 5, 13), ('Red Protectora Aves', 15.00, 50, 13),
('Semilla Tomate Cherry', 1.50, 500, 14), ('Semilla Lechuga Romana', 1.20, 600, 14), ('Semilla Pimiento Padrón', 1.80, 400, 14),
('Semilla Zanahoria 10g', 2.00, 350, 14), ('Plantón Berenjena', 0.80, 200, 14), ('Semilla Calabacín', 2.50, 180, 14),
('Invernadero Túnel 3m', 35.00, 25, 14), ('Malla Antipájaros 4x5m', 8.50, 90, 14), ('Tutor Caña Pack 10', 12.00, 150, 14),
('Semilla Pepino', 2.10, 220, 14), ('Plantón Cebolla Pack 50', 6.00, 100, 14), ('Semilla Espinaca', 1.90, 300, 14),
('Minihuerto Urbano Madera', 75.00, 12, 14), ('Kit Semilleros Bio', 14.50, 80, 14),
('Cactus Asiento Suegra G', 35.00, 20, 15), ('Aloe Vera Medicinal', 12.50, 80, 15), ('Cactus Cebra Mini', 3.50, 150, 15),
('Echeveria Elegans', 4.80, 120, 15), ('Chumbera sin Espinas', 15.00, 45, 15), ('Cactus de Navidad', 9.00, 70, 15),
('Piedras Vivas Pack 3', 18.00, 55, 15), ('Cactus Columna 50cm', 42.00, 15, 15), ('Euphorbia Cactus', 28.00, 25, 15),
('Crápula Ovata', 14.00, 60, 15), ('Cactus Bola Oro', 8.50, 90, 15), ('Sansevieria Trifas', 19.00, 40, 15),
('Pack 6 Suculentas Mix', 22.00, 50, 15), ('Cactus San Pedro', 55.00, 10, 15);

INSERT INTO pedidos (fecha, estado, idCliente) VALUES
('2025-01-10', 'Entregado', 1), ('2025-01-12', 'Entregado', 2), 
('2025-01-15', 'Rechazado', 3), ('2025-01-18', 'Entregado', 5),
('2025-01-20', 'Entregado', 10), ('2025-01-22', 'Entregado', 15), 
('2025-01-25', 'Entregado', 21), ('2025-01-28', 'Entregado', 22),
('2025-02-01', 'Entregado', 30), ('2025-02-03', 'Entregado', 41), 
('2025-02-05', 'Entregado', 50), ('2025-02-08', 'Entregado', 12),
('2025-02-10', 'Entregado', 25), ('2025-02-12', 'Entregado', 33),
('2025-02-15', 'Entregado', 44), ('2025-02-18', 'Entregado', 5), 
('2025-03-01', 'Entregado', 1),  ('2025-03-05', 'Entregado', 12), 
('2025-03-10', 'Entregado', 23), ('2025-03-12', 'Entregado', 34),
('2025-03-15', 'Entregado', 45), ('2025-03-18', 'Entregado', 6),
('2025-03-20', 'Entregado', 17), ('2025-03-22', 'Entregado', 28),
('2025-03-25', 'Entregado', 39), ('2025-04-01', 'Entregado', 2),
('2025-04-05', 'Rechazado', 13), ('2025-04-10', 'Entregado', 24),
('2025-04-15', 'Entregado', 35), ('2025-04-20', 'Entregado', 46),
('2025-04-25', 'Entregado', 7),  ('2025-05-01', 'Entregado', 18),
('2025-05-05', 'Entregado', 29), ('2025-05-10', 'Entregado', 40),
('2025-05-15', 'Entregado', 41), ('2025-05-20', 'Entregado', 3),
('2025-05-25', 'Entregado', 14), ('2025-06-01', 'Entregado', 25),
('2025-06-05', 'Entregado', 36), ('2025-06-10', 'Entregado', 47),
('2025-06-15', 'Entregado', 8),  ('2025-06-20', 'Entregado', 19),
('2025-06-25', 'Entregado', 31), ('2025-06-28', 'Entregado', 42),
('2025-06-30', 'Entregado', 2),  ('2025-06-01', 'Entregado', 4),
('2025-06-05', 'Entregado', 16), ('2025-06-10', 'Entregado', 27),
('2025-06-15', 'Entregado', 38), ('2025-06-20', 'Entregado', 49),
('2025-07-01', 'Entregado', 11), ('2025-07-05', 'Entregado', 22),
('2025-07-10', 'Entregado', 33), ('2025-07-15', 'Entregado', 44),
('2025-07-20', 'Entregado', 5),  ('2025-07-25', 'Entregado', 17), 
('2025-08-01', 'Entregado', 28), ('2025-08-05', 'Entregado', 39),
('2025-08-10', 'Entregado', 41), ('2025-08-15', 'Entregado', 12),
('2025-08-20', 'Entregado', 23), ('2025-08-25', 'Entregado', 34),
('2025-09-01', 'Entregado', 45), ('2025-09-05', 'Entregado', 6),
('2025-09-10', 'Entregado', 18), ('2025-09-15', 'Entregado', 29),
('2025-09-20', 'Entregado', 40), ('2025-09-25', 'Entregado', 12),
('2025-09-28', 'Entregado', 23), ('2025-09-30', 'Entregado', 34),
('2025-09-01', 'Entregado', 45), ('2025-09-05', 'Entregado', 16),
('2025-09-10', 'Entregado', 7),  ('2025-09-15', 'Entregado', 19),
('2025-09-20', 'Entregado', 31), ('2025-10-01', 'Entregado', 42),
('2025-10-05', 'Entregado', 13), ('2025-10-10', 'Entregado', 24),
('2025-10-15', 'Entregado', 35), ('2025-10-20', 'Entregado', 46),
('2025-10-25', 'Entregado', 7),  ('2025-11-01', 'Entregado', 8),
('2025-11-05', 'Entregado', 20), ('2025-11-10', 'Entregado', 32),
('2025-11-15', 'Entregado', 43), ('2025-11-20', 'Entregado', 4),
('2025-11-25', 'Entregado', 15), ('2025-12-01', 'Entregado', 26),
('2025-12-05', 'Entregado', 37), ('2025-12-10', 'Entregado', 48),
('2025-12-15', 'Entregado', 9),  ('2025-12-20', 'Entregado', 33), 
('2026-01-05', 'Enviado', 44),   ('2026-01-10', 'Enviado', 45),
('2026-01-12', 'Pendiente', 16), ('2026-01-14', 'Pendiente', 27),
('2026-01-15', 'Pendiente', 38), ('2026-01-18', 'Pendiente', 49),
('2026-01-20', 'Pendiente', 50), ('2026-01-21', 'Pendiente', 1);

INSERT INTO detallesPedidos (idPedido, idProducto, cantidad, precioUnidad, numeroLinea) VALUES
(1, 1, 2, 15.50, 1), (1, 15, 5, 5.50, 2), (1, 29, 1, 2.50, 3), (1, 43, 1, 45.00, 4), (1, 57, 10, 12.50, 5), (1, 71, 3, 8.50, 6), (1, 85, 2, 4.50, 7), (1, 99, 1, 12.50, 8), (1, 113, 5, 8.50, 9), (1, 127, 2, 11.50, 10),
(2, 5, 1, 2.50, 1), 
(3, 10, 10, 8.50, 1),
(4, 50, 2, 48.00, 1),
(5, 100, 1, 5.80, 1),
(6, 150, 3, 18.00, 1),
(7, 200, 1, 4.80, 1),
(8, 2, 1, 22.00, 1), (8, 16, 4, 3.20, 2),
(9, 3, 1, 5.00, 1), (9, 17, 1, 4.50, 2), (9, 31, 2, 3.00, 3),
(10, 4, 2, 25.00, 1), (10, 18, 5, 18.00, 2), (10, 32, 1, 2.80, 3),
(11, 20, 2, 12.50, 1), (11, 40, 1, 8.50, 2), (11, 60, 3, 9.50, 3), (11, 80, 1, 12.00, 4), (11, 110, 2, 7.50, 5),
(12, 12, 1, 14.50, 1), (12, 22, 1, 18.00, 2), (12, 32, 1, 2.80, 3), (12, 42, 1, 3.50, 4), (12, 52, 1, 39.00, 5),
(13, 150, 2, 18.00, 1), (14, 151, 1, 120.00, 1), (15, 152, 1, 190.00, 1), (16, 153, 2, 25.00, 1),
(17, 154, 1, 18.00, 1), (18, 155, 3, 32.00, 1), (19, 156, 1, 25.00, 1), (20, 157, 4, 110.00, 1),
(21, 50, 1, 48.00, 1), (21, 51, 1, 35.00, 2), (22, 52, 1, 39.00, 1), (22, 53, 1, 32.00, 2),
(23, 54, 2, 41.00, 1), (23, 55, 1, 36.00, 2), (24, 56, 1, 15.00, 1), (24, 57, 2, 12.50, 2),
(25, 58, 1, 45.00, 1), (25, 59, 1, 14.00, 2), (26, 60, 3, 9.50, 1), (26, 61, 1, 15.00, 2),
(30, 146, 1, 450.00, 1), (30, 143, 1, 190.00, 2),
(31, 1, 2, 15.50, 1), (32, 2, 1, 22.00, 1), (33, 3, 5, 5.00, 1), (34, 4, 2, 25.00, 1),
(35, 80, 1, 12.00, 1), (35, 81, 2, 22.50, 2), (35, 82, 1, 38.00, 3),
(36, 90, 10, 8.00, 1), (37, 91, 5, 1.50, 1), (38, 92, 1, 35.00, 1),
(39, 120, 1, 5.50, 1), (39, 121, 2, 13.00, 2), (39, 122, 1, 5.50, 3), (39, 123, 1, 10.50, 4),
(40, 130, 1, 16.00, 1), (41, 131, 2, 15.50, 1), (42, 132, 1, 14.50, 1), (43, 1, 1, 15.50, 1),
(44, 10, 2, 8.50, 1), (45, 20, 5, 12.50, 1), (46, 30, 1, 2.50, 1), (47, 40, 3, 8.50, 1),
(48, 50, 1, 48.00, 1), (49, 60, 2, 9.50, 1),
(50, 70, 1, 55.00, 1), (50, 2, 1, 22.00, 2), (50, 3, 2, 5.00, 3),
(51, 1, 3, 15.50, 1), (51, 10, 1, 8.50, 2), (51, 11, 1, 35.00, 3),
(52, 1, 1, 15.50, 1), (52, 25, 2, 14.00, 2), (52, 26, 1, 22.00, 3),
(53, 1, 2, 15.50, 1), (53, 40, 1, 8.50, 2), (53, 41, 1, 32.00, 3),
(54, 15, 10, 5.50, 1), (54, 60, 2, 9.50, 2), (54, 61, 1, 15.00, 3),
(55, 15, 2, 5.50, 1), (55, 80, 1, 12.00, 2), (55, 81, 1, 22.50, 3),
(56, 15, 5, 5.50, 1), (56, 100, 3, 5.80, 2), (56, 101, 1, 9.00, 3),
(57, 15, 1, 5.50, 1), (57, 120, 1, 5.50, 2), (57, 121, 1, 13.00, 3),
(58, 15, 3, 5.50, 1), (58, 140, 1, 16.00, 2), (58, 141, 1, 85.00, 3),
(59, 160, 2, 29.90, 2), (59, 161, 1, 45.00, 3),
(60, 100, 1, 5.80, 1), (60, 101, 1, 9.00, 2), (60, 102, 1, 8.50, 3),
(61, 103, 1, 9.00, 1), (62, 104, 1, 11.00, 1), (63, 105, 1, 5.00, 1), (64, 106, 1, 6.50, 1),
(65, 107, 1, 11.00, 1), (66, 108, 1, 4.50, 1), (67, 109, 1, 10.00, 1), (68, 110, 1, 7.50, 1),
(69, 111, 1, 9.50, 1), 
(70, 112, 1, 5.50, 1), (70, 1, 2, 15.50, 2), (70, 2, 1, 22.00, 3), (70, 3, 1, 5.00, 4), (70, 4, 1, 25.00, 5), (70, 5, 1, 2.50, 6),
(71, 113, 1, 8.50, 1), (71, 10, 1, 8.50, 2), (71, 20, 1, 12.50, 3), (71, 30, 1, 2.50, 4), (71, 40, 1, 8.50, 5), (71, 50, 1, 48.00, 6),
(72, 114, 1, 18.00, 1), (72, 100, 2, 5.80, 2), (72, 110, 1, 7.50, 3), (72, 120, 3, 5.50, 4), (72, 130, 1, 16.00, 5), (72, 140, 1, 16.00, 6),
(73, 115, 1, 4.50, 1), (73, 150, 1, 18.00, 2), (73, 160, 2, 29.90, 3), (73, 170, 1, 38.00, 4), (73, 180, 1, 85.00, 5), (73, 190, 1, 8.50, 6),
(74, 200, 2, 9.00, 1), (74, 210, 1, 55.00, 2), (74, 15, 5, 5.50, 3), (74, 35, 2, 4.50, 4), (74, 55, 1, 36.00, 5),
(75, 10, 2, 8.50, 1), (75, 25, 1, 14.00, 2), (75, 50, 1, 48.00, 3), (75, 75, 3, 12.00, 4), (75, 100, 2, 5.80, 5), (75, 125, 1, 11.50, 6),
(80, 5, 10, 2.50, 1), (80, 15, 20, 5.50, 2), (80, 45, 1, 38.00, 3), (80, 95, 2, 55.00, 4), (80, 155, 1, 32.00, 5),
(81, 12, 1, 14.50, 1), (82, 33, 5, 2.80, 1), (83, 54, 2, 41.00, 1), (84, 76, 1, 25.50, 1),
(85, 98, 10, 1.20, 1), (86, 110, 2, 7.50, 1), (87, 132, 1, 14.50, 1), (88, 154, 1, 18.00, 1),
(89, 176, 3, 8.00, 1),
(90, 190, 2, 8.50, 1), (90, 191, 1, 12.00, 2), (90, 198, 2, 12.50, 3),
(91, 192, 1, 2.10, 1), (91, 2, 1, 22.00, 2),
(92, 193, 2, 75.00, 1), (92, 22, 1, 18.00, 2),
(93, 194, 5, 14.50, 1), (93, 42, 1, 3.50, 2),
(94, 195, 1, 35.00, 1), (94, 62, 5, 18.00, 2),
(95, 196, 1, 12.50, 1), (95, 82, 2, 38.00, 2),
(96, 197, 2, 3.50, 1), (96, 102, 1, 8.50, 2),
(97, 198, 1, 4.80, 1), (97, 122, 1, 5.50, 2),
(98, 199, 1, 15.00, 1), (98, 142, 1, 145.00, 2),
(100, 200, 1, 9.00, 1), (100, 201, 1, 18.00, 2), (100, 202, 1, 42.00, 3), (100, 203, 1, 28.00, 4), (100, 204, 1, 14.00, 5), (100, 205, 1, 8.50, 6), (100, 206, 1, 19.00, 7), (100, 207, 1, 22.00, 8),
(100, 1, 1, 15.50, 9), (100, 50, 1, 48.00, 10), (100, 100, 2, 5.80, 11), (100, 150, 1, 18.00, 12), (100, 210, 5, 55.00, 13), (100, 25, 2, 14.00, 14), (100, 75, 3, 12.00, 15);

INSERT INTO pagos (forma, cantidad, fecha, idCliente) VALUES
('Transferencia', 150.50, '2025-01-15', 1), 
('Tarjeta', 45.00, '2025-01-20', 2), 
('Transferencia', 200.00, '2025-01-25', 5), 
('Tarjeta', 120.00, '2025-02-05', 10),
('PayPal', 85.00, '2025-02-15', 15), 
('Transferencia', 300.00, '2025-03-05', 21),
('Tarjeta', 50.00, '2025-03-15', 22), 
('PayPal', 12.50, '2025-03-20', 30),
('Transferencia', 400.00, '2025-04-05', 1), 
('Tarjeta', 65.00, '2025-04-15', 2),
('Transferencia', 95.00, '2025-04-20', 23), 
('Tarjeta', 150.00, '2025-04-25', 34),
('PayPal', 220.00, '2025-05-05', 45), 
('Transferencia', 35.00, '2025-05-15', 50),
('Tarjeta', 80.00, '2025-05-20', 6),   -- Corregido (antes 56)
('PayPal', 110.00, '2025-06-05', 7),  -- Corregido (antes 57)
('Transferencia', 45.00, '2025-06-15', 10), -- Corregido (antes 60)
('Tarjeta', 300.00, '2025-06-25', 17), -- Corregido (antes 67)
('PayPal', 150.00, '2025-07-05', 18),  -- Corregido (antes 68)
('Transferencia', 90.00, '2025-07-15', 20), -- Corregido (antes 70)
('Tarjeta', 130.00, '2025-07-25', 28), -- Corregido (antes 78)
('PayPal', 250.00, '2025-08-05', 29),  -- Corregido (antes 79)
('Transferencia', 60.00, '2025-08-15', 30), -- Corregido (antes 80)
('Tarjeta', 40.00, '2025-08-25', 31),  -- Corregido (antes 81)
('PayPal', 500.00, '2025-09-05', 32),  -- Corregido (antes 82)
('Transferencia', 75.00, '2025-09-15', 33), -- Corregido (antes 83)
('Tarjeta', 1000.00, '2025-09-25', 34), -- Corregido (antes 84)
('PayPal', 90.00, '2025-10-05', 35),   -- Corregido (antes 85)
('Transferencia', 110.00, '2025-10-15', 39), -- Corregido (antes 89)
('Tarjeta', 130.00, '2025-10-25', 40), -- Corregido (antes 90)
('PayPal', 400.00, '2025-11-05', 41),  -- Corregido (antes 91)
('Transferencia', 220.00, '2025-11-15', 42), -- Corregido (antes 92)
('Tarjeta', 180.00, '2025-11-25', 43), -- Corregido (antes 93)
('PayPal', 150.00, '2025-12-05', 44),  -- Corregido (antes 94)
('Transferencia', 300.00, '2025-12-15', 45), -- Corregido (antes 95)
('Tarjeta', 25.00, '2025-12-20', 46),  -- Corregido (antes 96)
('PayPal', 45.00, '2026-01-05', 47),   -- Corregido (antes 97)
('Transferencia', 12.50, '2026-01-10', 48), -- Corregido (antes 98)
('Tarjeta', 60.00, '2026-01-12', 50),  -- Corregido (antes 100)
('Transferencia', 85.00, '2025-02-28', 1), 
('Tarjeta', 120.00, '2025-05-10', 2),
('PayPal', 45.00, '2025-06-15', 3), 
('Transferencia', 210.00, '2025-07-20', 1),
('Tarjeta', 95.00, '2025-08-05', 10), 
('PayPal', 30.00, '2025-09-12', 12),
('Transferencia', 15.50, '2025-10-01', 23), 
('Tarjeta', 44.00, '2025-10-15', 34),
('PayPal', 120.00, '2025-11-20', 45), 
('Transferencia', 22.00, '2025-12-05', 6), -- Corregido (antes 56)
('Tarjeta', 250.00, '2025-03-25', 12), 
('PayPal', 140.00, '2025-04-12', 24),
('Transferencia', 35.00, '2025-05-18', 35), 
('Tarjeta', 88.00, '2025-06-22', 46),
('PayPal', 12.50, '2025-07-30', 7),    -- Corregido (antes 57)
('Transferencia', 65.00, '2025-08-14', 18), -- Corregido (antes 68)
('Tarjeta', 19.50, '2025-09-21', 29),  -- Corregido (antes 79)
('PayPal', 45.00, '2025-10-30', 31),   -- Corregido (antes 81)
('Transferencia', 300.00, '2025-11-12', 3), 
('Tarjeta', 15.00, '2025-12-01', 14),
('PayPal', 115.00, '2025-04-15', 25), 
('Transferencia', 42.00, '2025-05-10', 36),
('Tarjeta', 18.00, '2025-06-05', 47), 
('PayPal', 55.00, '2025-07-12', 8),    -- Corregido (antes 58)
('Transferencia', 130.00, '2025-08-20', 19), -- Corregido (antes 69)
('Tarjeta', 22.00, '2025-09-05', 21),  -- Corregido (antes 71)
('PayPal', 8.50, '2025-10-10', 32),    -- Corregido (antes 82)
('Transferencia', 99.00, '2025-11-15', 42), -- Corregido (antes 92)
('Tarjeta', 14.00, '2025-12-20', 4), 
('PayPal', 33.00, '2025-02-10', 16),
('Transferencia', 56.00, '2025-03-12', 27), 
('Tarjeta', 12.00, '2025-04-14', 38),
('PayPal', 85.00, '2025-05-16', 49), 
('Transferencia', 40.00, '2025-06-18', 11), -- Corregido (antes 61)
('Tarjeta', 20.00, '2025-07-20', 22),  -- Corregido (antes 72)
('PayPal', 150.00, '2025-08-22', 33),  -- Corregido (antes 83)
('Transferencia', 75.00, '2025-09-24', 43), -- Corregido (antes 93)
('Tarjeta', 40.00, '2025-10-26', 5),
('PayPal', 90.00, '2025-11-28', 17), 
('Transferencia', 200.00, '2025-12-30', 28),
('Tarjeta', 35.00, '2026-01-05', 39);

INSERT INTO empleadosGamas (idEmpleado, idGama, nivel) VALUES
(1, 1, 'Experto'), (1, 5, 'Medio'),
(2, 1, 'Experto'), (2, 6, 'Experto'), (2, 10, 'Medio'),
(3, 2, 'Medio'), (3, 7, 'Básico'),
(4, 4, 'Experto'), (4, 14, 'Medio'),
(5, 5, 'Experto'), (5, 9, 'Medio'),
(6, 6, 'Básico'), (6, 13, 'Básico'),
(7, 7, 'Experto'), (7, 11, 'Medio'),
(8, 8, 'Medio'), (8, 9, 'Experto'),
(9, 9, 'Básico'), (9, 10, 'Básico'),
(10, 10, 'Experto'), (10, 1, 'Medio'),
(11, 11, 'Medio'), (11, 12, 'Básico'),
(12, 12, 'Básico'), (12, 6, 'Medio'),
(13, 13, 'Experto'), (13, 1, 'Básico'),
(14, 14, 'Medio'), (14, 4, 'Básico'),
(15, 15, 'Básico'), (15, 2, 'Medio'),
(16, 1, 'Experto'), (16, 2, 'Experto'),
(17, 3, 'Medio'), (17, 8, 'Básico'),
(18, 4, 'Básico'), (18, 14, 'Experto'),
(19, 5, 'Experto'), (19, 11, 'Medio'),
(20, 6, 'Medio'), (20, 13, 'Básico'),
(21, 1, 'Experto'), (22, 2, 'Medio'), (23, 3, 'Básico'), (24, 4, 'Experto'),
(25, 5, 'Medio'), (26, 6, 'Básico'), (27, 7, 'Experto'), (28, 8, 'Medio'),
(29, 9, 'Básico'), (30, 10, 'Experto'), (31, 11, 'Medio'), (32, 12, 'Básico'),
(33, 13, 'Experto'), (34, 14, 'Medio'), (35, 15, 'Básico'), (36, 1, 'Experto'),
(37, 2, 'Medio'), (38, 3, 'Básico'), (39, 4, 'Experto'), (40, 5, 'Medio'),
(21, 11, 'Medio'),
(22, 12, 'Experto'),
(23, 13, 'Básico'),
(24, 14, 'Experto'),
(25, 15, 'Medio'),
(26, 11, 'Básico'),
(27, 12, 'Medio'),
(28, 13, 'Experto'),
(29, 14, 'Básico'),
(30, 15, 'Experto'),
(31, 1, 'Medio'),
(32, 2, 'Básico'),
(33, 4, 'Experto'),
(34, 5, 'Medio'),
(35, 6, 'Básico'),
(36, 14, 'Experto'),
(37, 15, 'Medio'),
(38, 1, 'Básico'),
(39, 2, 'Experto'),
(40, 3, 'Medio');