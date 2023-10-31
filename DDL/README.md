# Creación de bases de datos en MySQL.
Prácticas en clase Base de datos

### Base de datos proveedores
Ejemplo 1:
```
DROP DATABASE IF EXISTS proveedores;
CREATE DATABASE proveedores CHARSET utf8mb4;
USE proveedores;

CREATE TABLE categoria (
  id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
  nombre VARCHAR(100) NOT NULL
);

CREATE TABLE pieza (
  id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
  nombre VARCHAR(100) NOT NULL,
  color VARCHAR(50) NOT NULL,
  precio DECIMAL(7,2) NOT NULL,  
  id_categoria INT UNSIGNED NOT NULL,
  FOREIGN KEY (id_categoria) REFERENCES categoria(id)
  ON DELETE RESTRICT
  ON UPDATE RESTRICT
);

INSERT INTO categoria VALUES (1, 'Categoria A');
INSERT INTO categoria VALUES (2, 'Categoria B');
INSERT INTO categoria VALUES (3, 'Categoria C');

INSERT INTO pieza VALUES (1, 'Pieza 1', 'Blanco', 25.90, 1);
INSERT INTO pieza VALUES (2, 'Pieza 2', 'Verde', 32.75, 1);
INSERT INTO pieza VALUES (3, 'Pieza 3', 'Rojo', 12.00, 2);
INSERT INTO pieza VALUES (4, 'Pieza 4', 'Azul', 24.50, 2);

SELECT * FROM categoria;
SELECT * FROM pieza;
DELETE FROM categoria WHERE id=1;
DELETE FROM categoria WHERE id=3;
UPDATE categoria SET id=4 WHERE id=1;
```
+ ¿Podríamos borrar la **Categoría A** de la tabla **categoria**?
+ ¿Y la **Categoría C**?
+ ¿Podríamos actualizar la **Categoría A** de la tabla **categoria**?

### Base de datos proveedores
Ejemplo 2:
```
DROP DATABASE IF EXISTS proveedores;
CREATE DATABASE proveedores CHARSET utf8mb4;
USE proveedores;

CREATE TABLE categoria (
  id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
  nombre VARCHAR(100) NOT NULL
);

CREATE TABLE pieza (
  id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
  nombre VARCHAR(100) NOT NULL,
  color VARCHAR(50) NOT NULL,
  precio DECIMAL(7,2) NOT NULL,  
  id_categoria INT UNSIGNED NOT NULL,
  FOREIGN KEY (id_categoria) REFERENCES categoria(id)
  ON DELETE CASCADE
  ON UPDATE CASCADE
);

INSERT INTO categoria VALUES (1, 'Categoria A');
INSERT INTO categoria VALUES (2, 'Categoria B');
INSERT INTO categoria VALUES (3, 'Categoria C');

INSERT INTO pieza VALUES (1, 'Pieza 1', 'Blanco', 25.90, 1);
INSERT INTO pieza VALUES (2, 'Pieza 2', 'Verde', 32.75, 1);
INSERT INTO pieza VALUES (3, 'Pieza 3', 'Rojo', 12.00, 2);
INSERT INTO pieza VALUES (4, 'Pieza 4', 'Azul', 24.50, 2);

SELECT * FROM categoria;
SELECT * FROM pieza;
DELETE FROM categoria WHERE id=1;
UPDATE categoria SET id=4 WHERE id=1;
```
+ ¿Podríamos actualizar la **Categoría A** de la tabla **categoria**?
+ ¿Qué le ocurre a las piezas que pertenecen la **Categoría A** después de actualizarla?
+ ¿Podríamos borrar la **Categoría A** de la tabla **categoria**?
+ ¿Qué le ocurre a las piezas que pertenecen la **Categoría A** después de borrarla?

### Base de datos proveedores
Ejemplo 3:
```
DROP DATABASE IF EXISTS proveedores;
CREATE DATABASE proveedores CHARSET utf8mb4;
USE proveedores;

CREATE TABLE categoria (
  id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
  nombre VARCHAR(100) NOT NULL
);

CREATE TABLE pieza (
  id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
  nombre VARCHAR(100) NOT NULL,
  color VARCHAR(50) NOT NULL,
  precio DECIMAL(7,2) NOT NULL,  
  id_categoria INT UNSIGNED,
  FOREIGN KEY (id_categoria) REFERENCES categoria(id)
  ON DELETE SET NULL
  ON UPDATE SET NULL
);

INSERT INTO categoria VALUES (1, 'Categoria A');
INSERT INTO categoria VALUES (2, 'Categoria B');
INSERT INTO categoria VALUES (3, 'Categoria C');

INSERT INTO pieza VALUES (1, 'Pieza 1', 'Blanco', 25.90, 1);
INSERT INTO pieza VALUES (2, 'Pieza 2', 'Verde', 32.75, 1);
INSERT INTO pieza VALUES (3, 'Pieza 3', 'Rojo', 12.00, 2);
INSERT INTO pieza VALUES (4, 'Pieza 4', 'Azul', 24.50, 2);

SELECT * FROM categoria;
SELECT * FROM pieza;
DELETE FROM categoria WHERE id=1;
DELETE FROM categoria WHERE id=3;
UPDATE categoria SET id=4 WHERE id=1;
```
+ ¿Podríamos actualizar la **Categoría A** de la tabla **categoria**?
+ ¿Qué le ocurre a las piezas que pertenecen la **Categoría A** después de actualizarla?
+ ¿Podríamos borrar la **Categoría A** de la tabla **categoria**?
+ ¿Qué le ocurre a las piezas que pertenecen la **Categoría A** después de borrarla?

## Autor

Desarrollo de Sistemas Web  
Universidad Veracruzana
