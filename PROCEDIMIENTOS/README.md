# Procedimientos almacenados SQL.
Prácticas en clase Base de datos


### Base de datos netflix
```sql
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

### Ejemplo 1
Escriba un procedimiento llamado `listar_peliculas` que reciba como entrada el `id` de la categoria y muestre un listado de todos las películas que existen dentro de esa categoria.

Este procedimiento no devuelve ningún parámetro de salida, lo que hace es mostrar el listado de los productos.


```sql
DROP PROCEDURE IF EXISTS listar_peliculas;
DELIMITER $$
CREATE PROCEDURE listar_peliculas(IN id_genero INT)
BEGIN
  SELECT * FROM pelicula WHERE pelicula.id_genero=id_genero;
END $$ 
DELIMITER ;

-- Para la lista de procedimientos
SHOW PROCEDURE STATUS WHERE db = 'netflix' \G;

-- Para llamar al procedimiento
CALL listar_peliculas(2);
```

### Ejemplo 2
Escriba un procedimiento llamado `contar_peliculas` que reciba como entrada el id de la categoria y devuelva el número de películas que existen dentro de esa categoria.

```sql
DROP PROCEDURE IF EXISTS contar_peliculas;
DELIMITER $$
CREATE PROCEDURE contar_peliculas(IN id_genero INT, OUT total INT)
BEGIN
  SET total = (SELECT COUNT(*) FROM pelicula WHERE pelicula.id_genero=id_genero);
END $$ 
DELIMITER ;

-- Para la lista de procedimientos
SHOW PROCEDURE STATUS WHERE db = 'netflix' \G;

-- Para llamar al procedimiento
CALL contar_peliculas(2, @total);
SELECT @total;
```

### Ejemplo 3
Escriba un procedimiento llamado `calcular_max_min_media` que devuelva como salida tres parámetros. El precio máximo, el precio mínimo y la media de las peliculas que existen en esa categoria.
```sql
DROP PROCEDURE IF EXISTS calcular_max_min_media;
DELIMITER $$
CREATE PROCEDURE calcular_max_min_media (OUT maximo DECIMAL(7, 2), OUT minimo DECIMAL(7, 2), OUT media DECIMAL(7, 2))
BEGIN
SET maximo = (SELECT MAX(precio) FROM pelicula);
SET minimo = (SELECT MIN(precio) FROM pelicula);
SET media = (SELECT AVG(precio) FROM pelicula);
END $$ 
DELIMITER ;

-- Para llamar al procedimiento
CALL calcular_max_min_media(@maximo, @minimo, @media);
SELECT @maximo, @minimo, @media;
```
