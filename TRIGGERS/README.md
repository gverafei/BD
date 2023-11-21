# Procedimientos almacenados SQL.
Prácticas en clase Base de datos

### Ejemplo 1
```
-- Paso 1
DROP DATABASE IF EXISTS test;
CREATE DATABASE test;
USE test;

-- Paso 2 
CREATE TABLE alumnos (
    id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(50) NOT NULL,
    apellido1 VARCHAR(50) NOT NULL,
    apellido2 VARCHAR(50), 
    nota FLOAT
);

-- Paso 3
DELIMITER $$
DROP TRIGGER IF EXISTS trigger_check_nota_before_insert$$
CREATE TRIGGER trigger_check_nota_before_insert
BEFORE INSERT
ON alumnos FOR EACH ROW
BEGIN
  IF NEW.nota < 0 THEN
    set NEW.nota = 0;
  ELSEIF NEW.nota > 10 THEN
    set NEW.nota = 10;
  END IF;
END $$
DELIMITER ;

-- Paso 4
DELIMITER $$
DROP TRIGGER IF EXISTS trigger_check_nota_before_update$$
CREATE TRIGGER trigger_check_nota_before_update
BEFORE UPDATE
ON alumnos FOR EACH ROW
BEGIN
  IF NEW.nota < 0 THEN
    set NEW.nota = 0;
  ELSEIF NEW.nota > 10 THEN
    set NEW.nota = 10;
  END IF;
END $$
DELIMITER ;

-- Paso 5
INSERT INTO alumnos VALUES (1, 'Pepe', 'López', 'López', -1);
INSERT INTO alumnos VALUES (2, 'María', 'Sánchez', 'Sánchez', 11);
INSERT INTO alumnos VALUES (3, 'Juan', 'Pérez', 'Pérez', 8.5);

-- Paso 6
SELECT * FROM alumnos;

-- Paso 7
UPDATE alumnos SET nota = -4 WHERE id = 3;
UPDATE alumnos SET nota = 14 WHERE id = 3;
UPDATE alumnos SET nota = 9.5 WHERE id = 3;

-- Paso 8
SELECT * FROM alumnos;
```

### Ejemplo 2
```
-- Paso 0
DROP DATABASE IF EXISTS test;
CREATE DATABASE test;
USE test;

-- Paso 1
DROP TABLE IF EXISTS alumnos;
CREATE TABLE alumnos (
    id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(50) NOT NULL,
    apellido1 VARCHAR(50) NOT NULL,
    apellido2 VARCHAR(50), 
    email VARCHAR(50) NOT NULL
);

-- Paso 2
DROP PROCEDURE IF EXISTS crear_email;
DELIMITER $$
CREATE PROCEDURE crear_email(IN nombre VARCHAR(50), IN apellido1 VARCHAR(50), IN apellido2 VARCHAR(50), IN dominio VARCHAR(50), OUT email VARCHAR(50))
BEGIN
  SET email = (SELECT LOWER(CONCAT(LEFT(nombre, 1), LEFT(apellido1, 3), LEFT(apellido2, 3), '@', dominio)));
END $$ 
DELIMITER ;

CALL crear_email('Pedro', 'Perez', 'Chavez', 'uv.mx', @email);
SELECT @email;

-- Paso 3
DELIMITER $$
DROP TRIGGER IF EXISTS trigger_crear_email_before_insert$$
CREATE TRIGGER trigger_crear_email_before_insert
BEFORE INSERT
ON alumnos FOR EACH ROW
BEGIN
  IF NEW.email IS NULL THEN
    CALL crear_email(NEW.nombre, NEW.apellido1, NEW.apellido2, 'uv.mx', @email);
    set NEW.email = @email;
  END IF;
END $$
DELIMITER ;

-- Paso 4
DROP TABLE IF EXISTS log_cambios_email;

-- Paso 4
CREATE TABLE log_cambios_email (
    id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    id_alumno VARCHAR(50) NOT NULL,
    fecha_hora DATETIME NOT NULL,
    old_email VARCHAR(50), 
    new_email VARCHAR(50) NOT NULL
);

-- Paso 1
INSERT INTO alumnos VALUES (1, 'Pepe', 'Lopez', 'Baena', NULL);
INSERT INTO alumnos VALUES (2, 'María', 'Sanchez', 'Aguirre', NULL);
INSERT INTO alumnos VALUES (3, 'Juan', 'Perez', 'Peña', NULL);

-- Paso 2
SELECT * FROM alumnos;

-- Paso 3
UPDATE alumnos SET email = 'pepe@uv.mx' WHERE id = 1;
UPDATE alumnos SET email = 'maria@uv.mx' WHERE id = 2;
UPDATE alumnos SET email = 'juan@uv.mx' WHERE id = 3;

-- Paso 4
SELECT * FROM alumnos;

-- Paso 5
SELECT * FROM log_cambios_email;
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
