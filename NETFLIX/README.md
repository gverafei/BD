# Conexión desde un lenguaje Anfitrión SQL.
Prácticas en clase Base de datos

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
```
