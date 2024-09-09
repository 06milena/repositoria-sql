
# Proyecto de Biblioteca - Gestión de Base de Datos MySQL

Este proyecto tiene como objetivo crear y gestionar una base de datos relacional para una biblioteca utilizando MySQL. La base de datos contiene información sobre libros, autores, usuarios y préstamos, permitiendo realizar diferentes tipos de consultas y operaciones relacionadas con la gestión de una biblioteca.

## Descripción del Proyecto

El proyecto implementa una base de datos relacional con las siguientes tablas principales:

- **Autores**: Contiene la información de los autores de los libros (nombre, nacionalidad, fecha de nacimiento).
- **Libros**: Registra los libros disponibles en la biblioteca (título, autor, género, fecha de publicación).
- **Usuarios**: Almacena la información de los usuarios de la biblioteca (nombre, dirección, correo electrónico, fecha de registro).
- **Préstamos**: Registra los préstamos de libros realizados por los usuarios (usuario, libro, fecha de préstamo, fecha de devolución, estado del préstamo).

Además, se incluye una tabla adicional:
- **Categorías**: Para clasificar los libros en diferentes géneros o tipos.
- **Idioma**: Para almacenar el idioma de los libros.

### Funcionalidades Principales

- **Gestión de autores, libros, usuarios y préstamos**.
- **Consultas SQL avanzadas**: Permite realizar consultas para obtener información sobre los libros más prestados, usuarios con más préstamos, libros disponibles por autor, y mucho más.
- **Operaciones DML y DDL**: Se incluyen operaciones de inserción, actualización y eliminación de datos (DML), así como la creación y modificación de tablas y restricciones (DDL).

## Requisitos

- MySQL 5.7 o superior.
- Cliente MySQL o cualquier herramienta gráfica compatible con MySQL (como MySQL Workbench).

## Restaurar Base de Datos

Para restaurar la base de datos de la biblioteca, sigue estos pasos:

1. **Crear la Base de Datos**: 
   ```sql
   CREATE DATABASE biblioteca;
   ```

2. **Seleccionar la Base de Datos**:
   ```sql
   USE biblioteca;
   ```

3. **Crear las Tablas**: Copia y ejecuta las siguientes instrucciones SQL para crear las tablas `Autores`, `Libros`, `Usuarios`, `Préstamos`, y `Categorías`.

   ```sql
   -- Crear tabla Autores
   CREATE TABLE Autores (
       id_autor INT AUTO_INCREMENT PRIMARY KEY,
       nombre VARCHAR(100) NOT NULL,
       nacionalidad VARCHAR(50),
       fecha_nacimiento DATE
   );

   -- Crear tabla Libros
   CREATE TABLE Libros (
       id_libro INT AUTO_INCREMENT PRIMARY KEY,
       titulo VARCHAR(150) NOT NULL,
       id_autor INT,
       genero VARCHAR(50),
       fecha_publicacion DATE,
       num_paginas INT,
       stock INT,
       idioma VARCHAR(50) DEFAULT 'Español',
       id_categoria INT,
       FOREIGN KEY (id_autor) REFERENCES Autores(id_autor)
   );

   -- Crear tabla Usuarios
   CREATE TABLE Usuarios (
       id_usuario INT AUTO_INCREMENT PRIMARY KEY,
       nombre VARCHAR(150) NOT NULL,
       direccion VARCHAR(255),
       email VARCHAR(100) UNIQUE
   );

   -- Crear tabla Prestamos
   CREATE TABLE Prestamos (
       id_prestamo INT AUTO_INCREMENT PRIMARY KEY,
       id_usuario INT,
       id_libro INT,
       fecha_prestamo DATE DEFAULT CURRENT_DATE,
       fecha_devolucion DATE,
       estado ENUM('Pendiente', 'Devuelto') DEFAULT 'Pendiente',
       FOREIGN KEY (id_usuario) REFERENCES Usuarios(id_usuario),
       FOREIGN KEY (id_libro) REFERENCES Libros(id_libro)
   );

   -- Crear tabla Categorias
   CREATE TABLE Categorias (
       id_categoria INT AUTO_INCREMENT PRIMARY KEY,
       nombre_categoria VARCHAR(100) NOT NULL
   );
   ```

4. **Insertar Datos Iniciales**:
   Para poblar las tablas con datos iniciales, ejecuta las siguientes consultas:

   ```sql
   -- Insertar datos en Autores
   INSERT INTO Autores (nombre, nacionalidad, fecha_nacimiento) VALUES
   ('Gabriel García Márquez', 'Colombiano', '1927-03-06'),
   ('J.K. Rowling', 'Británica', '1965-07-31');

   -- Insertar datos en Libros
   INSERT INTO Libros (titulo, id_autor, genero, fecha_publicacion, num_paginas, stock) VALUES
   ('Cien años de soledad', 1, 'Novela', '1967-06-05', 417, 5),
   ('Harry Potter y la piedra filosofal', 2, 'Fantasía', '1997-06-26', 309, 7);

   -- Insertar datos en Usuarios
   INSERT INTO Usuarios (nombre, direccion, email) VALUES
   ('Juan Pérez', 'Calle 123', 'juan.perez@example.com'),
   ('Ana Gómez', 'Carrera 45', 'ana.gomez@example.com');

   -- Insertar datos en Prestamos
   INSERT INTO Prestamos (id_usuario, id_libro, fecha_devolucion) VALUES
   (1, 1, '2024-09-10'),
   (2, 2, '2024-09-15');

   -- Insertar datos en Categorías
   INSERT INTO Categorias (nombre_categoria) VALUES
   ('Fantasía'),
   ('Novela');
   ```

5. **Consultas Avanzadas**:
   Puedes realizar consultas avanzadas para obtener información sobre los libros, autores, usuarios y préstamos, como por ejemplo:
   
   ```sql
   -- Consultar los libros más prestados
   SELECT Libros.titulo, COUNT(Prestamos.id_prestamo) AS veces_prestado
   FROM Prestamos
   JOIN Libros ON Prestamos.id_libro = Libros.id_libro
   GROUP BY Libros.titulo
   ORDER BY veces_prestado DESC
   LIMIT 5;
   ```


## Conclusión

Este proyecto proporciona una estructura sólida para gestionar una biblioteca, permitiendo la integración de nuevas funcionalidades como categorías de libros, estadísticas de préstamos, y más. MySQL es la base de datos relacional que facilita la gestión eficiente de los datos y las consultas complejas para obtener información clave.
