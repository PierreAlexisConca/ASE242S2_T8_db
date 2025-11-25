-- PALEES Store Database Data
-- Version 1.0
-- Sample data for testing (10 records per table with relationships)
-- Created: 2024-11-24

USE palees;

SET AUTOCOMMIT=0;

--
-- Data for table `category`
--

INSERT INTO category (id_category, name_category, description, last_update) VALUES
(1, 'Camisas Hombre', 'Camisas casuales y formales para hombre', '2024-11-24 10:00:00'),
(2, 'Pantalones Hombre', 'Pantalones jeans, casuales y formales', '2024-11-24 10:00:00'),
(3, 'Vestidos Mujer', 'Vestidos casuales, elegantes y de fiesta', '2024-11-24 10:00:00'),
(4, 'Blusas Mujer', 'Blusas y tops para mujer', '2024-11-24 10:00:00'),
(5, 'Faldas', 'Faldas largas, cortas y midi', '2024-11-24 10:00:00'),
(6, 'Chaquetas', 'Chaquetas y abrigos unisex', '2024-11-24 10:00:00'),
(7, 'Accesorios', 'Cinturones, bufandas y gorros', '2024-11-24 10:00:00'),
(8, 'Calzado', 'Zapatos casuales y deportivos', '2024-11-24 10:00:00'),
(9, 'Ropa Deportiva', 'Prendas para actividad física', '2024-11-24 10:00:00'),
(10, 'Joyería', 'Collares, pulseras y aretes', '2024-11-24 10:00:00');

COMMIT;

--
-- Data for table `customer`
--

INSERT INTO customer (client_id, name, surnames, mail, phone, address, active, create_date, last_update) VALUES
(1, 'María', 'González López', 'maria.gonzalez@email.com', '987654321', 'Av. Principal 123, Lima', TRUE, '2024-01-15 09:30:00', '2024-11-24 10:00:00'),
(2, 'Carlos', 'Rodríguez Silva', 'carlos.rodriguez@email.com', '987654322', 'Jr. Los Pinos 456, San Isidro', TRUE, '2024-02-20 14:15:00', '2024-11-24 10:00:00'),
(3, 'Ana', 'Martínez Torres', 'ana.martinez@email.com', '987654323', 'Calle Las Flores 789, Miraflores', TRUE, '2024-03-10 11:45:00', '2024-11-24 10:00:00'),
(4, 'Luis', 'Fernández Ruiz', 'luis.fernandez@email.com', '987654324', 'Av. Arequipa 234, Lince', TRUE, '2024-04-05 16:20:00', '2024-11-24 10:00:00'),
(5, 'Patricia', 'Sánchez Díaz', 'patricia.sanchez@email.com', '987654325', 'Jr. Libertad 567, Surco', TRUE, '2024-05-12 10:30:00', '2024-11-24 10:00:00'),
(6, 'Jorge', 'Ramírez Castro', 'jorge.ramirez@email.com', '987654326', 'Calle Sol 890, San Borja', TRUE, '2024-06-18 13:00:00', '2024-11-24 10:00:00'),
(7, 'Laura', 'Pérez Morales', 'laura.perez@email.com', '987654327', 'Av. Colonial 345, Callao', TRUE, '2024-07-22 15:45:00', '2024-11-24 10:00:00'),
(8, 'Miguel', 'Torres Vargas', 'miguel.torres@email.com', '987654328', 'Jr. Unión 678, Jesús María', TRUE, '2024-08-30 09:15:00', '2024-11-24 10:00:00'),
(9, 'Carmen', 'López Herrera', 'carmen.lopez@email.com', '987654329', 'Calle Luna 901, La Molina', TRUE, '2024-09-14 12:30:00', '2024-11-24 10:00:00'),
(10, 'Roberto', 'García Núñez', 'roberto.garcia@email.com', '987654330', 'Av. Brasil 432, Pueblo Libre', TRUE, '2024-10-20 14:00:00', '2024-11-24 10:00:00');

COMMIT;

--
-- Data for table `product`
--

INSERT INTO product (id_product, name_produc, description, price, stocks, category_id, last_update) VALUES
(1, 'Camisa Formal Blanca', 'Camisa de vestir manga larga color blanco', 89.90, 25, 1, '2024-11-24 10:00:00'),
(2, 'Jean Slim Fit Azul', 'Pantalón jean corte slim fit azul oscuro', 129.90, 30, 2, '2024-11-24 10:00:00'),
(3, 'Vestido Floral Verano', 'Vestido estampado floral manga corta', 149.90, 15, 3, '2024-11-24 10:00:00'),
(4, 'Blusa Elegante Negra', 'Blusa de satén color negro con mangas largas', 79.90, 20, 4, '2024-11-24 10:00:00'),
(5, 'Falda Plisada Beige', 'Falda midi plisada color beige', 99.90, 18, 5, '2024-11-24 10:00:00'),
(6, 'Chaqueta Cuero Marrón', 'Chaqueta de cuero sintético color marrón', 249.90, 12, 6, '2024-11-24 10:00:00'),
(7, 'Cinturón Casual Negro', 'Cinturón de cuero genuino color negro', 45.90, 35, 7, '2024-11-24 10:00:00'),
(8, 'Zapatillas Deportivas', 'Zapatillas running unisex color blanco', 189.90, 22, 8, '2024-11-24 10:00:00'),
(9, 'Polo Deportivo Dry-Fit', 'Polo manga corta tecnología dry-fit', 69.90, 28, 9, '2024-11-24 10:00:00'),
(10, 'Collar Plata 925', 'Collar cadena de plata 925 con dije', 159.90, 10, 10, '2024-11-24 10:00:00');

COMMIT;

--
-- Data for table `purchase`
--

INSERT INTO purchase (id_purchase, purchase_date, total, client_id, last_update) VALUES
(1, '2024-11-01', 219.80, 1, '2024-11-24 10:00:00'),
(2, '2024-11-03', 129.90, 2, '2024-11-24 10:00:00'),
(3, '2024-11-05', 299.80, 3, '2024-11-24 10:00:00'),
(4, '2024-11-08', 179.80, 4, '2024-11-24 10:00:00'),
(5, '2024-11-10', 349.80, 5, '2024-11-24 10:00:00'),
(6, '2024-11-12', 495.80, 6, '2024-11-24 10:00:00'),
(7, '2024-11-15', 115.80, 7, '2024-11-24 10:00:00'),
(8, '2024-11-18', 379.80, 8, '2024-11-24 10:00:00'),
(9, '2024-11-20', 229.80, 9, '2024-11-24 10:00:00'),
(10, '2024-11-22', 569.70, 10, '2024-11-24 10:00:00');

COMMIT;

--
-- Data for table `purchase_details`
--

INSERT INTO purchase_details (id_detail, purchase_id, product_id, amount, unit_price, subtotal, last_update) VALUES
-- Purchase 1: María compra camisa y camisa formal
(1, 1, 1, 2, 89.90, 179.80, '2024-11-24 10:00:00'),
(2, 1, 7, 1, 45.90, 45.90, '2024-11-24 10:00:00'),

-- Purchase 2: Carlos compra jean
(3, 2, 2, 1, 129.90, 129.90, '2024-11-24 10:00:00'),

-- Purchase 3: Ana compra vestido y blusa
(4, 3, 3, 1, 149.90, 149.90, '2024-11-24 10:00:00'),
(5, 3, 4, 1, 79.90, 79.90, '2024-11-24 10:00:00'),

-- Purchase 4: Luis compra blusa y falda
(6, 4, 4, 1, 79.90, 79.90, '2024-11-24 10:00:00'),
(7, 4, 5, 1, 99.90, 99.90, '2024-11-24 10:00:00'),

-- Purchase 5: Patricia compra chaqueta y falda
(8, 5, 6, 1, 249.90, 249.90, '2024-11-24 10:00:00'),
(9, 5, 5, 1, 99.90, 99.90, '2024-11-24 10:00:00'),

-- Purchase 6: Jorge compra chaqueta, jean y cinturón
(10, 6, 6, 1, 249.90, 249.90, '2024-11-24 10:00:00'),
(11, 6, 2, 1, 129.90, 129.90, '2024-11-24 10:00:00'),
(12, 6, 7, 1, 45.90, 45.90, '2024-11-24 10:00:00'),

-- Purchase 7: Laura compra polo deportivo y cinturón
(13, 7, 9, 1, 69.90, 69.90, '2024-11-24 10:00:00'),
(14, 7, 7, 1, 45.90, 45.90, '2024-11-24 10:00:00'),

-- Purchase 8: Miguel compra zapatillas y jean
(15, 8, 8, 1, 189.90, 189.90, '2024-11-24 10:00:00'),
(16, 8, 2, 1, 129.90, 129.90, '2024-11-24 10:00:00'),

-- Purchase 9: Carmen compra vestido y blusa
(17, 9, 3, 1, 149.90, 149.90, '2024-11-24 10:00:00'),
(18, 9, 4, 1, 79.90, 79.90, '2024-11-24 10:00:00'),

-- Purchase 10: Roberto compra chaqueta, zapatillas y collar
(19, 10, 6, 1, 249.90, 249.90, '2024-11-24 10:00:00'),
(20, 10, 8, 1, 189.90, 189.90, '2024-11-24 10:00:00'),
(21, 10, 10, 1, 159.90, 159.90, '2024-11-24 10:00:00');

COMMIT;

SET AUTOCOMMIT=1;

-- Verify data insertion
SELECT 'Categories loaded:' AS info, COUNT(*) AS count FROM category;
SELECT 'Customers loaded:' AS info, COUNT(*) AS count FROM customer;
SELECT 'Products loaded:' AS info, COUNT(*) AS count FROM product;
SELECT 'Purchases loaded:' AS info, COUNT(*) AS count FROM purchase;
SELECT 'Purchase details loaded:' AS info, COUNT(*) AS count FROM purchase_details;