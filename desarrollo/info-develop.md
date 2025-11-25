Preparar el entorno

Asegúrate de tener MySQL (o MySQL Workbench) instalado y la conexión lista (host localhost, puerto 3307 en tu ejemplo).

Abre el repositorio del proyecto en tu máquina (ruta del proyecto donde están las carpetas scripts/, imagen/, recursos/, desarrollo/, etc.).

Abre MySQL Workbench o el cliente SQL que uses.

Abrir el repositorio (mostrar estructura)

Navega en el explorador de archivos al repo y abre la carpeta del proyecto.

Mostrar estas carpetas y archivos y explicar brevemente su propósito:

caso/ — caso de negocio

desarrollo/ — documentación de implementación

imagen/ — diagramas lógico y físico

recursos/ — diccionario de datos (PDF)

guion/ — guion / pasos

scripts/ — scripts SQL (estructura y datos)

README.md — instrucciones generales y cómo ejecutar

Crear el esquema de la base de datos

En MySQL Workbench, conectar a la instancia.

Abrir scripts/01-schema.sql.

Ejecutar el script (o ejecutar manualmente las sentencias principales). Ejemplo de comandos que puedes ejecutar:

CREATE SCHEMA IF NOT EXISTS palees;
USE palees;
-- (A continuación ejecutar contenido de 01-schema.sql)


Mostrar mensajes de ejecución sin errores.

Crear tablas principales (ejecutar y explicar)

Ejecutar las sentencias de creación de tablas incluidas en 01-schema.sql. Mostrar las tablas clave: category, customer, product, purchase, purchase_details.

Ejecuta para verificar:

SHOW TABLES;


Aclarar brevemente las restricciones implementadas en cada tabla (PK, FK, UNIQUE, CHECK, ON DELETE).

Ejecutar script de datos

Abrir scripts/02-data.sql.

Ejecutar los INSERT de prueba incluidos.

Ejecutar consultas de verificación:

SELECT COUNT(*) FROM category;
SELECT COUNT(*) FROM customer;
SELECT COUNT(*) FROM product;
SELECT COUNT(*) FROM purchase;
SELECT COUNT(*) FROM purchase_details;


Mostrar resultados indicando que los registros se insertaron.

Ver consultas de muestreo

Ejecutar y mostrar ejemplos de selección para presentar datos:

SELECT * FROM category;
SELECT * FROM customer LIMIT 3;
SELECT * FROM product LIMIT 3;
SELECT * FROM purchase ORDER BY purchase_date DESC LIMIT 5;
SELECT * FROM purchase_details WHERE purchase_id IN (1,6,10);


Demostrar restricciones (pruebas que generan errores)

Ejecutar inserciones que deben fallar y mostrar el error que devuelve MySQL (explicar por qué falla). Ejemplos:

Precio negativo (CHECK):

INSERT INTO product (name_produc, description, price, stocks, category_id)
VALUES ('Producto Test', 'Prueba', -50.00, 10, 1);


Email sin @ (CHECK):

INSERT INTO customer (name, surnames, mail, phone, address)
VALUES ('Test','Usuario','emailsinarroba','999999999','Dirección');


Email duplicado (UNIQUE):

-- Si ya existe 'maria.gonzalez@email.com'
INSERT INTO customer (name, surnames, mail, phone, address)
VALUES ('Nuevo','Cliente','maria.gonzalez@email.com','999888777','Dir');


Fecha futura (trigger o validación):

INSERT INTO purchase (purchase_date, total, client_id)
VALUES ('2026-12-31', 100.00, 1);


Stock excedido (CHECK):

INSERT INTO product (name_produc, description, price, stocks, category_id)
VALUES ('Test Stock','Prueba',50.00,1500,1);


Mostrar cada error en pantalla y explicar la restricción que lo causa.

Probar inserción válida

Insertar un registro válido y mostrar que sí se inserta:

INSERT INTO product (name_produc, description, price, stocks, category_id)
VALUES ('Polo Nuevo Diseño','Polo algodón',59.90,30,9);
SELECT * FROM product WHERE name_produc = 'Polo Nuevo Diseño';


Demostrar triggers y actualización automática (stock y totales)

Mostrar stock actual de un producto:

SELECT id_product, name_produc, stocks FROM product WHERE id_product = 2;


Insertar un detalle de venta que active el trigger de stock y el trigger de total:

INSERT INTO purchase_details (purchase_id, product_id, amount, unit_price, subtotal)
VALUES (1, 2, 3, 129.90, 389.70);


Volver a consultar el stock del producto y el total de la compra:

SELECT name_produc, stocks FROM product WHERE id_product = 2;
SELECT id_purchase, total FROM purchase WHERE id_purchase = 1;


Mostrar el cambio esperado (por ejemplo: stock disminuyó en 3; total de la compra fue recalculado).

Consultas de verificación y conteos finales

Ejecutar una consulta agrupada para mostrar el conteo de registros por tabla:

SELECT 'Categorías' AS tabla, COUNT(*) FROM category
UNION ALL SELECT 'Clientes', COUNT(*) FROM customer
UNION ALL SELECT 'Productos', COUNT(*) FROM product
UNION ALL SELECT 'Compras', COUNT(*) FROM purchase
UNION ALL SELECT 'Detalles', COUNT(*) FROM purchase_details;


Mostrar los resultados a cámara.

Abrir y previsualizar reportes (JasperReports / iReport)

Abrir JasperSoft Studio o iReport.

Cargar customer_list_report.jrxml:

Mostrar la query asociada (ej.: SELECT * FROM customer_list;) y ejecutar preview.

Mostrar que el reporte lista clientes, totales, etc.

Cargar purchase_details_report.jrxml:

Mostrar la query (ej.: SELECT * FROM purchase_details_report;) y ejecutar preview.

Mostrar filtros (por fecha/cliente), subtotales y totales.

Demostrar cómo aplicar un filtro simple (por rango de fechas o por cliente) y actualizar el preview.

Mostrar documentación y diagramas

Abrir imagen/ y mostrar:

Diagrama lógico

Diagrama físico

Abrir recursos/ y mostrar:

Diccionario de datos (PDF) — explicar que contiene tablas, campos, tipos y restricciones.

Abrir desarrollo/info-development.md o equivalente y señalar:

Explicación paso a paso de cada sentencia SQL usada.

Comprobaciones finales recomendadas (lista rápida)

Ejecutar SHOW TABLES; y DESCRIBE <tabla>; para las tablas clave.

Probar 1–2 inserciones válidas y 1–2 inserciones inválidas por pantalla para evidenciar restricciones.

Previsualizar 1 reporte maestro (clientes) y 1 reporte transaccional (ventas).