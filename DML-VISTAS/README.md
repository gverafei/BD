# Vistas de datos SQL.
Prácticas en clase Base de datos

### Creación de índices
```
DROP TABLE IF EXISTS cliente;
CREATE TABLE cliente (
  id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
  nombre VARCHAR(50) NOT NULL,
  email VARCHAR(15) NOT NULL UNIQUE,
  telefono VARCHAR(9) NOT NULL,
  INDEX idx_nombre (nombre)
);

DESCRIBE cliente;

SHOW INDEXES FROM cliente \G;
```

### Ejercicios de Vistas
```
CREATE OR REPLACE VIEW resumen_peliculas AS 
  SELECT pelicula.id, pelicula.titulo, genero.nombre AS genero, clasificacion.nombre AS clasificacion, pelicula.precio 
  FROM 
  pelicula INNER JOIN genero ON pelicula.id_genero=genero.id 
  INNER JOIN clasificacion ON pelicula.id_clasificacion=clasificacion.id 
  ORDER BY pelicula.titulo;

SELECT * FROM resumen_peliculas;

SHOW FULL TABLES;
```

### Ejercicios de Vistas
```
CREATE OR REPLACE VIEW resumen_peliculas (id, titulo, genero, clasificacion, precio) AS 
SELECT pelicula.id, pelicula.titulo, genero.nombre, clasificacion.nombre, pelicula.precio 
FROM 
pelicula INNER JOIN genero ON pelicula.id_genero=genero.id 
INNER JOIN clasificacion ON pelicula.id_clasificacion=clasificacion.id 
ORDER BY pelicula.titulo;
SELECT * FROM resumen_peliculas;
SHOW FULL TABLES WHERE table_type = 'VIEW';
```

### Base de datos netflix
```
DROP DATABASE IF EXISTS netflix;
CREATE DATABASE netflix CHARACTER SET utf8mb4;
USE netflix;

CREATE TABLE IF NOT EXISTS tipo (
  idtipo INT UNSIGNED PRIMARY KEY, 
  nombre VARCHAR(60) NOT NULL
);

CREATE TABLE IF NOT EXISTS categoria (
  idcategoria INT UNSIGNED PRIMARY KEY,
  nombre VARCHAR(60) NOT NULL
);

CREATE TABLE IF NOT EXISTS pelicula (
  idpelicula INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
  idtipo INT UNSIGNED NOT NULL,
  titulo VARCHAR(255) NOT NULL,
  imagen VARCHAR(255) NOT NULL,
  CONSTRAINT fk_idtipo FOREIGN KEY (idtipo) 
  REFERENCES tipo(idtipo) 
  ON DELETE CASCADE ON UPDATE CASCADE
);

CREATE TABLE IF NOT EXISTS pelicula_categoria (
  idpelicula INT UNSIGNED NOT NULL,
  idcategoria INT UNSIGNED NOT NULL,
  CONSTRAINT fk_idpelicula FOREIGN KEY (idpelicula) REFERENCES pelicula(idpelicula) 
  ON DELETE CASCADE ON UPDATE CASCADE,
  CONSTRAINT fk_idcategria FOREIGN KEY (idcategoria) REFERENCES categoria(idcategoria) 
  ON DELETE CASCADE ON UPDATE CASCADE,
  CONSTRAINT pk_pelicula_categoria PRIMARY KEY (idpelicula, idcategoria)
);

INSERT INTO tipo VALUES(1,'Películas');
INSERT INTO tipo VALUES(2,'Series');

INSERT INTO categoria VALUES(1,'Acción');
INSERT INTO categoria VALUES(2,'Tendencias');
INSERT INTO categoria VALUES(3,'Originales de Netflix');
INSERT INTO categoria VALUES(4,'Populares de Netflix');
INSERT INTO categoria VALUES(5,'Joyas ocultas');
INSERT INTO categoria VALUES(6,'Ciencia ficción');
INSERT INTO categoria VALUES(7,'Terror');
INSERT INTO categoria VALUES(8,'Aventuras emocionantes');

INSERT INTO pelicula VALUES (2,1,'Matrix','uploads/matrix.jpg'),(3,1,'300','uploads/300.jpg'),(4,1,'Anabelle','uploads/annabelle.jpg'),(5,1,'Batam V Superman','uploads/batmanvsuperman.jpg'),(6,2,'Breaking Bad','uploads/breakingbad.jpg'),(7,2,'Cobra Kai','uploads/cobrakai.jpg'),(8,1,'Constantine','uploads/constantine.jpg'),(9,1,'Al filo del mañana','uploads/edge.jpg'),(10,1,'El conjuro','uploads/elconjuro.jpg'),(11,1,'El silencio','uploads/elsilencio.jpg'),(12,1,'Gambito de dama','uploads/gambitodedama.jpg'),(13,1,'Guerra mundial Z','uploads/guerramundialz.jpg'),(14,1,'Indiana Jones 3','uploads/indiana3.jpg'),(15,1,'Eso','uploads/it.jpg'),(16,1,'La casa de papel','uploads/lacasadepapel.jpg'),(17,2,'Luis Miguel','uploads/luismiguel.jpg'),(18,2,'The Witcher','uploads/thewitcher.jpg'),(19,1,'Matrix 3','uploads/matrix3.jpg'),(20,1,'Mujer Maravilla','uploads/mujermaravilla.jpg'),(21,1,'Spiderman 2','uploads/spiderman2.jpg'),(22,2,'The Crown','uploads/thecrown.jpg'),(23,2,'The Walking Dead','uploads/thewalkingdead.jpg'),(24,1,'Volver al futuro 3','uploads/volveralfuturo3.jpg');

INSERT INTO pelicula_categoria VALUES (2,1),(3,1),(5,1),(9,1),(14,1),(19,1),(6,2),(12,2),(19,2),(20,2),(22,2),(24,2),(7,3),(11,3),(16,3),(17,3),(18,3),(22,3),(6,4),(13,4),(16,4),(20,4),(23,4),(24,4),(2,5),(8,5),(13,5),(16,5),(21,5),(24,5),(2,6),(5,6),(9,6),(21,6),(23,6),(24,6),(4,7),(10,7),(15,7),(3,8),(9,8),(14,8),(20,8),(21,8),(24,8);
```

### Base de datos ventas
```
DROP DATABASE IF EXISTS ventas;
CREATE DATABASE ventas CHARACTER SET utf8mb4;
USE ventas;

CREATE TABLE cliente (
  id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
  nombre VARCHAR(100) NOT NULL,
  apellido1 VARCHAR(100) NOT NULL,
  apellido2 VARCHAR(100),
  ciudad VARCHAR(100),
  categoría INT UNSIGNED
);

CREATE TABLE vendedor (
  id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
  nombre VARCHAR(100) NOT NULL,
  apellido1 VARCHAR(100) NOT NULL,
  apellido2 VARCHAR(100),
  comision FLOAT
);

CREATE TABLE pedido (
  id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
  total DOUBLE NOT NULL,
  fecha DATE,
  id_cliente INT UNSIGNED NOT NULL,
  id_vendedor INT UNSIGNED NOT NULL,
  FOREIGN KEY (id_cliente) REFERENCES cliente(id),
  FOREIGN KEY (id_vendedor) REFERENCES vendedor(id)
);

INSERT INTO cliente VALUES(1, 'Aarón', 'Rivero', 'Gómez', 'Almería', 100);
INSERT INTO cliente VALUES(2, 'Adela', 'Salas', 'Díaz', 'Granada', 200);
INSERT INTO cliente VALUES(3, 'Adolfo', 'Rubio', 'Flores', 'Sevilla', NULL);
INSERT INTO cliente VALUES(4, 'Adrián', 'Suárez', NULL, 'Jaén', 300);
INSERT INTO cliente VALUES(5, 'Marcos', 'Loyola', 'Méndez', 'Almería', 200);
INSERT INTO cliente VALUES(6, 'María', 'Santana', 'Moreno', 'Cádiz', 100);
INSERT INTO cliente VALUES(7, 'Pilar', 'Ruiz', NULL, 'Sevilla', 300);
INSERT INTO cliente VALUES(8, 'Pepe', 'Ruiz', 'Santana', 'Huelva', 200);
INSERT INTO cliente VALUES(9, 'Guillermo', 'López', 'Gómez', 'Granada', 225);
INSERT INTO cliente VALUES(10, 'Daniel', 'Santana', 'Loyola', 'Sevilla', 125);

INSERT INTO vendedor VALUES(1, 'Daniel', 'Sáez', 'Vega', 0.15);
INSERT INTO vendedor VALUES(2, 'Juan', 'Gómez', 'López', 0.13);
INSERT INTO vendedor VALUES(3, 'Diego','Flores', 'Salas', 0.11);
INSERT INTO vendedor VALUES(4, 'Marta','Herrera', 'Gil', 0.14);
INSERT INTO vendedor VALUES(5, 'Antonio','Carretero', 'Ortega', 0.12);
INSERT INTO vendedor VALUES(6, 'Manuel','Domínguez', 'Hernández', 0.13);
INSERT INTO vendedor VALUES(7, 'Antonio','Vega', 'Hernández', 0.11);
INSERT INTO vendedor VALUES(8, 'Alfredo','Ruiz', 'Flores', 0.05);

INSERT INTO pedido VALUES(1, 150.5, '2017-10-05', 5, 2);
INSERT INTO pedido VALUES(2, 270.65, '2016-09-10', 1, 5);
INSERT INTO pedido VALUES(3, 65.26, '2017-10-05', 2, 1);
INSERT INTO pedido VALUES(4, 110.5, '2016-08-17', 8, 3);
INSERT INTO pedido VALUES(5, 948.5, '2017-09-10', 5, 2);
INSERT INTO pedido VALUES(6, 2400.6, '2016-07-27', 7, 1);
INSERT INTO pedido VALUES(7, 5760, '2015-09-10', 2, 1);
INSERT INTO pedido VALUES(8, 1983.43, '2017-10-10', 4, 6);
INSERT INTO pedido VALUES(9, 2480.4, '2016-10-10', 8, 3);
INSERT INTO pedido VALUES(10, 250.45, '2015-06-27', 8, 2);
INSERT INTO pedido VALUES(11, 75.29, '2016-08-17', 3, 7);
INSERT INTO pedido VALUES(12, 3045.6, '2017-04-25', 2, 1);
INSERT INTO pedido VALUES(13, 545.75, '2019-01-25', 6, 1);
INSERT INTO pedido VALUES(14, 145.82, '2017-02-02', 6, 1);
INSERT INTO pedido VALUES(15, 370.85, '2019-03-11', 1, 5);
INSERT INTO pedido VALUES(16, 2389.23, '2019-03-11', 1, 5);
```
