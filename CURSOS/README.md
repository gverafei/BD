# Ejercicios
Prácticas en clase Base de datos

### Base de datos cursos
```
DROP DATABASE IF EXISTS cursos;
CREATE DATABASE cursos CHARACTER SET utf8mb4;
USE cursos;

CREATE TABLE alumnos (
  matricula INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
  nombre VARCHAR(100) NOT NULL,
  grupo VARCHAR(100) NOT NULL
);

CREATE TABLE curso (
  idcurso INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
  nombre VARCHAR(100) NOT NULL
);

CREATE TABLE practicas (
  matricula INT UNSIGNED,
  idcurso INT UNSIGNED,
  calificacion DOUBLE NOT NULL,
  CONSTRAINT fk_matricula FOREIGN KEY (matricula) REFERENCES alumnos(matricula) 
  ON DELETE CASCADE ON UPDATE CASCADE,
  CONSTRAINT fk_idcurso FOREIGN KEY (idcurso) REFERENCES curso(idcurso) 
  ON DELETE CASCADE ON UPDATE CASCADE,
  CONSTRAINT pk_pelicula_categoria PRIMARY KEY (matricula, idcurso)
);

INSERT INTO alumnos VALUES(1, 'Juan Perez', 'B101');
INSERT INTO alumnos VALUES(2, 'Rosita Chávez', 'B101');
INSERT INTO alumnos VALUES(3, 'Alejandra Rosas', 'C202');
INSERT INTO alumnos VALUES(4, 'Mauricio Ochoa', 'D301');

INSERT INTO curso VALUES(1, 'Base de datos');
INSERT INTO curso VALUES(2, 'Programación');
INSERT INTO curso VALUES(3, 'Redes');

INSERT INTO practicas VALUES(1, 1, 4);
INSERT INTO practicas VALUES(1, 2, 7);
INSERT INTO practicas VALUES(2, 1, 8);
INSERT INTO practicas VALUES(2, 3, 6);
INSERT INTO practicas VALUES(3, 1, 3);
INSERT INTO practicas VALUES(4, 1, 9);
INSERT INTO practicas VALUES(4, 2, 4);
INSERT INTO practicas VALUES(4, 3, 5);
```