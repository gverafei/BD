# Vistas de datos SQL.
Prácticas en clase Base de datos

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
### Base de datos netflix
```
DROP DATABASE IF EXISTS netflix;
CREATE DATABASE netflix CHARACTER SET utf8mb4;
USE netflix;

CREATE TABLE IF NOT EXISTS genero (
  id INT UNSIGNED PRIMARY KEY, 
  nombre VARCHAR(60) NOT NULL
);

CREATE TABLE IF NOT EXISTS clasificacion (
  id INT UNSIGNED PRIMARY KEY, 
  nombre VARCHAR(60) NOT NULL
);

CREATE TABLE IF NOT EXISTS pelicula (
  id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
  id_genero INT UNSIGNED NOT NULL,
  id_clasificacion INT UNSIGNED NOT NULL,
  titulo VARCHAR(60) NOT NULL,
  sinopsis VARCHAR(60) NOT NULL,
  precio DECIMAL(7,2) NOT NULL CHECK (precio > 0),
  CONSTRAINT fk_genero FOREIGN KEY (id_genero) 
  REFERENCES genero(id) 
  ON DELETE CASCADE ON UPDATE CASCADE,
  CONSTRAINT fk_clasificacion FOREIGN KEY (id_clasificacion) 
  REFERENCES clasificacion(id) 
  ON DELETE CASCADE ON UPDATE CASCADE
);

INSERT INTO clasificacion VALUES(1,'A');
INSERT INTO clasificacion (id, nombre) VALUES(2,'AA');
INSERT INTO clasificacion VALUES(3,'B');
INSERT INTO clasificacion VALUES(4,'B-15');
INSERT INTO clasificacion VALUES(5,'C');

INSERT INTO genero (id, nombre) VALUES(1,'Acción');
INSERT INTO genero (id, nombre) VALUES(2,'Aventura');
INSERT INTO genero (id, nombre) VALUES(3,'Comedia');
INSERT INTO genero (id, nombre) VALUES(4,'Romance');
INSERT INTO genero (id, nombre) VALUES(5,'Suspenso');
INSERT INTO genero (id, nombre) VALUES(6,'Terror');
INSERT INTO genero (id, nombre) VALUES(7,'Infantil');

INSERT INTO pelicula (id_genero, id_clasificacion, titulo, sinopsis, precio) VALUES(2, 4, 'The Matrix', 'Una película de ciencia ficción', 300.00);
INSERT INTO pelicula (id_genero, id_clasificacion, titulo, sinopsis, precio) VALUES(7, 2, 'Mohana', 'Para los peques', 280.00);
INSERT INTO pelicula (id_genero, id_clasificacion, titulo, sinopsis, precio) VALUES(2, 4, 'Barbie', 'Una muñeca con aventuras', 310.00);
INSERT INTO pelicula (id_genero, id_clasificacion, titulo, sinopsis, precio) VALUES(3, 4, 'Sonic', 'Una película de videojuegos', 230.00);
INSERT INTO pelicula (id_genero, id_clasificacion, titulo, sinopsis, precio) VALUES(4, 3, 'El amor de invierno', 'Película romántica', 150.00);
INSERT INTO pelicula (id_genero, id_clasificacion, titulo, sinopsis, precio) VALUES(6, 5, 'Scream', 'Es una de terror', 290.00);
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
