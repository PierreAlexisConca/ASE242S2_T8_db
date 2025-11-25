-- PALEES Store Database Schema - VERSIÓN CON TODAS LAS RESTRICCIONES
-- Version 2.0 - Con restricciones completas para cumplir requisitos académicos
-- Database for clothing store management system
-- Created: 2024-11-24

-- Copyright (c) 2024 PALEES Store
-- All rights reserved.

SET @OLD_UNIQUE_CHECKS=@@UNIQUE_CHECKS, UNIQUE_CHECKS=0;
SET @OLD_FOREIGN_KEY_CHECKS=@@FOREIGN_KEY_CHECKS, FOREIGN_KEY_CHECKS=0;
SET @OLD_SQL_MODE=@@SQL_MODE, SQL_MODE='TRADITIONAL';

DROP SCHEMA IF EXISTS palees;
CREATE SCHEMA palees;
USE palees;

-- ============================================
-- TABLAS MAESTRAS
-- ============================================

--
-- Table structure for table `category`
-- RESTRICCIONES: PK, NOT NULL, UNIQUE, DEFAULT, CHECK
--

CREATE TABLE category (
  id_category INT NOT NULL AUTO_INCREMENT,
  name_category VARCHAR(50) NOT NULL,
  description VARCHAR(100) NOT NULL,
  last_update TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (id_category),
  UNIQUE KEY idx_unique_category_name (name_category),
  KEY idx_name_category (name_category),
  -- RESTRICCIÓN CHECK: Nombre de categoría debe tener al menos 3 caracteres
  CONSTRAINT chk_category_name_length CHECK (CHAR_LENGTH(name_category) >= 3),
  -- RESTRICCIÓN CHECK: Descripción debe tener al menos 10 caracteres
  CONSTRAINT chk_category_desc_length CHECK (CHAR_LENGTH(description) >= 10)
) ENGINE=InnoDB DEFAULT CHARSET=utf8
COMMENT='Tabla maestra: categorías de productos con restricciones de validación';

--
-- Table structure for table `customer`
-- RESTRICCIONES: PK, NOT NULL, UNIQUE, DEFAULT, CHECK
--

CREATE TABLE customer (
  client_id INT NOT NULL AUTO_INCREMENT,
  name VARCHAR(50) NOT NULL,
  surnames VARCHAR(50) NOT NULL,
  mail VARCHAR(80) NOT NULL,
  phone VARCHAR(15) NOT NULL,
  address VARCHAR(100) NOT NULL,
  active BOOLEAN NOT NULL DEFAULT TRUE,
  create_date DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
  last_update TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (client_id),
  -- RESTRICCIÓN UNIQUE: Email único por cliente
  UNIQUE KEY idx_unique_mail (mail),
  -- RESTRICCIÓN UNIQUE: Teléfono único por cliente
  UNIQUE KEY idx_unique_phone (phone),
  KEY idx_surnames (surnames),
  KEY idx_mail (mail),
  -- RESTRICCIÓN CHECK: Email debe contener @
  CONSTRAINT chk_customer_email_format CHECK (mail LIKE '%@%'),
  -- RESTRICCIÓN CHECK: Nombre debe tener al menos 2 caracteres
  CONSTRAINT chk_customer_name_length CHECK (CHAR_LENGTH(name) >= 2),
  -- RESTRICCIÓN CHECK: Apellidos debe tener al menos 3 caracteres
  CONSTRAINT chk_customer_surnames_length CHECK (CHAR_LENGTH(surnames) >= 3),
  -- RESTRICCIÓN CHECK: Teléfono debe tener entre 7 y 15 dígitos
  CONSTRAINT chk_customer_phone_length CHECK (CHAR_LENGTH(phone) BETWEEN 7 AND 15),
  -- RESTRICCIÓN CHECK: Dirección debe tener al menos 10 caracteres
  CONSTRAINT chk_customer_address_length CHECK (CHAR_LENGTH(address) >= 10)
) ENGINE=InnoDB DEFAULT CHARSET=utf8
COMMENT='Tabla maestra: clientes con múltiples restricciones de validación';

--
-- Table structure for table `product`
-- RESTRICCIONES: PK, NOT NULL, FK, DEFAULT, CHECK, INDEX
--

CREATE TABLE product (
  id_product INT NOT NULL AUTO_INCREMENT,
  name_produc VARCHAR(80) NOT NULL,
  description VARCHAR(100) NOT NULL,
  price DECIMAL(10,2) NOT NULL,
  stocks INT NOT NULL DEFAULT 0,
  category_id INT NOT NULL,
  last_update TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (id_product),
  KEY idx_name_product (name_produc),
  KEY idx_fk_category_id (category_id),
  KEY idx_price (price),
  KEY idx_stocks (stocks),
  -- RESTRICCIÓN FK: Categoría debe existir, no se puede eliminar si tiene productos
  CONSTRAINT fk_product_category FOREIGN KEY (category_id) 
    REFERENCES category (id_category) 
    ON DELETE RESTRICT 
    ON UPDATE CASCADE,
  -- RESTRICCIÓN CHECK: Precio debe ser mayor a 0
  CONSTRAINT chk_product_price_positive CHECK (price > 0),
  -- RESTRICCIÓN CHECK: Precio no debe exceder 10000
  CONSTRAINT chk_product_price_max CHECK (price <= 10000),
  -- RESTRICCIÓN CHECK: Stock no puede ser negativo
  CONSTRAINT chk_product_stocks_nonnegative CHECK (stocks >= 0),
  -- RESTRICCIÓN CHECK: Stock no debe exceder 1000 unidades
  CONSTRAINT chk_product_stocks_max CHECK (stocks <= 1000),
  -- RESTRICCIÓN CHECK: Nombre de producto debe tener al menos 3 caracteres
  CONSTRAINT chk_product_name_length CHECK (CHAR_LENGTH(name_produc) >= 3),
  -- RESTRICCIÓN CHECK: Descripción debe tener al menos 10 caracteres
  CONSTRAINT chk_product_desc_length CHECK (CHAR_LENGTH(description) >= 10)
) ENGINE=InnoDB DEFAULT CHARSET=utf8
COMMENT='Tabla maestra: productos con restricciones de precio y stock';

-- ============================================
-- TABLAS TRANSACCIONALES
-- ============================================

--
-- Table structure for table `purchase`
-- RESTRICCIONES: PK, NOT NULL, FK, DEFAULT, CHECK, INDEX
--

CREATE TABLE purchase (
  id_purchase INT NOT NULL AUTO_INCREMENT,
  purchase_date DATE NOT NULL,
  total DECIMAL(10,2) NOT NULL DEFAULT 0.00,
  client_id INT NOT NULL,
  last_update TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (id_purchase),
  KEY idx_purchase_date (purchase_date),
  KEY idx_fk_client_id (client_id),
  KEY idx_total (total),
  -- RESTRICCIÓN FK: Cliente debe existir, no se puede eliminar si tiene compras
  CONSTRAINT fk_purchase_customer FOREIGN KEY (client_id) 
    REFERENCES customer (client_id) 
    ON DELETE RESTRICT 
    ON UPDATE CASCADE,
  -- RESTRICCIÓN CHECK: Total no puede ser negativo
  CONSTRAINT chk_purchase_total_nonnegative CHECK (total >= 0)
) ENGINE=InnoDB DEFAULT CHARSET=utf8
COMMENT='Tabla transaccional: registros de compras con validaciones de fecha y monto';

--
-- Table structure for table `purchase_details`
-- RESTRICCIONES: PK, NOT NULL, FK (múltiples), CHECK, INDEX
--

CREATE TABLE purchase_details (
  id_detail INT NOT NULL AUTO_INCREMENT,
  purchase_id INT NOT NULL,
  product_id INT NOT NULL,
  amount INT NOT NULL,
  unit_price DECIMAL(10,2) NOT NULL,
  subtotal DECIMAL(10,2) NOT NULL,
  last_update TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (id_detail),
  KEY idx_fk_purchase_id (purchase_id),
  KEY idx_fk_product_id (product_id),
  -- RESTRICCIÓN UNIQUE: No se puede agregar el mismo producto dos veces en una compra
  UNIQUE KEY idx_unique_purchase_product (purchase_id, product_id),
  -- RESTRICCIÓN FK: Compra debe existir, si se elimina compra se eliminan detalles
  CONSTRAINT fk_details_purchase FOREIGN KEY (purchase_id) 
    REFERENCES purchase (id_purchase) 
    ON DELETE CASCADE 
    ON UPDATE CASCADE,
  -- RESTRICCIÓN FK: Producto debe existir, no se puede eliminar si está en detalles
  CONSTRAINT fk_details_product FOREIGN KEY (product_id) 
    REFERENCES product (id_product) 
    ON DELETE RESTRICT 
    ON UPDATE CASCADE,
  -- RESTRICCIÓN CHECK: Cantidad debe ser mayor a 0
  CONSTRAINT chk_detail_amount_positive CHECK (amount > 0),
  -- RESTRICCIÓN CHECK: Cantidad no debe exceder 100 unidades por producto
  CONSTRAINT chk_detail_amount_max CHECK (amount <= 100),
  -- RESTRICCIÓN CHECK: Precio unitario debe ser mayor a 0
  CONSTRAINT chk_detail_unit_price_positive CHECK (unit_price > 0),
  -- RESTRICCIÓN CHECK: Subtotal debe ser mayor a 0
  CONSTRAINT chk_detail_subtotal_positive CHECK (subtotal > 0),
  -- RESTRICCIÓN CHECK: Subtotal debe ser igual a cantidad * precio unitario
  CONSTRAINT chk_detail_subtotal_calculation CHECK (subtotal = amount * unit_price)
) ENGINE=InnoDB DEFAULT CHARSET=utf8
COMMENT='Tabla transaccional: detalles de compras con múltiples restricciones de negocio';

-- ============================================
-- VISTAS (REPORTES)
-- ============================================

--
-- View: customer_list
-- Vista para reporte de clientes
--

CREATE VIEW customer_list
AS
SELECT 
  c.client_id AS ID,
  CONCAT(c.name, ' ', c.surnames) AS full_name,
  c.mail AS email,
  c.phone AS phone,
  c.address AS address,
  IF(c.active, 'Active', 'Inactive') AS status,
  COUNT(p.id_purchase) AS total_purchases,
  IFNULL(SUM(p.total), 0.00) AS total_spent
FROM customer AS c
LEFT JOIN purchase AS p ON c.client_id = p.client_id
GROUP BY c.client_id;

--
-- View: product_list
-- Vista para reporte de productos
--

CREATE VIEW product_list
AS
SELECT 
  pr.id_product AS ID,
  pr.name_produc AS product_name,
  pr.description AS description,
  c.name_category AS category,
  pr.price AS price,
  pr.stocks AS stock,
  CASE 
    WHEN pr.stocks = 0 THEN 'Out of Stock'
    WHEN pr.stocks < 10 THEN 'Low Stock'
    ELSE 'In Stock'
  END AS stock_status
FROM product AS pr
JOIN category AS c ON pr.category_id = c.id_category;

--
-- View: sales_by_category
-- Vista para reporte de ventas por categoría
--

CREATE VIEW sales_by_category
AS
SELECT 
  c.name_category AS category,
  COUNT(DISTINCT pr.id_product) AS products_count,
  COUNT(pd.id_detail) AS items_sold,
  SUM(pd.amount) AS total_units,
  SUM(pd.subtotal) AS total_sales
FROM category AS c
JOIN product AS pr ON c.id_category = pr.category_id
LEFT JOIN purchase_details AS pd ON pr.id_product = pd.product_id
GROUP BY c.id_category
ORDER BY total_sales DESC;

--
-- View: sales_summary
-- Vista para reporte resumen de ventas
--

CREATE VIEW sales_summary
AS
SELECT 
  p.id_purchase AS purchase_id,
  p.purchase_date AS date,
  CONCAT(c.name, ' ', c.surnames) AS customer_name,
  c.mail AS customer_email,
  COUNT(pd.id_detail) AS items_count,
  p.total AS total_amount
FROM purchase AS p
JOIN customer AS c ON p.client_id = c.client_id
LEFT JOIN purchase_details AS pd ON p.id_purchase = pd.purchase_id
GROUP BY p.id_purchase
ORDER BY p.purchase_date DESC;

--
-- View: purchase_details_report
-- Vista para reporte detallado de compras
--

CREATE VIEW purchase_details_report
AS
SELECT 
  pd.id_detail AS detail_id,
  p.id_purchase AS purchase_id,
  p.purchase_date AS date,
  CONCAT(c.name, ' ', c.surnames) AS customer_name,
  pr.name_produc AS product_name,
  cat.name_category AS category,
  pd.amount AS quantity,
  pd.unit_price AS unit_price,
  pd.subtotal AS subtotal,
  p.total AS purchase_total
FROM purchase_details AS pd
JOIN purchase AS p ON pd.purchase_id = p.id_purchase
JOIN customer AS c ON p.client_id = c.client_id
JOIN product AS pr ON pd.product_id = pr.id_product
JOIN category AS cat ON pr.category_id = cat.id_category
ORDER BY p.purchase_date DESC, p.id_purchase;

-- ============================================
-- PROCEDIMIENTOS ALMACENADOS
-- ============================================

--
-- Procedure: calculate_purchase_total
-- Calcula el total de una compra
--

DELIMITER $$

CREATE PROCEDURE calculate_purchase_total(
  IN p_purchase_id INT,
  OUT p_total DECIMAL(10,2)
)
LANGUAGE SQL
READS SQL DATA
COMMENT 'Calcula el total de una compra sumando los subtotales'
BEGIN
  SELECT IFNULL(SUM(subtotal), 0.00) INTO p_total
  FROM purchase_details
  WHERE purchase_id = p_purchase_id;
  
  UPDATE purchase 
  SET total = p_total
  WHERE id_purchase = p_purchase_id;
END$$

DELIMITER ;

--
-- Procedure: add_purchase_detail
-- Agrega un detalle de compra con validaciones
--

DELIMITER $$

CREATE PROCEDURE add_purchase_detail(
  IN p_purchase_id INT,
  IN p_product_id INT,
  IN p_amount INT,
  OUT p_result VARCHAR(100)
)
LANGUAGE SQL
MODIFIES SQL DATA
COMMENT 'Agrega un detalle de compra con validación de stock'
BEGIN
  DECLARE v_stock INT;
  DECLARE v_price DECIMAL(10,2);
  DECLARE v_subtotal DECIMAL(10,2);
  
  -- Verificar stock disponible
  SELECT stocks, price INTO v_stock, v_price
  FROM product
  WHERE id_product = p_product_id;
  
  IF v_stock < p_amount THEN
    SET p_result = 'ERROR: Stock insuficiente';
  ELSE
    -- Calcular subtotal
    SET v_subtotal = p_amount * v_price;
    
    -- Insertar detalle
    INSERT INTO purchase_details (purchase_id, product_id, amount, unit_price, subtotal)
    VALUES (p_purchase_id, p_product_id, p_amount, v_price, v_subtotal);
    
    SET p_result = 'SUCCESS: Detalle agregado correctamente';
  END IF;
END$$

DELIMITER ;

-- ============================================
-- FUNCIONES
-- ============================================

--
-- Function: get_customer_total_purchases
-- Retorna el total gastado por un cliente
--

DELIMITER $$

CREATE FUNCTION get_customer_total_purchases(p_client_id INT) 
RETURNS DECIMAL(10,2)
DETERMINISTIC
READS SQL DATA
COMMENT 'Retorna el total gastado por un cliente'
BEGIN
  DECLARE v_total DECIMAL(10,2);
  
  SELECT IFNULL(SUM(total), 0.00) INTO v_total
  FROM purchase
  WHERE client_id = p_client_id;
  
  RETURN v_total;
END$$

DELIMITER ;

--
-- Function: check_product_stock
-- Verifica si hay stock disponible
--

DELIMITER $$

CREATE FUNCTION check_product_stock(p_product_id INT, p_required_amount INT) 
RETURNS BOOLEAN
DETERMINISTIC
READS SQL DATA
COMMENT 'Verifica si hay stock suficiente de un producto'
BEGIN
  DECLARE v_stock INT;
  
  SELECT stocks INTO v_stock
  FROM product
  WHERE id_product = p_product_id;
  
  IF v_stock >= p_required_amount THEN
    RETURN TRUE;
  ELSE
    RETURN FALSE;
  END IF;
END$$

DELIMITER ;

-- ============================================
-- TRIGGERS
-- ============================================

--
-- Trigger: before_purchase_insert
-- Valida fecha de compra antes de insertar
--

DELIMITER $

CREATE TRIGGER before_purchase_insert
BEFORE INSERT ON purchase
FOR EACH ROW
BEGIN
  -- RESTRICCIÓN: Fecha de compra no puede ser futura
  IF NEW.purchase_date > CURDATE() THEN
    SIGNAL SQLSTATE '45000'
    SET MESSAGE_TEXT = 'Error: La fecha de compra no puede ser futura';
  END IF;
  
  -- RESTRICCIÓN: Fecha de compra no puede ser muy antigua (más de 10 años)
  IF NEW.purchase_date < DATE_SUB(CURDATE(), INTERVAL 10 YEAR) THEN
    SIGNAL SQLSTATE '45000'
    SET MESSAGE_TEXT = 'Error: La fecha de compra no puede ser anterior a 10 años';
  END IF;
END$

DELIMITER ;

--
-- Trigger: before_purchase_update
-- Valida fecha de compra antes de actualizar
--

DELIMITER $

CREATE TRIGGER before_purchase_update
BEFORE UPDATE ON purchase
FOR EACH ROW
BEGIN
  -- RESTRICCIÓN: Fecha de compra no puede ser futura
  IF NEW.purchase_date > CURDATE() THEN
    SIGNAL SQLSTATE '45000'
    SET MESSAGE_TEXT = 'Error: La fecha de compra no puede ser futura';
  END IF;
  
  -- RESTRICCIÓN: Fecha de compra no puede ser muy antigua (más de 10 años)
  IF NEW.purchase_date < DATE_SUB(CURDATE(), INTERVAL 10 YEAR) THEN
    SIGNAL SQLSTATE '45000'
    SET MESSAGE_TEXT = 'Error: La fecha de compra no puede ser anterior a 10 años';
  END IF;
END$

DELIMITER ;

--
-- Trigger: before_purchase_detail_insert
-- Calcula subtotal antes de insertar
--

DELIMITER $

CREATE TRIGGER before_purchase_detail_insert
BEFORE INSERT ON purchase_details
FOR EACH ROW
BEGIN
  -- Calcular subtotal automáticamente
  SET NEW.subtotal = NEW.amount * NEW.unit_price;
END$

DELIMITER ;

--
-- Trigger: after_purchase_detail_insert
-- Actualiza total de compra y disminuye stock
--

DELIMITER $$

CREATE TRIGGER after_purchase_detail_insert
AFTER INSERT ON purchase_details
FOR EACH ROW
BEGIN
  -- Actualizar total de la compra
  UPDATE purchase 
  SET total = (
    SELECT IFNULL(SUM(subtotal), 0.00)
    FROM purchase_details 
    WHERE purchase_id = NEW.purchase_id
  )
  WHERE id_purchase = NEW.purchase_id;
  
  -- Disminuir stock del producto
  UPDATE product 
  SET stocks = stocks - NEW.amount
  WHERE id_product = NEW.product_id;
END$$

DELIMITER ;

--
-- Trigger: after_purchase_detail_delete
-- Actualiza total de compra y aumenta stock al eliminar detalle
--

DELIMITER $$

CREATE TRIGGER after_purchase_detail_delete
AFTER DELETE ON purchase_details
FOR EACH ROW
BEGIN
  -- Actualizar total de la compra
  UPDATE purchase 
  SET total = (
    SELECT IFNULL(SUM(subtotal), 0.00)
    FROM purchase_details 
    WHERE purchase_id = OLD.purchase_id
  )
  WHERE id_purchase = OLD.purchase_id;
  
  -- Aumentar stock del producto
  UPDATE product 
  SET stocks = stocks + OLD.amount
  WHERE id_product = OLD.product_id;
END$$

DELIMITER ;

SET SQL_MODE=@OLD_SQL_MODE;
SET FOREIGN_KEY_CHECKS=@OLD_FOREIGN_KEY_CHECKS;
SET UNIQUE_CHECKS=@OLD_UNIQUE_CHECKS;

-- ============================================
-- RESUMEN DE RESTRICCIONES IMPLEMENTADAS
-- ============================================
/*
RESTRICCIONES POR TABLA:

1. CATEGORY (7 restricciones):
   - PRIMARY KEY
   - UNIQUE (nombre)
   - NOT NULL (todos los campos)
   - CHECK (longitud nombre >= 3)
   - CHECK (longitud descripción >= 10)
   - DEFAULT (timestamp)
   - INDEX (nombre)

2. CUSTOMER (10 restricciones):
   - PRIMARY KEY
   - UNIQUE (email)
   - UNIQUE (teléfono)
   - NOT NULL (todos los campos)
   - CHECK (formato email)
   - CHECK (longitud nombre >= 2)
   - CHECK (longitud apellidos >= 3)
   - CHECK (longitud teléfono 7-15)
   - CHECK (longitud dirección >= 10)
   - DEFAULT (active=TRUE, timestamps)

3. PRODUCT (12 restricciones):
   - PRIMARY KEY
   - FOREIGN KEY (category_id)
   - NOT NULL (todos los campos)
   - CHECK (precio > 0)
   - CHECK (precio <= 10000)
   - CHECK (stock >= 0)
   - CHECK (stock <= 1000)
   - CHECK (longitud nombre >= 3)
   - CHECK (longitud descripción >= 10)
   - DEFAULT (stock=0, timestamp)
   - INDEX múltiples (nombre, precio, stock, categoría)
   - ON DELETE RESTRICT / ON UPDATE CASCADE

4. PURCHASE (8 restricciones):
   - PRIMARY KEY
   - FOREIGN KEY (client_id)
   - NOT NULL (todos los campos)
   - CHECK (total >= 0)
   - TRIGGER (fecha <= hoy)
   - TRIGGER (fecha >= hace 10 años)
   - DEFAULT (total=0, timestamp)
   - INDEX múltiples (fecha, cliente, total)

5. PURCHASE_DETAILS (13 restricciones):
   - PRIMARY KEY
   - FOREIGN KEY (purchase_id) con ON DELETE CASCADE
   - FOREIGN KEY (product_id) con ON DELETE RESTRICT
   - UNIQUE (compra + producto)
   - NOT NULL (todos los campos)
   - CHECK (cantidad > 0)
   - CHECK (cantidad <= 100)
   - CHECK (precio unitario > 0)
   - CHECK (subtotal > 0)
   - CHECK (subtotal = cantidad * precio)
   - DEFAULT (timestamp)
   - INDEX múltiples
   - Triggers automáticos

TRIGGERS IMPLEMENTADOS (5):
1. before_purchase_insert - Valida fecha de compra (no futura, no antigua)
2. before_purchase_update - Valida fecha al actualizar
3. before_purchase_detail_insert - Calcula subtotal automático
4. after_purchase_detail_insert - Actualiza total y disminuye stock
5. after_purchase_detail_delete - Restaura total y aumenta stock

TOTAL: 50+ restricciones implementadas (CHECK, FK, UNIQUE, NOT NULL, DEFAULT, TRIGGERS)
*/