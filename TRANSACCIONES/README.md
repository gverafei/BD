# Transacciones.
Prácticas en clase Base de datos

### Ejemplo 1
```sql
DROP DATABASE IF EXISTS test;
CREATE DATABASE test CHARACTER SET utf8mb4;
USE test;
CREATE TABLE cliente (
	id INT UNSIGNED PRIMARY KEY,
	nombre CHAR (20)
);

START TRANSACTION;
INSERT INTO cliente VALUES (1, 'Pepe');
COMMIT;

-- 1. ¿Qué devolverá esta consulta?
SELECT * FROM cliente;

-- 2. Suponga que queremos realizar una transferencia de dinero entre dos cuentas bancarias con la siguiente transacción:
SET AUTOCOMMIT=0;
START TRANSACTION;
UPDATE cuentas SET saldo = saldo - 100 WHERE id = 1;
UPDATE cuentas SET saldo = saldo + 100 WHERE id = 2;
COMMIT;

-- 3. ¿Qué devolverá esta consulta?
SELECT * FROM cuentas;

-- 4. Suponga que queremos realizar una transferencia de dinero entre dos cuentas bancarias con la siguiente transacción y una de las dos cuentas no existe:
START TRANSACTION;
UPDATE cuentas SET saldo = saldo - 100 WHERE id = 9999;
UPDATE cuentas SET saldo = saldo + 100 WHERE id = 2;

-- 5. ¿Qué devolverá esta consulta?
SELECT * FROM cuentas;

ROLLBACK;
-- 6. ¿Qué devolverá esta consulta?
SELECT * FROM cuentas;

-- 7. Suponga que queremos realizar una transferencia de dinero entre dos cuentas bancarias con la siguiente transacción y la cuenta origen no tiene saldo:
START TRANSACTION;
UPDATE cuentas SET saldo = saldo - 100 WHERE id = 3;
UPDATE cuentas SET saldo = saldo + 100 WHERE id = 2;

-- 8. ¿Qué devolverá esta consulta?
SELECT * FROM cuentas;

ROLLBACK;
-- 9. ¿Qué devolverá esta consulta?
SELECT * FROM cuentas;
```

### Ejemplo 2
```sql
DROP DATABASE IF EXISTS test;
CREATE DATABASE test CHARACTER SET utf8mb4;
USE test;

CREATE TABLE producto (
  id INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
  nombre VARCHAR(100) NOT NULL
);

INSERT INTO producto (id, nombre) VALUES (1, 'Primero');
INSERT INTO producto (id, nombre) VALUES (2, 'Segundo');
INSERT INTO producto (id, nombre) VALUES (3, 'Tercero');

-- 1. Comprobamos las filas que existen en la tabla
SELECT * FROM producto;

-- 2. Ejecutamos una transacción que incluye un SAVEPOINT
START TRANSACTION;
INSERT INTO producto (id, nombre) VALUES (4, 'Cuarto');
SAVEPOINT sp1;
INSERT INTO producto (id, nombre) VALUES (5, 'Cinco');
INSERT INTO producto (id, nombre) VALUES (6, 'Seis');

-- 3. ¿Qué devolverá esta consulta?
SELECT * FROM producto;

ROLLBACK TO sp1;
-- 4. ¿Qué devolverá esta consulta?
SELECT * FROM producto;
RELEASE SAVEPOINT sp1;

```