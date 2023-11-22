# Vistas de datos SQL.
Prácticas en clase Base de datos

### Crear vistas. Ejemplo 1
Ejercicio: Crear una vista llamada resumen_peliculas que muestre para cada película, el id, el titulo, el nombre del genero, el nombre de su clasificacion y el precio.
```sql
CREATE OR REPLACE VIEW resumen_peliculas AS 
  SELECT pelicula.id, pelicula.titulo, genero.nombre AS genero, clasificacion.nombre AS clasificacion, pelicula.precio 
  FROM 
  pelicula INNER JOIN genero ON pelicula.id_genero=genero.id 
  INNER JOIN clasificacion ON pelicula.id_clasificacion=clasificacion.id 
  ORDER BY pelicula.titulo;

SELECT * FROM resumen_peliculas;

SHOW FULL TABLES;
```

### Crear vistas. Ejemplo 2
Crear una vista llamada resumen_peliculas que muestre para cada película, el id, el titulo, el nombre del genero, el nombre de su clasificacion y el precio. Colocar los alias en la clausula de creación.
```sql
CREATE OR REPLACE VIEW resumen_peliculas (id, titulo, genero, clasificacion, precio) AS 
SELECT pelicula.id, pelicula.titulo, genero.nombre, clasificacion.nombre, pelicula.precio 
FROM 
pelicula INNER JOIN genero ON pelicula.id_genero=genero.id 
INNER JOIN clasificacion ON pelicula.id_clasificacion=clasificacion.id 
ORDER BY pelicula.titulo;
SELECT * FROM resumen_peliculas;
SHOW FULL TABLES WHERE table_type = 'VIEW';
```

### Renombrar vistas
Cambiarle el nombre a la vista resumen_peliculas a listado_peliculas.

```sql
RENAME TABLE resumen_peliculas TO listado_peliculas;
SHOW FULL TABLES;
```

### Eliminar vistas
Cambiarle el nombre a la vista resumen_peliculas a listado_peliculas.

```sql
RENAME TABLE resumen_peliculas TO listado_peliculas;
SHOW FULL TABLES;
```

### Práctica con Vistas
1. Escriba una vista que se llame listado_pedidos_clientes que muestre un listado donde aparezcan todos los clientes y los pedidos que ha realizado cada uno de ellos. La vista deberá tener las siguientes columnas: nombre y apellidos del cliente concatenados, ciudad, id del pedido, fecha del pedido y la cantidad total del pedido.

```sql
-- Ejercicio 1
CREATE OR REPLACE VIEW listado_pedidos_clientes (cliente_id, cliente, ciudad, pedido_id, fecha, total) AS 
  SELECT c.id, CONCAT_WS(' ', c.nombre, c.apellido1, c.apellido2), ciudad, p.id, p.fecha, p.total 
  FROM cliente c INNER JOIN pedido p ON c.id=p.id_cliente;

-- Verificamos el resultado de la Vista
SELECT * FROM listado_pedidos_clientes;

-- Listamos las listas creadas
SHOW FULL TABLES WHERE table_type = 'VIEW';
```
2. Escriba una vista que se llame listado_pedidos_vendedores que muestre un listado donde aparezcan todos los vendedores y los pedidos que han vendido cada uno de ellos. La vista deberá tener las siguientes columnas: nombre y apellidos del vendedor concatenados, id del pedido, fecha del pedido, la cantidad total del pedido y el calculo de su comisión (pedido.total * vendedor.comision).

```sql
-- Ejercicio 2
CREATE OR REPLACE VIEW listado_pedidos_vendedores (vendedor_id, vendedor, pedido_id, fecha, total, comision) AS 
  SELECT v.id, CONCAT_WS(' ', v.nombre, v.apellido1, v.apellido2), p.id, p.fecha, p.total, TRUNCATE(p.total*v.comision,2) 
  FROM vendedor v INNER JOIN pedido p ON v.id=p.id_vendedor;

-- Verificamos el resultado de la Vista
SELECT * FROM listado_pedidos_vendedores;

-- Listamos las listas creadas
SHOW FULL TABLES WHERE table_type = 'VIEW';
```
3. Utilice las vistas que ha creado en los pasos anteriores para devolver un listado de los clientes de la ciudad de Almería que han realizado pedidos.

```sql
-- Ejercicio 3
SELECT * FROM listado_pedidos_clientes WHERE ciudad='Almería';
```
4. Utilice las vistas que ha creado en los pasos anteriores para calcular el número de pedidos que se ha realizado cada uno de los clientes.

```sql
-- Ejercicio 4
SELECT cliente_id, cliente, COUNT(*) FROM listado_pedidos_clientes GROUP BY cliente_id;
```
5. Utilice las vistas que ha creado en los pasos anteriores para calcular el valor del pedido máximo y mínimo que ha realizado cada cliente.

```sql
-- Ejercicio 5
SELECT cliente_id, cliente, MAX(total), MIN(total) FROM listado_pedidos_clientes GROUP BY cliente_id;
```
6. Modifique el nombre de las vista listado_pedidos_clientes y asígnele el nombre listado_de_pedidos.

```sql
-- Ejercicio 6
RENAME TABLE listado_pedidos_clientes TO listado_pedidos;

-- Listamos las listas creadas
SHOW FULL TABLES WHERE table_type = 'VIEW';
```
7. Elimine las vistas que ha creado en los pasos anteriores. 

```sql
-- Ejercicio 7
DROP VIEW listado_pedidos;
DROP VIEW listado_pedidos_vendedores;
```

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

### Base de datos ventas
```sql
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
