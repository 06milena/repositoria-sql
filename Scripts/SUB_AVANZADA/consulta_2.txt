-- Obtener libros prestados actualmente
SELECT L.titulo, U.nombre AS usuario, P.fecha_prestamo
FROM Prestamos P
JOIN Libros L ON P.libro_id = L.libro_id
JOIN Usuarios U ON P.usuario_id = U.usuario_id
WHERE P.estado = 'prestado';

-- Contar el número de libros por género
SELECT genero, COUNT(*) AS cantidad_libros
FROM Libros
GROUP BY genero;
