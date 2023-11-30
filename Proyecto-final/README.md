# Proyecto final Bases de Datos.
Código en Lenguaje de Definición de Datos DDL

### Base de datos FEI
DDL
```sql
DROP DATABASE IF EXISTS FEI;
CREATE DATABASE FEI CHARACTER SET utf8mb4;
USE FEI;

CREATE TABLE IF NOT EXISTS area (
  cod_area INT UNSIGNED PRIMARY KEY, 
  nombre VARCHAR(60) NOT NULL
);

CREATE TABLE IF NOT EXISTS carrera (
  cod_carrera INT UNSIGNED PRIMARY KEY,
  nombre VARCHAR(60) NOT NULL,
  descripcion VARCHAR(255) NOT NULL
);

CREATE TABLE IF NOT EXISTS materia (
  cod_materia INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
  cod_carrera INT UNSIGNED NOT NULL,
  cod_area INT UNSIGNED NOT NULL,
  nombre VARCHAR(60) NOT NULL,
  descripcion VARCHAR(255) NOT NULL,
  creditos INT UNSIGNED NOT NULL,
  bloque INT UNSIGNED NOT NULL,
  CONSTRAINT fk_cod_carrera FOREIGN KEY (cod_carrera) REFERENCES carrera(cod_carrera) ON DELETE CASCADE ON UPDATE CASCADE,
  CONSTRAINT fk_cod_area FOREIGN KEY (cod_area) REFERENCES area(cod_area) ON DELETE CASCADE ON UPDATE CASCADE
);

CREATE TABLE IF NOT EXISTS grado_estudios (
  cod_grado INT UNSIGNED PRIMARY KEY,
  nombre VARCHAR(60) NOT NULL
);

CREATE TABLE IF NOT EXISTS profesor (
  num_personal INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
  cod_grado INT UNSIGNED NOT NULL,
  nombre VARCHAR(60) NOT NULL,
  paterno VARCHAR(60) NOT NULL,
  materno VARCHAR(60) NOT NULL,
  email VARCHAR(30) NOT NULL,
  CONSTRAINT fk_cod_grado FOREIGN KEY (cod_grado) REFERENCES grado_estudios(cod_grado) ON DELETE CASCADE ON UPDATE CASCADE
);

CREATE TABLE IF NOT EXISTS periodo (
  cod_periodo INT UNSIGNED PRIMARY KEY,
  nombre VARCHAR(60) NOT NULL,
  fecha_ini DATE NOT NULL,
  fecha_fin DATE NOT NULL,
  CONSTRAINT chk_fecha_ini CHECK (fecha_ini < fecha_fin),
  CONSTRAINT chk_fecha_fin CHECK (fecha_fin > fecha_ini)
);

  CREATE TABLE IF NOT EXISTS alumno (
    matricula VARCHAR(12) PRIMARY KEY,
    nombre VARCHAR(60) NOT NULL,
    paterno VARCHAR(60) NOT NULL,
    materno VARCHAR(60) NOT NULL,
    email VARCHAR(30) NOT NULL,
    fecha_nac DATE
  );

CREATE TABLE IF NOT EXISTS oferta_academica (
  nrc INT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
  cod_materia INT UNSIGNED NOT NULL,
  num_personal INT UNSIGNED NOT NULL,
  cod_periodo INT UNSIGNED NOT NULL,
  CONSTRAINT fk_cod_materia FOREIGN KEY (cod_materia) REFERENCES materia(cod_materia) ON DELETE CASCADE ON UPDATE CASCADE,
  CONSTRAINT fk_num_personal FOREIGN KEY (num_personal) REFERENCES profesor(num_personal) ON DELETE CASCADE ON UPDATE CASCADE,
  CONSTRAINT fk_cod_periodo FOREIGN KEY (cod_periodo) REFERENCES periodo(cod_periodo) ON DELETE CASCADE ON UPDATE CASCADE
);

CREATE TABLE IF NOT EXISTS carga_academica (
  matricula VARCHAR(12) NOT NULL,
  nrc INT UNSIGNED NOT NULL,
  calificacion FLOAT(4,2) NOT NULL,
  PRIMARY KEY(matricula, nrc),
  CONSTRAINT fk_nrc FOREIGN KEY (nrc) REFERENCES oferta_academica(nrc) ON DELETE CASCADE ON UPDATE CASCADE
);
```

### Base de datos FEI
DML
```sql
```