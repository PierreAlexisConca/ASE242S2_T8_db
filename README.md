ASE242S2_00_db - PALEES Store
Contiene toda la información del diseño y desarrollo de la base de datos del proyecto PALEES (Sistema de Gestión para Tienda de Ropa).

📋 Descripción del Proyecto
PALEES es una base de datos diseñada para gestionar las operaciones de una tienda de ropa, incluyendo:

Gestión de productos y categorías
Registro de clientes
Control de inventario (stock)
Registro de ventas y detalles de compra
Reportes de ventas y análisis
👥 Equipo de Desarrollo
Pierre Alexis Conca Flores
Medalid Chaparro Condezo
Anthony Gala Vilcapoma
Curso: Administración de Sistemas de Bases de Datos
Sección: ASE242S2
Ciclo: 2024-2
Fecha: Noviembre 2024

🗂️ Estructura del Repositorio
ASE242S2_00_db/
│
├── caso/
│   └── descripción.md          # Descripción del caso de negocio
│
├── desarrollo/
│   └── info-development.md     # Documentación paso a paso de implementación
│
├── imagen/
│   ├── diseño-logico.png       # Diagrama del modelo lógico
│   └── diseño-fisico.png       # Diagrama del modelo físico
│
├── recursos/
│   └── diccionario-datos.pdf   # Diccionario de datos completo
│
├── guion/
│   └── scripts.md              # Guion para demostración en video
│
└── scripts/
    ├── 01-schema.sql           # Script de estructura de BD
    └── 02-data.sql             # Script de inserción de datos
🎯 Características Principales
Tablas Maestras
category: 10 categorías de productos (camisas, pantalones, vestidos, etc.)
customer: 10 clientes registrados con información completa
product: 10 productos con precios, stock y categorías
Tablas Transaccionales
purchase: 10 registros de compras realizadas
purchase_details: 21 detalles de compra (ventas con múltiples productos)
Restricciones Implementadas (50+)
✅ PRIMARY KEY en todas las tablas
✅ FOREIGN KEY con integridad referencial
✅ UNIQUE para evitar duplicados (email, teléfono, nombres)
✅ CHECK para validar reglas de negocio (precios, stock, cantidades)
✅ NOT NULL en campos obligatorios
✅ DEFAULT para valores por defecto
✅ TRIGGERS para automatización (cálculos, validaciones, actualización de stock)
Vistas para Reportes
customer_list: Listado de clientes con estadísticas
product_list: Productos con estado de stock
sales_by_category: Ventas agrupadas por categoría
sales_summary: Resumen de ventas por cliente
purchase_details_report: Detalle completo de todas las ventas
🚀 Instalación y Uso
Requisitos Previos
MySQL 8.0 o superior
Docker (opcional)
MySQL Workbench o DBeaver
Opción 1: Usando Docker
bash
# Crear contenedor MySQL
docker run --name palees -e MYSQL_ROOT_PASSWORD=concaflores123 -p 3307:3306 -d mysql:8.0

# Esperar 30 segundos que MySQL inicie

# Conectarse al contenedor
docker exec -it palees mysql -uroot -pconcaflores123
Opción 2: MySQL Local
bash
# Conectarse a MySQL
mysql -u root -p
Ejecutar Scripts
sql
-- 1. Ejecutar script de estructura
SOURCE /ruta/a/scripts/01-schema.sql;

-- 2. Ejecutar script de datos
SOURCE /ruta/a/scripts/02-data.sql;

-- 3. Verificar instalación
USE palees;
SHOW TABLES;
SELECT * FROM customer_list;
📊 Reportes con JasperReports
El proyecto incluye 2 reportes principales:

Reporte Maestro: customer_list
Muestra listado completo de clientes
Total de compras por cliente
Total gastado por cliente
Reporte Transaccional: purchase_details_report
Detalle completo de todas las ventas
Información de cliente, producto y montos
Agrupado por fecha de compra
Conexión JDBC:

Host: localhost
Port: 3307
Database: palees
User: root
Password: concaflores123
URL: jdbc:mysql://localhost:3307/palees
🧪 Pruebas de Restricciones
Restricciones que puedes probar:
sql
-- ❌ Intentar precio negativo (debe fallar)
INSERT INTO product (name_produc, description, price, stocks, category_id)
VALUES ('Test', 'Producto de prueba', -50.00, 10, 1);

-- ❌ Intentar email sin @ (debe fallar)
INSERT INTO customer (name, surnames, mail, phone, address)
VALUES ('Test', 'Usuario', 'emailsinarroba', '999999999', 'Dirección de prueba');

-- ❌ Intentar fecha futura (debe fallar)
INSERT INTO purchase (purchase_date, total, client_id)
VALUES ('2026-12-31', 100.00, 1);

-- ✅ Inserción correcta (debe funcionar)
INSERT INTO product (name_produc, description, price, stocks, category_id)
VALUES ('Camisa Nueva', 'Camisa de algodón azul', 79.90, 20, 1);
📹 Video de Demostración
[Enlace al video en YouTube/Drive]

Contenido del video:

Presentación del equipo (con cámara activa)
Explicación del caso de negocio
Demostración de scripts de estructura y datos
Prueba de restricciones funcionando
Demostración de reportes en JasperReports
Navegación por el repositorio de GitHub
📚 Documentación Adicional
Descripción del caso
Guía de implementación
Diccionario de datos
Scripts SQL
🔧 Tecnologías Utilizadas
Base de Datos: MySQL 8.0
Contenedor: Docker
Reportes: JasperReports / iReport
Modelado: Vertabelo / MySQL Workbench
Versionamiento: Git & GitHub
📝 Licencia
Este proyecto es desarrollado con fines académicos para el curso de Administración de Sistemas de Bases de Datos.

Última actualización: Noviembre 2024
Versión: 1.0

