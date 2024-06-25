# TAREA 14 😄
## Data Base "DBVENTAS"
## En la tabla DBVENTAS aplicar los procedimientos almacenado y Trigger
### Ejemplo con la tabla DISTRITO
### 1. Procedimiento almacenado para la tabla distrito
#### Procedimiento para Agregar Distrito
```sql
-- Agregar distrito
CREATE PROCEDURE sp_AgregarDistrito
    @COD_DIS CHAR(5),
    @NOM_DIS VARCHAR(50)
AS
BEGIN
    INSERT INTO DISTRITO (COD_DIS, NOM_DIS)
    VALUES (@COD_DIS, @NOM_DIS);
END

EXEC sp_AgregarDistrito @COD_DIS = 'D37', @NOM_DIS = 'ATE';
```
#### Procedimiento para Actualizar Distrito
```sql
-- Actualizar Distrito
CREATE PROCEDURE sp_ActualizarDistrito
    @COD_DIS CHAR(5),
    @NOM_DIS VARCHAR(50)
AS
BEGIN
    UPDATE DISTRITO
    SET NOM_DIS = @NOM_DIS
    WHERE COD_DIS = @COD_DIS;
END;
GO

EXEC sp_ActualizarDistrito @COD_DIS = 'D37', @NOM_DIS = 'SURCO';
```

#### Procedimiento para Eliminar Distrito
```sql
-- Eliminar distrito
CREATE PROCEDURE sp_EliminarDistrito
    @COD_DIS CHAR(5)
AS
BEGIN
    DELETE FROM DISTRITO
    WHERE COD_DIS = @COD_DIS;
END;
GO

EXEC sp_EliminarDistrito @COD_DIS = 'D37';
```

### Crear Trigger para "Distrito"
```sql
-- TRIGGER DISTRITO
CREATE TRIGGER TX_MENSAJE_DISTRITO
ON DISTRITO 
FOR INSERT, UPDATE
AS
PRINT 'MENSAJE DISPARADO POR LA INSERCION 
  O ACTUALIZACION DE LA TABLA DISTRITO'
GO

INSERT INTO DISTRITO (COD_DIS, NOM_DIS)
VALUES ('D01', 'SURCO')
GO

UPDATE DISTRITO
SET NOM_DIS = 'SAN BORJA'
WHERE COD_DIS = 'D01'
GO
```

### 2. Procedimiento almacenado para la tabla Abastecimiento
#### Procedimiento para Agregar Abastecimiento
```sql
-- Agregar abastecimiento
CREATE PROCEDURE sp_AgregarAbastecimiento
    @COD_PRV CHAR(5),
    @COD_PRO CHAR(5),
    @PRE_ABA MONEY
AS
BEGIN
    INSERT INTO ABASTECIMIENTO (COD_PRV, COD_PRO, PRE_ABA)
    VALUES (@COD_PRV, @COD_PRO, @PRE_ABA);
END

EXEC sp_AgregarAbastecimiento @COD_PRV = 'PR21', @COD_PRO = 'P021', @PRE_ABA = 111.0000;
```
#### Procedimiento para Actualizar Abastecimiento
```sql
-- Actualizar Abastecimiento
CREATE PROCEDURE sp_ActualizarAbastecimiento
    @COD_PRV CHAR(5),
    @COD_PRO CHAR(5),
    @PRE_ABA MONEY
AS
BEGIN
    UPDATE ABASTECIMIENTO
    SET PRE_ABA = @PRE_ABA
    WHERE COD_PRV = @COD_PRV AND COD_PRO = @COD_PRO;
END;
GO

EXEC sp_ActualizarAbastecimiento @COD_PRV = 'PR21', @COD_PRO = 'P022', @PRE_ABA = 222.0000;
```

#### Procedimiento para Eliminar Abastecimiento
```sql
-- Eliminar Abastecimiento
CREATE PROCEDURE sp_EliminarAbastecimiento
    @COD_PRV CHAR(5),
    @COD_PRO CHAR(5)
AS
BEGIN
    DELETE FROM ABASTECIMIENTO
    WHERE COD_PRV = @COD_PRV AND COD_PRO = @COD_PRO;
END;
GO

EXEC sp_EliminarAbastecimiento @COD_PRV = 'PR21', @COD_PRO = 'P021';
```

### Crear Trigger para "Abastecimiento"
```sql
-- Trigger Abastecimiento
CREATE TRIGGER TX_MENSAJE_ABASTECIMIENTO
ON ABASTECIMIENTO 
FOR INSERT, UPDATE
AS
PRINT 'MENSAJE DISPARADO POR LA INSERCION 
  O ACTUALIZACION DE LA TABLA ABASTECIMIENTO'
GO

INSERT INTO ABASTECIMIENTO (COD_PRV, COD_PRO, PRE_ABA)
VALUES ('PR21', 'P021', 111.0000)
GO

UPDATE ABASTECIMIENTO
SET PRE_ABA = 350.00
WHERE COD_PRV = 'PR21' AND COD_PRO = 'P021'
GO
```