# ENTREGABLE 02 😄
## DATA BASE CLINICA
### 1. Desarrolla programas de Transact SQL para la creación de una tienda virtual.
* Se crea procedimientos almacenados para ingresar, consultar, modificar y eliminar datos de la base, junto con funciones.
* En un programa Transact SQL, se desarrolla todo el proceso para condicionar los procedimientos almacenados necesarios dentro de la aplicación en la tienda virtual que se está desarrollando.

### 2. Realiza consultas avanzadas en BD para la generación de reportes estadísticos.

## PASOS PARA CREAR LOS PROCEDIMIENTOS ALMACENADOS EN LAS TABLAS
### 1. Crear un nuevo departamento
```sql
-- Crear nuevo departamento
CREATE PROCEDURE sp_InsertarDept
    @Dept_No INT,
    @DNombre VARCHAR(50),
    @Lec VARCHAR(50)
AS
BEGIN
    INSERT INTO Dept (Dept_No, DNombre, Lec)
    VALUES (@Dept_No, @DNombre, @Lec);
END;
```
### 2. Consultar departamento
```sql
-- Consultar departamento
CREATE PROCEDURE sp_ConsultarDept
AS
BEGIN
    SELECT * FROM Dept;
END;
```

### 3. Actualizar departamento
```sql
-- Actualizar departamento
CREATE PROCEDURE sp_ActualizarDept
    @Dept_No INT,
    @DNombre VARCHAR(50),
    @Lec VARCHAR(50)
AS
BEGIN
    UPDATE Dept
    SET DNombre = @DNombre, Lec = @Lec
    WHERE Dept_No = @Dept_No;
END;

```
### 4. Eliminar departamento
```sql
-- Eliminar departamento
CREATE PROCEDURE sp_EliminarDept
    @Dept_No INT
AS
BEGIN
    DELETE FROM Dept
    WHERE Dept_No = @Dept_No;
END;
```

## PROCEDIMIENTO ALMACENADO PARA LA TABLA "Emp"
### 1. Crear nuevo empleado
```sql
-- Crear nuevo empleado
CREATE PROCEDURE sp_InsertarEmp
    @Emp_No INT,
    @Apellido VARCHAR(50),
    @Oficio VARCHAR(50),
    @Dia INT,
    @Fecha_Alt DATE,
    @Salario DECIMAL(10, 2),
    @Comisión DECIMAL(10, 2),
    @Dept_No INT
AS
BEGIN
    INSERT INTO Emp (Emp_No, Apellido, Oficio, Dia, Fecha_Alt, Salario, Comisión, Dept_No)
    VALUES (@Emp_No, @Apellido, @Oficio, @Dia, @Fecha_Alt, @Salario, @Comisión, @Dept_No);
END;
```
### 2. Consultar empleado
```sql
-- Consultar empleados
CREATE PROCEDURE sp_ConsultarEmp
AS
BEGIN
    SELECT * FROM Emp;
END;

```
### 3. Actualizar empleado
```sql
-- Actualizar empleado
CREATE PROCEDURE sp_ActualizarEmp
    @Emp_No INT,
    @Apellido VARCHAR(50),
    @Oficio VARCHAR(50),
    @Dia INT,
    @Fecha_Alt DATE,
    @Salario DECIMAL(10, 2),
    @Comisión DECIMAL(10, 2),
    @Dept_No INT
AS
BEGIN
    UPDATE Emp
    SET Apellido = @Apellido, Oficio = @Oficio, Dia = @Dia, Fecha_Alt = @Fecha_Alt,
        Salario = @Salario, Comisión = @Comisión, Dept_No = @Dept_No
    WHERE Emp_No = @Emp_No;
END;
```
### 4. Eliminar empleado
```sql
-- Eliminar empleado
CREATE PROCEDURE sp_EliminarEmp
    @Emp_No INT
AS
BEGIN
    DELETE FROM Emp
    WHERE Emp_No = @Emp_No;
END;
```

## PROCEDIMIENTO ALMACENADO PARA LA TABLA "Enfermo"
### 1. Crear nuevo registro de enfermo
```sql
-- Crear nuevo registro de enfermo
CREATE PROCEDURE sp_InsertarEmfermo
    @Inscripción INT,
    @Apellido VARCHAR(50),
    @Dirección VARCHAR(100),
    @Fecha_Nac DATE,
    @S CHAR(1),
    @NSS VARCHAR(15)
AS
BEGIN
    INSERT INTO Emfermo (Inscripción, Apellido, Dirección, Fecha_Nac, S, NSS)
    VALUES (@Inscripción, @Apellido, @Dirección, @Fecha_Nac, @S, @NSS);
END;
```
### 2. Consultar registro de enfermos
```sql
-- Consultar registro de enfermos
CREATE PROCEDURE sp_ConsultarEmfermo
AS
BEGIN
    SELECT * FROM Emfermo;
END;
```
### 3. Actualizar registro de enfermo
```sql
-- Actualizar registro de enfermo
CREATE PROCEDURE sp_ActualizarEmfermo
    @Inscripción INT,
    @Apellido VARCHAR(50),
    @Dirección VARCHAR(100),
    @Fecha_Nac DATE,
    @S CHAR(1),
    @NSS VARCHAR(15)
AS
BEGIN
    UPDATE Emfermo
    SET Apellido = @Apellido, Dirección = @Dirección, Fecha_Nac = @Fecha_Nac,
        S = @S, NSS = @NSS
    WHERE Inscripción = @Inscripción;
END;

```
### 4. Eliminar registro de enfermo
```sql
-- Eliminar registro de enfermo
CREATE PROCEDURE sp_EliminarEmfermo
    @Inscripción INT
AS
BEGIN
    DELETE FROM Emfermo
    WHERE Inscripción = @Inscripción;
END;
```

## CONSULTAS AVANZADAS Y REPORTES ESTADISTICOS
### 1. Generar un reporte de empleados por departamento:
```sql
-- 1. Generar un reporte de empleados por departamento:
CREATE VIEW vw_EmpleadosPorDepto
AS
SELECT d.DNombre, e.Apellido, e.Oficio, e.Salario, e.Comisión
FROM Emp e
JOIN Dept d ON e.Dept_No = d.Dept_No;

-- cosultar vista
SELECT * FROM vw_EmpleadosPorDepto;
```

### 2. Generar un reporte de empleados enfermos:
```sql
-- 2. Generar un reporte de empleados enfermos:
CREATE VIEW vw_EmpleadosEnfermos
AS
SELECT e.Apellido AS Empleado, ef.Apellido AS Enfermo, ef.Dirección, ef.Fecha_Nac, ef.S, ef.NSS
FROM Emp e
JOIN Emfermo ef ON e.Apellido = ef.Apellido;

-- consultar vista
SELECT * FROM vw_EmpleadosEnfermos;
```

### 3. Generar reporte de empleados con salarios superiores a un valor específico:
```sql
 -- 3. Generar reporte de empleados con salarios superiores a un valor específico:
CREATE PROCEDURE sp_ReporteSalarioSuperior
    @SalarioMin DECIMAL(10, 2)
AS
BEGIN
    SELECT Apellido, Oficio, Salario
    FROM Emp
    WHERE Salario > @SalarioMin;
END;

-- ejemplo de uso de procedimiento
EXEC sp_ReporteSalarioSuperior @SalarioMin = 4000;
```
