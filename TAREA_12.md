# TAREA 12 😄
## Ejercicios de Consultas SQL
*Ordenar de menor a mayor y dar formato en soles en todas las consulas que tienen un valor en ventas.*

### 1. Total de ventas en el año 2009:

¿Cuál es el total de ventas realizadas en el año 2009?
```sql
declare @fecha_inicio_2009 datetime= '2009-01-01'
declare @fecha_fin_2009 datetime = '2009-12-31'

select FORMAT(sum(total),'C', 'es-PE') AS Total_Ventas
from ve.documento
where fechaMovimiento between @fecha_inicio_2009 and @fecha_fin_2009
```
![texto_alternativo](IMG/01_tarea.png)


### 2. Personas sin entradas registradas en la tabla personaDestino:
¿Cuáles son las personas que no tienen una entrada registrada en la tabla personaDestino?
```sql
select p.*
from ma.persona p
left join ma.personaDestino pd on p.persona = pd.persona
where pd.persona is null
```
![texto_alternativo](IMG/02_tarea.png)

### 3. Promedio del monto total de transacciones de ventas:
¿Cuál es el promedio del monto total de todas las transacciones de ventas registradas en la base de datos, expresado en moneda local (soles peruanos)?
```sql
select FORMAT(AVG(total), 'C', 'es-PE') AS "Promedio del total de ventas"
from ve.documento
```
![texto_alternativo](IMG/03_tarea.png)

### 4. Documentos de ventas con monto total superior al promedio:
Obtén una lista de todos los documentos de ventas cuyo monto total supere el promedio del monto total de todos los documentos de ventas registrados en la base de datos.
```sql
select *
from ve.documento
where total > (select AVG(total) from ve.documento)
ORDER BY documento ASC
```
![texto_alternativo](IMG/04_tarea.png)

### 5. Documentos de ventas pagados con una forma de pago específica:
Listar los documentos de ventas que han sido pagados utilizando una forma de pago específica desde la tabla documentoPago.
```sql
select d.*
from ve.documentoPago dp
inner join ve.documento d on dp.documento = d.documento
inner join pa.pago p on dp.pago = p.pago
where p.formaPago = 1
ORDER BY documento ASC
```
![texto_alternativo](IMG/05_tarea.png)


### 6.	Detalles de documentos de ventas canjeados:
¿Cómo se distribuye el saldo total entre los diferentes almacenes, considerando la información de los saldos iniciales de inventario en la base de datos?
```sql
SELECT almacen, FORMAT(SUM(costoSoles), 'C', 'es-PE') AS Saldo_Total
FROM ma.saldosIniciales
GROUP BY almacen
ORDER BY SUM(costoSoles) ASC;
```
![texto_alternativo](IMG/06_tarea.png)


### 7.	Saldo total distribuido por almacén:
Obtén una lista de todos los documentos de ventas cuyo monto total supere el promedio del monto total de todos los documentos de ventas registrados en la base de datos.
```sql
SELECT *
FROM ve.documento
WHERE total > (SELECT AVG(total)
FROM ve.documento)
ORDER BY documento ASC;
```
![texto_alternativo](IMG/07_tarea.png)


### 8.	Detalles de documentos de ventas por vendedor:
¿Cuáles son los detalles de los documentos de ventas asociados al vendedor con identificación número 3 en la base de datos, considerando la información detallada de cada documento en relación con sus elementos de venta?
```sql
SELECT d.*
FROM ve.documento d
INNER JOIN ve.documentoDetalle dd ON d.documento = dd.documento
WHERE d.vendedor = 3;
```
![texto_alternativo](IMG/08_tarea.png)


### 9.	Total de ventas por año y vendedor:
¿Cuál es el total de ventas por año y vendedor en la base de datos de ventas, considerando solo aquellos vendedores cuya suma total de ventas en un año específico sea superior a 100,000 unidades monetarias?
```sql
SELECT vendedor, YEAR(fechaMovimiento) AS Anio, FORMAT(SUM(total),'C', 'es-PE') AS Total_Ventas
FROM ve.documento
GROUP BY vendedor, YEAR(fechaMovimiento)
HAVING SUM(total) > 100000
ORDER BY Total_Ventas ASC
```
![texto_alternativo](IMG/09_tarea.png)


### 10.	Desglose mensual de ventas por vendedor:
¿Cuál es el desglose mensual de las ventas por vendedor en cada año, considerando la suma total de ventas para cada mes y año específico?
```sql
SELECT vendedor, MONTH(fechaMovimiento) AS Anio, FORMAT(SUM(total), 'C', 'es-PE') AS Total_Ventas
FROM ve.documento
GROUP BY vendedor, MONTH(fechaMovimiento)
HAVING SUM(total) > 100000
ORDER BY SUM(total) DESC;
```
![texto_alternativo](IMG/10_tarea.png)


### 11.	Clientes que compraron más de 10 veces en un año:
¿Cuántos clientes compraron más de 10 veces en un año?
```sql
SELECT persona, YEAR(fechaMovimiento) AS año, COUNT(*) AS Compras
FROM ve.documento
WHERE tipoMovimiento = 1
GROUP BY persona, YEAR(fechaMovimiento)
HAVING COUNT(*) > 10
ORDER BY año ASC
```
![texto_alternativo](IMG/11_tarea.png)


### 12.	Total acumulado de descuentos por vendedor:
¿Cuál es el total acumulado de descuentos aplicados por cada vendedor en la base de datos de ventas, considerando la suma de los descuentos descto01, descto02 y descto03, y mostrando solo aquellos vendedores cuyo total de descuentos acumulados supere los 5000?
```sql
SELECT vendedor, FORMAT(SUM(descto01 + descto02 + descto03), 'C', 'es-PE') AS Descuentos_Acumulados
FROM ve.documento
GROUP BY vendedor
HAVING SUM(descto01 + descto02 + descto03) > 5000
ORDER BY vendedor ASC;
```
![texto_alternativo](IMG/12_tarea.png)


### 13.	Total anual de ventas por persona:
¿Cuál es el total anual de ventas realizadas por cada persona en la base de datos de ventas, considerando únicamente los movimientos de tipo venta (tipoMovimiento = 1), y mostrando solo aquellas personas cuyas ventas anuales superen los 10000?
```sql
SELECT persona, YEAR(fechaMovimiento) AS Año, FORMAT(SUM(total),'C', 'es-PE') AS Total_Anual
FROM ve.documento
WHERE tipoMovimiento = 1
GROUP BY persona, YEAR(fechaMovimiento)
HAVING SUM(total) > 10000
ORDER BY Año ASC
```
![texto_alternativo](IMG/13_tarea.png)


### 14.	Recuento total de productos vendidos por vendedor:
¿Cuál es el recuento total de productos vendidos por cada vendedor en la base de datos de ventas?
```sql
SELECT 
    d.vendedor, 
    COUNT(dd.documentoDetalle) AS Total_Productos_Vendidos
FROM 
    ve.documentoDetalle dd
JOIN 
    ve.documento d ON dd.documento = d.documento
GROUP BY 
    d.vendedor
ORDER BY Total_Productos_Vendidos ASC
```
![texto_alternativo](IMG/14_tarea.png)


### 15.	Ventas mensuales desglosadas por tipo de pago:
¿Cuánto se vendió cada mes del año 2009, desglosado por tipo de pago?
```sql
SELECT 
    MONTH(d.fechaMovimiento) AS Mes,
    p.formaPago,
    FORMAT(SUM(d.total),'C', 'es-PE') AS Total_Ventas
FROM 
    ve.documento d
JOIN 
    pa.pago p ON d.vendedor = p.vendedor
WHERE 
    YEAR(d.fechaMovimiento) = 2009
GROUP BY 
    MONTH(d.fechaMovimiento),
    p.formaPago
ORDER BY Mes ASC
```
![texto_alternativo](IMG/01_tarea.png)


### 16.	Total de ventas en el año 2007:
¿Cuál es el total de ventas realizadas en el año 2007?
```sql
declare @fecha_inicio_2007 datetime= '2007-01-01'
declare @fecha_fin_2007 datetime = '2007-12-31'

select FORMAT(sum(total),'C', 'es-PE') AS Total_Ventas
from ve.documento
where fechaMovimiento between @fecha_inicio_2007 and @fecha_fin_2007
```
![texto_alternativo](IMG/16_tarea.png)


### 17.	Personas sin entradas registradas en la tabla personaDestino en el año 2008:
¿Cuáles son las personas que no tienen una entrada registrada en la tabla personaDestino en el año 2008?
```sql
DECLARE @fecha_inicio_2008 DATETIME = '2008-01-01';

SELECT p.*
FROM ma.persona p
LEFT JOIN ma.personaDestino pd ON p.persona = pd.persona AND YEAR(@fecha_inicio_2008) = 2008
WHERE pd.persona IS NULL;
```
![texto_alternativo](IMG/17_tarea.png)


### 18.	Promedio del monto total de transacciones de ventas en el año 2009:
¿Cuál es el promedio del monto total de todas las transacciones de ventas registradas en la base de datos en el año 2009, expresado en moneda local (soles peruanos)?
```sql
SELECT FORMAT(AVG(total), 'C', 'es-PE') AS "Promedio del total de ventas"
FROM ve.documento
WHERE YEAR(fechaMovimiento) = 2009;
```
![texto_alternativo](IMG/18_tarea.png)


### 19.	Documentos de ventas con monto total superior al promedio en el año 2005:
Obtén una lista de todos los documentos de ventas cuyo monto total supere el promedio del monto total de todos los documentos de ventas registrados en la base de datos en el año 2005.
```sql
SELECT *
FROM ve.documento
WHERE total > (
    SELECT AVG(total)
    FROM ve.documento
    WHERE YEAR(fechaMovimiento) = 2005
)
ORDER BY documento ASC
```
![texto_alternativo](IMG/01_tarea.png)


### 20.	Documentos de ventas pagados con una forma de pago específica en el año 2006:
Listar los documentos de ventas que han sido pagados utilizando una forma de pago específica desde la tabla documentoPago en el año 2006.
```sql
SELECT d.*
FROM ve.documento d
JOIN pa.pago p ON d.vendedor = p.vendedor
WHERE YEAR(d.fechaMovimiento) = 2006 AND p.formaPago = 1;
```
![texto_alternativo](IMG/20_tarea.png)


### 21.	Detalles de documentos de ventas canjeados en el año 2007:
¿Cómo se distribuye el saldo total entre los diferentes almacenes, considerando la información de los saldos iniciales de inventario en la base de datos en el año 2007?
```sql
DECLARE @fecha_inicio_2007 DATETIME = '2007-01-01';

SELECT almacen, FORMAT(SUM(costoSoles), 'C', 'es-PE') AS Saldo_Total
FROM [ma].[saldosIniciales]
WHERE YEAR(@fecha_inicio_2007) = 2007 GROUP BY almacen
ORDER BY almacen
```
![texto_alternativo](IMG/21_tarea.png)


### 22.	Saldo total distribuido por almacén en el año 2008:
Obtén una lista de todos los documentos de ventas cuyo monto total supere el promedio del monto total de todos los documentos de ventas registrados en la base de datos en el año 2008.
```sql
SELECT *
FROM ve.documento
WHERE total > (SELECT AVG(total)
FROM ve.documento
WHERE YEAR(fechaMovimiento) = 2008);
```
![texto_alternativo](IMG/22_tarea.png)


### 23.	Detalles de documentos de ventas por vendedor en el año 2009:
¿Cuáles son los detalles de los documentos de ventas asociados al vendedor con identificación número 3 en la base de datos en el año 2009, considerando la información detallada de cada documento en relación con sus elementos de venta?
```sql
SELECT d.*
FROM ve.documento d
JOIN ve.documentoDetalle dd
ON d.documento = dd.documento
WHERE d.vendedor = 3;
```
![texto_alternativo](IMG/23_tarea.png)


### 24.	Total de ventas por año y vendedor en el año 2008:
¿Cuál es el total de ventas por año y vendedor en la base de datos de ventas, considerando solo aquellos vendedores cuya suma total de ventas en el año 2008 sea superior a 100,000 unidades monetarias?
```sql
SELECT vendedor, YEAR(fechaMovimiento) AS Año, FORMAT(SUM(total),'C', 'es-PE') AS Total_Ventas
FROM ve.documento
WHERE YEAR(fechaMovimiento) = 2008
GROUP BY vendedor, YEAR(fechaMovimiento)
HAVING SUM(total) > 100000
ORDER BY vendedor ASC
```
![texto_alternativo](IMG/24_tarea.png)


### 25.	Desglose mensual de ventas por vendedor en el año 2009:
¿Cuál es el desglose mensual de las ventas por vendedor en cada año, considerando la suma total de ventas para cada mes y año específico en el año 2009?
```sql
SELECT vendedor, MONTH(fechaMovimiento) AS Mes, FORMAT(SUM(total), 'C', 'es-PE') AS Total_Ventas
FROM ve.documento
WHERE YEAR(fechaMovimiento) = 2009
GROUP BY vendedor, MONTH(fechaMovimiento)
ORDER BY Total_Ventas ASC
```
![texto_alternativo](IMG/25_tarea.png)


### 26.	Clientes que compraron más de 10 veces en un año en el año 2005:
¿Cuántos clientes compraron más de 10 veces en un año en el año 2005?
```sql
SELECT persona, YEAR(fechaMovimiento) AS Año, COUNT(*) AS Compras
FROM ve.documento
WHERE tipoMovimiento = 1 AND YEAR(fechaMovimiento) = 2005
GROUP BY persona, YEAR(fechaMovimiento) HAVING COUNT(*) > 5;
```
![texto_alternativo](IMG/26_tarea.png)


### 27.	Total acumulado de descuentos por vendedor en el año 2006:
¿Cuál es el total acumulado de descuentos aplicados por cada vendedor en la base de datos de ventas, considerando la suma de los descuentos descto01, descto02 y descto03, y mostrando solo aquellos vendedores cuyo total de descuentos acumulados supere los 5000 en el año 2005?
```sql
SELECT vendedor, FORMAT(SUM(descto01 + descto02 + descto03),'C', 'es-PE') AS Descuentos_Acumulados
FROM ve.documento
WHERE YEAR(fechaMovimiento) = 2005
GROUP BY vendedor HAVING SUM(descto01 + descto02 + descto03) > 5000
ORDER BY Descuentos_Acumulados ASC
```
![texto_alternativo](IMG/27_tarea.png)


### 28.	Total anual de ventas por persona en el año 2007:
¿Cuál es el total anual de ventas realizadas por cada persona en la base de datos de ventas, considerando únicamente los movimientos de tipo venta (tipoMovimiento = 1), y mostrando solo aquellas personas cuyas ventas anuales superen los 10000 en el año 2007?
```sql
SELECT persona, YEAR(fechaMovimiento) AS Año, FORMAT(SUM(total),'C', 'es-PE') AS Total_Ventas
FROM ve.documento
WHERE tipoMovimiento = 1 AND YEAR(fechaMovimiento) = 2007
GROUP BY persona, YEAR(fechaMovimiento) HAVING SUM(total) > 10000
ORDER BY Total_Ventas ASC
```
![texto_alternativo](IMG/28_tarea.png)


### 29.	Recuento total de productos vendidos por vendedor en el año 2008:
¿Cuál es el recuento total de productos vendidos por cada vendedor en la base de datos de ventas en el año 2008?
```sql
SELECT vendedor, COUNT(*) AS Total_Productos_Vendidos
FROM ve.documento
WHERE YEAR(fechaMovimiento) = 2008
GROUP BY vendedor
ORDER BY Total_Productos_Vendidos ASC
```
![texto_alternativo](IMG/29_tarea.png)


### 30.	Ventas mensuales desglosadas por tipo de pago en el año 2009:
¿Cuánto se vendió cada mes del año 2009, desglosado por tipo de pago?
```sql
SELECT MONTH(fechaMovimiento) AS Mes, p.formaPago, FORMAT(SUM(total),'C', 'es-PE') AS Total_Ventas
FROM ve.documento d
JOIN pa.pago p ON d.vendedor = p.vendedor
WHERE YEAR(fechaMovimiento) = 2009
GROUP BY MONTH(fechaMovimiento), p.formaPago
ORDER BY Mes ASC
```
![texto_alternativo](IMG/30_tarea.png)

