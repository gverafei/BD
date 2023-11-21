# Índices de datos SQL.
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
