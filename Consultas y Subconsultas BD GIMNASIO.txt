--Desarrollar y resolver 10 ejercicios con consultas a una sola tabla--

--1) Todos los socios que hoy están dados de alta como “ACTIVO”.
SELECT *
FROM SOCIO
WHERE ESTADO = 'ACTIVO';

--2) Listamos los socios en orden alfabético por apellido.
SELECT ID_SOCIO, NOMBRE, APELLIDO
FROM SOCIO
ORDER BY APELLIDO ASC;

--3) Cuántos socios hay cargados en el sistema en total.
SELECT 
    COUNT(*) AS TOTAL_SOCIOS
FROM SOCIO;

--4) Planes del gimnasio del más caro al más barato.
SELECT 
    ID_PLAN, 
    NOMBRE_PLAN, 
    CUOTA_MENSUAL
FROM
    PLAN
ORDER BY CUOTA_MENSUAL DESC;

--5) Planes que duran medio año o más.
SELECT 
    ID_PLAN, 
    NOMBRE_PLAN,
    DURACION_MESES
FROM PLAN
WHERE DURACION_MESES >= 6;

--6) Formas de pago que cobran un recargo.
SELECT 
    ID_METODO_PAGO, 
    NOMBRE_METODO, 
    RECARGO_PORCENTAJE
FROM METODO_PAGO
WHERE RECARGO_PORCENTAJE > 0;

--7) Pagos  realizados durante el año 2025.
SELECT 
    ID_PAGO, 
    ID_SOCIO, 
    FECHA_PAGO, 
    IMPORTE, 
    ESTADO_PAGO
FROM PAGO_CUOTA
WHERE ESTADO_PAGO = 'PAGADO'
  AND FECHA_PAGO >= '2025-01-01'
  AND FECHA_PAGO <= '2025-12-31';

--8) Sirve para ver los 10 pagos más recientes cargados.
SELECT 
    ID_PAGO, 
    ID_SOCIO,   
    FECHA_PAGO,     
    IMPORTE, 
    ESTADO_PAGO
FROM PAGO_CUOTA
ORDER BY FECHA_PAGO DESC
LIMIT 10;

--9) Vemos qué actividades aceptan más gente, ordenadas del cupo más grande al más chico.
SELECT 
    ID_ACTIVIDAD, 
    NOMBRE_ACTIVIDAD, 
    CUPO_MAXIMO
FROM ACTIVIDAD
ORDER BY CUPO_MAXIMO DESC;

--10) Inscripciones de socios a actividades están marcadas como “ACTIVO”.
SELECT 
    ID_SOCIO, 
    ID_ACTIVIDAD, 
    FECHA_INSCRIPCION,     
    ESTADO
FROM SOCIO_ACTIVIDAD
WHERE ESTADO = 'ACTIVO';










--Desarrollar y resolver 10 ejercicios con consultas a múltiples tablas--


-- 1) Mostrar, para cada pago realizado, el nombre del plan y el método de pago utilizado

SELECT
    p.ID_PAGO,
    pl.NOMBRE_PLAN,
    mp.NOMBRE_METODO,
    p.IMPORTE
FROM PAGO_CUOTA p
INNER JOIN PLAN pl
      ON p.ID_PLAN = pl.ID_PLAN
INNER JOIN METODO_PAGO mp
      ON p.ID_METODO_PAGO = mp.ID_METODO_PAGO;


-- 2) Lista  socios y  planes que pagaron al menos una vez mostrando solo pagos con estado PAGADO

SELECT DISTINCT
    s.ID_SOCIO,
    s.NOMBRE,
    s.APELLIDO,
    pl.NOMBRE_PLAN
FROM PAGO_CUOTA p
INNER JOIN SOCIO s   
    ON p.ID_SOCIO = s.ID_SOCIO
INNER JOIN PLAN pl   
    ON p.ID_PLAN  = pl.ID_PLAN
WHERE p.ESTADO_PAGO = 'PAGADO';


-- 3) Pagos indicando el método de pago usado

SELECT
    p.ID_PAGO,
    s.NOMBRE,
    s.APELLIDO,
    mp.NOMBRE_METODO,
    p.IMPORTE,
    p.ESTADO_PAGO
FROM PAGO_CUOTA p
INNER JOIN SOCIO s        
    ON p.ID_SOCIO = s.ID_SOCIO
INNER JOIN METODO_PAGO mp 
    ON p.ID_METODO_PAGO = mp.ID_METODO_PAGO
WHERE p.ESTADO_PAGO = 'PAGADO';


-- 4) Listamos los socios y las actividades en las que están inscriptos
SELECT
    s.ID_SOCIO,
    s.NOMBRE,
    s.APELLIDO,
    a.NOMBRE_ACTIVIDAD,
    sa.FECHA_INSCRIPCION,
    sa.ESTADO
FROM SOCIO_ACTIVIDAD sa
INNER JOIN SOCIO s     
    ON sa.ID_SOCIO     = s.ID_SOCIO
INNER JOIN ACTIVIDAD a 
    ON sa.ID_ACTIVIDAD = a.ID_ACTIVIDAD
WHERE sa.ESTADO = 'ACTIVO';


-- 5) Cuántas actividades tiene cada socio
SELECT
    s.ID_SOCIO,
    s.NOMBRE,
    s.APELLIDO,
    COUNT(sa.ID_ACTIVIDAD) AS CANTIDAD_ACTIVIDADES
FROM SOCIO s
LEFT JOIN SOCIO_ACTIVIDAD sa 
    ON s.ID_SOCIO = sa.ID_SOCIO
GROUP BY
    s.ID_SOCIO,
    s.NOMBRE,
    s.APELLIDO;



-- 6) Ver los pagos realizados por los socios con su plan correspondiente, ordenados del más reciente al más antiguo.
SELECT
    s.ID_SOCIO,
    s.NOMBRE,
    s.APELLIDO,
    pl.NOMBRE_PLAN,
    p.PERIODO_DESDE,
    p.PERIODO_HASTA,
    p.FECHA_PAGO,
    p.ESTADO_PAGO
FROM PAGO_CUOTA p
INNER JOIN SOCIO s 
    ON p.ID_SOCIO = s.ID_SOCIO
INNER JOIN PLAN pl 
    ON p.ID_PLAN  = pl.ID_PLAN
WHERE p.ESTADO_PAGO = 'PAGADO'
ORDER BY p.FECHA_PAGO DESC;


-- 7) Total de dinero pagado por cada socio de mayor a menor.
SELECT
    s.ID_SOCIO,
    s.NOMBRE,
    s.APELLIDO,
    SUM(p.IMPORTE) AS TOTAL_PAGADO
FROM SOCIO s
INNER JOIN PAGO_CUOTA p 
    ON s.ID_SOCIO = p.ID_SOCIO
WHERE p.ESTADO_PAGO = 'PAGADO'
GROUP BY
    s.ID_SOCIO,
    s.NOMBRE,
    s.APELLIDO
ORDER BY TOTAL_PAGADO DESC;


-- 8) Cuánto se recaudó por cada plan de menor a mayor.
SELECT
    pl.ID_PLAN,
    pl.NOMBRE_PLAN,
    SUM(p.IMPORTE) AS TOTAL_RECAUDADO
FROM PLAN pl
INNER JOIN PAGO_CUOTA p 
    ON pl.ID_PLAN = p.ID_PLAN
WHERE p.ESTADO_PAGO = 'PAGADO'
GROUP BY
    pl.ID_PLAN,
    pl.NOMBRE_PLAN
ORDER BY TOTAL_RECAUDADO ASC;


-- 9) Métodos de pago usa cada socio.
SELECT DISTINCT
    s.ID_SOCIO,
    s.NOMBRE,
    s.APELLIDO,
    mp.NOMBRE_METODO
FROM PAGO_CUOTA p
INNER JOIN SOCIO s        
    ON p.ID_SOCIO = s.ID_SOCIO
INNER JOIN METODO_PAGO mp 
    ON p.ID_METODO_PAGO = mp.ID_METODO_PAGO;


-- 10) Cuántos socios tiene cada actividad (solo inscripciones activas)
SELECT
    a.ID_ACTIVIDAD,
    a.NOMBRE_ACTIVIDAD,
    COUNT(sa.ID_SOCIO) AS CANT_SOCIOS_ACTIVOS
FROM ACTIVIDAD a
LEFT JOIN SOCIO_ACTIVIDAD sa
       ON a.ID_ACTIVIDAD = sa.ID_ACTIVIDAD
      AND sa.ESTADO = 'ACTIVO'
GROUP BY
    a.ID_ACTIVIDAD,
    a.NOMBRE_ACTIVIDAD;
    





--Desarrollar y resolver 5 ejercicios a través de subconsultas--

-- 1) Socios que pagaron más que el promedio de todos los pagos


SELECT DISTINCT
    s.ID_SOCIO,
    s.NOMBRE,
    s.APELLIDO,
    p.IMPORTE,
    (
        SELECT AVG(IMPORTE)
        FROM PAGO_CUOTA
        WHERE ESTADO_PAGO = 'PAGADO'
    ) AS PROMEDIO_IMPORTE_PAGADO
FROM SOCIO s
INNER JOIN PAGO_CUOTA p
    ON s.ID_SOCIO = p.ID_SOCIO
WHERE p.ESTADO_PAGO = 'PAGADO'
  AND p.IMPORTE > (
        SELECT AVG(IMPORTE)
        FROM PAGO_CUOTA
        WHERE ESTADO_PAGO = 'PAGADO'
      );



-- 2) Socios que alguna vez pagaron el plan "Mensual Full"

SELECT DISTINCT
    s.ID_SOCIO,
    s.NOMBRE,
    s.APELLIDO
FROM SOCIO s
INNER JOIN PAGO_CUOTA p
    ON s.ID_SOCIO = p.ID_SOCIO
WHERE p.ID_PLAN IN (
        SELECT ID_PLAN
        FROM PLAN
        WHERE NOMBRE_PLAN = 'Mensual Full'
      );



-- 3) Planes más caros que el promedio.

SELECT
    ID_PLAN,
    NOMBRE_PLAN,
    CUOTA_MENSUAL,
    (
        SELECT AVG(CUOTA_MENSUAL)
        FROM PLAN
    ) AS PROMEDIO_CUOTA_MENSUAL
FROM PLAN
WHERE CUOTA_MENSUAL > (
        SELECT AVG(CUOTA_MENSUAL)
        FROM PLAN
      );

-- 4) Socios anotados en actividades con mucho cupo (mas de 30)


SELECT DISTINCT
    s.ID_SOCIO,
    s.NOMBRE,
    s.APELLIDO
FROM SOCIO s
INNER JOIN SOCIO_ACTIVIDAD sa
    ON s.ID_SOCIO = sa.ID_SOCIO
WHERE sa.ID_ACTIVIDAD IN (
        SELECT ID_ACTIVIDAD
        FROM ACTIVIDAD
        WHERE CUPO_MAXIMO > 30
      );



-- 5) Pago mas grande que alguna vez hubo en el gimansio


SELECT
    ID_PAGO,
    ID_SOCIO,
    IMPORTE,
    FECHA_PAGO
FROM PAGO_CUOTA
WHERE ESTADO_PAGO = 'PAGADO'
  AND IMPORTE = (
        SELECT MAX(IMPORTE)
        FROM PAGO_CUOTA
        WHERE ESTADO_PAGO = 'PAGADO'
      );

