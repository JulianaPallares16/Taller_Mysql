# Taller_Mysql

### Estudiante:
**Juliana Pallares**

## Normalización
CREATE TABLE IF NOT EXISTS customer(
  id VARCHAR(50) PRIMARY KEY,
  nombre VARCHAR(50),
  email VARCHAR(50) UNIQUE
) ENGINE = INNODB;

CREATE TABLE IF NOT EXISTS address(
  id INT AUTO_INCREMENT PRIMARY KEY,
  dirección VARCHAR(255)
) ENGINE = INNODB;

CREATE TABLE IF NOT EXISTS countries(
  id INT AUTO_INCREMENT PRIMARY KEY,
  country_name VARCHAR(50) UNIQUE
) ENGINE = INNODB;

CREATE TABLE IF NOT EXISTS regions(
  id INT AUTO_INCREMENT PRIMARY KEY,
  region_name VARCHAR(50) UNIQUE,
  country_id INT,
  CONSTRAINT FK_country_id FOREIGN KEY (country_id) REFERENCES countries(id)
) ENGINE = INNODB;

CREATE TABLE IF NOT EXISTS cities(
  id INT AUTO_INCREMENT PRIMARY KEY,
  city_name VARCHAR(50) UNIQUE,
  region_id INT,
  CONSTRAINT FK_region_id FOREIGN KEY (region_id) REFERENCES regions(id)
) ENGINE = INNODB;

CREATE TABLE IF NOT EXISTS zip_code(
  id INT AUTO_INCREMENT PRIMARY KEY,
  codigo VARCHAR(50) UNIQUE,
  city_id INT,
  CONSTRAINT FK_city_id FOREIGN KEY (city_id) REFERENCES cities(id)
) ENGINE = INNODB;

CREATE TABLE IF NOT EXISTS suppliers(
  id INT AUTO_INCREMENT PRIMARY KEY,
  nombre VARCHAR(50)
) ENGINE = INNODB;

CREATE TABLE IF NOT EXISTS locations(
  id INT AUTO_INCREMENT PRIMARY KEY,
  cliente_id VARCHAR(50),
  CONSTRAINT FK_cliente_id FOREIGN KEY (cliente_id) REFERENCES customer(id),
  proveedor_id INT,
  CONSTRAINT FK_proveedor_id FOREIGN KEY (proveedor_id) REFERENCES suppliers(id),
  direccion_id INT,
  CONSTRAINT FK_direccion_id FOREIGN KEY (direccion_id) REFERENCES address(id),
  codigopostal_id INT,
  CONSTRAINT FK_codigopostal_id FOREIGN KEY (codigopostal_id) REFERENCES zip_code(id)
) ENGINE = INNODB;

CREATE TABLE IF NOT EXISTS empleoyees(
  id INT AUTO_INCREMENT PRIMARY KEY,
  documento VARCHAR(50) UNIQUE
) ENGINE = INNODB;

CREATE TABLE IF NOT EXISTS empleoyee_data(
  id INT AUTO_INCREMENT PRIMARY KEY,
  nombre VARCHAR(50),
  salario DECIMAL(10,2),
  fecha_contrato DATE,
  empleado_id INT,
  CONSTRAINT FK_empleado_id FOREIGN KEY (empleado_id) REFERENCES empleoyees(id)
) ENGINE = INNODB;

CREATE TABLE IF NOT EXISTS positions(
  id INT AUTO_INCREMENT PRIMARY KEY,
  cargo VARCHAR(50),
  descripción TEXT
) ENGINE = INNODB;

CREATE TABLE IF NOT EXISTS empleoyee_position(
  id INT AUTO_INCREMENT PRIMARY KEY,
  cargo_id INT,
  CONSTRAINT FK_cargo_id FOREIGN KEY (cargo_id) REFERENCES positions(id),
  empleado_id INT,
  CONSTRAINT FK_empleadoid FOREIGN KEY (empleado_id) REFERENCES empleoyees(id)
) ENGINE = INNODB;

CREATE TABLE IF NOT EXISTS customer_phone(
  id INT AUTO_INCREMENT PRIMARY KEY,
  numero VARCHAR(50),
  código_area VARCHAR(3),
  cliente_id VARCHAR(50),
  CONSTRAINT FK_clienteid FOREIGN KEY (cliente_id) REFERENCES customer(id)
) ENGINE = INNODB;

CREATE TABLE IF NOT EXISTS supplier_contact(
  id INT AUTO_INCREMENT PRIMARY KEY,
  email VARCHAR(50) UNIQUE,
  proveedor_id INT,
  CONSTRAINT FK_proveedorid FOREIGN KEY (proveedor_id) REFERENCES suppliers(id)
) ENGINE = INNODB;

CREATE TABLE IF NOT EXISTS supplier_phone(
  id INT AUTO_INCREMENT PRIMARY KEY,
  numero VARCHAR(50),
  código_area VARCHAR(3),
  proveedorcontacto_id INT,
  CONSTRAINT FK_proveedorcontacto_id FOREIGN KEY (proveedorcontacto_id) REFERENCES supplier_contact(id)
) ENGINE = INNODB;

CREATE TABLE IF NOT EXISTS empleoyee_supplier(
  proveedor_id INT,
  CONSTRAINT FK_proveedorrid FOREIGN KEY (proveedor_id) REFERENCES suppliers(id),
  empleado_id INT,
  CONSTRAINT FK_empleadooid FOREIGN KEY (empleado_id) REFERENCES empleoyees(id),
  PRIMARY KEY (proveedor_id, empleado_id)
) ENGINE = INNODB;

CREATE TABLE IF NOT EXISTS product_types(
  id INT AUTO_INCREMENT PRIMARY KEY,
  nombre VARCHAR(50),
  descripción TEXT
) ENGINE = INNODB;

CREATE TABLE IF NOT EXISTS products(
  id INT AUTO_INCREMENT PRIMARY KEY,
  nombre VARCHAR(50),
  precio DECIMAL(10,2),
  proveedor_id INT,
  CONSTRAINT FK_provedorid FOREIGN KEY (proveedor_id) REFERENCES suppliers(id),
  tipoproducto_id INT,
  CONSTRAINT FK_tipoproducto_id FOREIGN KEY (tipoproducto_id) REFERENCES product_types(id)
) ENGINE = INNODB;

CREATE TABLE IF NOT EXISTS orders(
  id INT AUTO_INCREMENT PRIMARY KEY,
  cliente_id VARCHAR(50),
  CONSTRAINT FK_clientid FOREIGN KEY (cliente_id) REFERENCES customer(id),
  fecha DATE
  empleado_id INT,
  CONSTRAINT FK_empleoyeeid FOREIGN KEY (empleado_id) REFERENCES empleoyees(id),
) ENGINE = INNODB;

CREATE TABLE IF NOT EXISTS order_detail(
  id INT AUTO_INCREMENT PRIMARY KEY,
  pedido_id INT,
  CONSTRAINT FK_pedido_id FOREIGN KEY (pedido_id) REFERENCES orders(id),
  producto_id INT,
  CONSTRAINT FK_producto_id FOREIGN KEY (producto_id) REFERENCES products(id),
  cantidad INT,
  precio DECIMAL(10,2)
) ENGINE = INNODB;

CREATE TABLE IF NOT EXISTS order_history(
  id INT AUTO_INCREMENT PRIMARY KEY,
  cliente_nuevo VARCHAR(50),
  fecha_nueva DATE,
  pedido_id INT,
  CONSTRAINT FK_pedidoid FOREIGN KEY (pedido_id) REFERENCES orders(id)
) ENGINE = INNODB;

## Datos insertados
INSERT INTO customer(id, nombre, email) VALUES ('1104260414', 'Juliana', 'julianaapallaresn@gmail.com'), ('18973450', 'Edgar', 'pallaresedgar@gmail.com'), ('64574575', 'Biela', 'biandel14@hotmail.com'), ('234567', 'Ivanna', 'paterninamercadoivanna@gmail.com');

INSERT INTO orders(cliente_id, fecha, empleado_id) VALUES ('1104260414', '2025-06-14', 1), ('18973450', '2025-06-21', 3), ('64574575', '2024-06-25', 1)

INSERT INTO orders(cliente_id, fecha, empleado_id) VALUES ('1104260414', '2025-06-22', 1);

INSERT INTO positions(cargo, descripción) VALUES ('Asesor', 'Persona especializada que brinda orientación, recomendaciones o soluciones en un área específica para apoyar la toma de decisiones o mejorar resultados'), ('Gerente general', 'Supervisa toda la organización y define estrategias globales');

INSERT INTO empleoyees(documento) VALUES ('123456'), ('654321'), ('246810');

INSERT INTO empleoyee_position(cargo_id, empleado_id) VALUES (1, 4), (1, 1), (2,2), (1,3);

INSERT INTO empleoyee_data(nombre, salario, fecha_contrato, empleado_id) VALUES ('Dhall', 2500000.00, '2024-03-12', 4), ('Diego', 2500000.00, '2024-03-26', 1), ('Mayudis', 8500000.00, '2020-03-06', 2), ('Marlen', 2500000.00, '2024-03-16', 3);

INSERT INTO suppliers(nombre) VALUES ('Dulceria de la costa'), ('Tienda1'), ('Tienda2');

INSERT INTO product_types(nombre, descripción) VALUES ('Gaseosas', 'Bebida azucarada con colorantes artificiales y gas'), ('Dulces', 'Comida hecha a base de azucares'), ('Enlatados', 'Productos alimenticios almacenados en latas'); 

 INSERT INTO products(nombre, precio, proveedor_id, tipoproducto_id) VALUES ('Bolita de leche', 5000.00, 1, 2), ('Coca cola', 3000.00, 2, 1), ('Cocada', 6000.00, 1, 2);

 INSERT INTO products(nombre, precio, proveedor_id, tipoproducto_id) VALUES ('Frijoles', 5000.00, 3, 3);

INSERT INTO products(nombre, precio, proveedor_id, tipoproducto_id) VALUES ('Sprite', 3000.00, 2, 1), ('Salchichas', 6000.00, 3, 3), ('Maíz', 8000.00, 3, 3);

 INSERT INTO address(dirección) VALUES ('Carrera 9 #6i'), ('Avenida inventada 22'), ('Çalle 7 transversal 11');

 INSERT INTO address(dirección) VALUES ('Carrera 10 #6y'),  ('Calle 10 #6y'), ('Carrera 10 #13j');

INSERT INTO countries(country_name) VALUES ('Colombia'), ('Venezuela'), ('Ecuador');

INSERT INTO regions(region_name, country_id) VALUES ('Sucre',1), ('Cordoba',1), ('Cesar',1);

INSERT INTO cities(city_name, region_id) VALUES ('Sincelejo', 1), ('Monteria', 2), ('Curumaní', 3);

INSERT INTO zip_code(city_id, codigo) VALUES (1, 'm12345'), (2, 'e12345'), (3, 'b12345');

INSERT INTO locations(cliente_id, direccion_id, codigopostal_id) VALUES ('1104260414', 1, 1), ('18973450', 2, 2), ('64574575', 3, 3);

INSERT INTO order_detail(pedido_id, producto_id, cantidad, precio) VALUES (1, 1, 5, 25000.00), (2, 3, 2, 12000.00), (3, 3, 5, 30000.00); 

INSERT INTO order_detail(pedido_id, producto_id, cantidad, precio) VALUES (2, 2, 2, 6000.00), (4, 1, 2, 10000.00), (3, 4, 1, 5000.00), (4, 3, 5, 30000.00); 

INSERT INTO order_detail(pedido_id, producto_id, cantidad, precio) VALUES (3, 6, 1, 8000.00), (1, 6, 1, 8000.00), (3, 5, 1, 6000.00);

INSERT INTO locations(proveedor_id, direccion_id, codigopostal_id) VALUES (1, 4, 1), (2, 5, 3), (3, 6, 3);

## JOINS

**1. Obtener la lista de todos los pedidos con los nombres de clientes usando INNER JOIN .**

SELECT 
  o.id AS pedido_id, 
  o.fecha, 
  c.nombre AS cliente_nombre, 
  e.id AS empleado_id,
  ed.nombre AS empleado_nombre
FROM orders AS o
INNER JOIN customer AS c ON c.id = o.cliente_id
INNER JOIN empleoyees AS e ON e.id = o.empleado_id
INNER JOIN empleoyee_data AS ed ON ed.empleado_id = e.id;

**2. Listar los productos y proveedores que los suministran con INNER JOIN .**

SELECT p.id,
 p.nombre,
 p.precio, 
 tp.nombre AS tipo_producto, 
 pr.nombre AS proveedor
FROM products AS p
INNER JOIN product_types AS tp ON tp.id = p.tipoproducto_id
INNER JOIN suppliers AS pr ON pr.id = p.proveedor_id;

**3. Mostrar los pedidos y las ubicaciones de los clientes con LEFT JOIN .**

SELECT 
  o.id AS pedido_id, 
  o.fecha, 
  c.nombre AS cliente_nombre, 
  d.dirección AS dirección_cliente,
  co.codigo AS codigoPostal_cliente,
  ci.city_name AS ciudad_cliente,
  r.region_name AS estado_cliente,
  p.country_name AS pais_cliente,
  e.id AS empleado_id,
  ed.nombre AS empleado_nombre
FROM orders AS o
LEFT JOIN customer AS c ON c.id = o.cliente_id
LEFT JOIN empleoyees AS e ON e.id = o.empleado_id
LEFT JOIN empleoyee_data AS ed ON ed.empleado_id = e.id
LEFT JOIN locations AS l ON l.cliente_id = c.id
LEFT JOIN address AS d ON d.id = l.direccion_id
LEFT JOIN zip_code AS co ON co.id = l.codigopostal_id
LEFT JOIN cities AS ci ON ci.id = co.city_id
LEFT JOIN regions AS r ON r.id = ci.region_id
LEFT JOIN countries AS p ON p.id = r.country_id;

**4. Consultar los empleados que han registrado pedidos, incluyendo empleados sin pedidos ( LEFT JOIN ).**

SELECT
  e.id AS empleado_id,
  ed.nombre AS empleado_nombre,
  p.cargo AS cargo_empleado,
  o.id AS pedido_id
FROM empleoyees AS e
LEFT JOIN empleoyee_data AS ed ON ed.empleado_id = e.id
LEFT JOIN empleoyee_position AS ep ON ep.empleado_id = e.id
LEFT JOIN positions AS p ON p.id = ep.cargo_id
LEFT JOIN orders AS o ON o.empleado_id = e.id;

**5. Obtener el tipo de producto y los productos asociados con INNER JOIN .**

SELECT
  tp.id AS id_tipoProducto,
  tp.nombre AS tipo_producto,
  p.id AS id_producto,
  p.nombre AS producto
FROM product_types AS tp
INNER JOIN products AS p ON p.tipoproducto_id = tp.id;

**6. Listar todos los clientes y el número de pedidos realizados con COUNT y GROUP BY .**

SELECT 
  c.id AS cliente_id,
  c.nombre AS cliente,
  COUNT(o.id) AS numero_pedidos
FROM customer AS c
LEFT JOIN orders o ON o.cliente_id = c.id
GROUP BY c.id, c.nombe;

**7. Combinar Pedidos y Empleados para mostrar qué empleados gestionaron pedidos específicos.**

SELECT
  o.id AS pedido,
  e.id AS id_empleado,
  ed.nombre AS nombre_empleado
FROM orders AS o
INNER JOIN empleoyees AS e ON e.id = o.empleado_id
INNER JOIN empleoyee_data AS ed ON ed.empleado_id = e.id;

**8. Mostrar productos que no han sido pedidos ( RIGHT JOIN ).**

SELECT
  p.id AS producto_id,
  p.nombre AS nombre_producto
FROM order_detail AS od
RIGHT JOIN products AS p ON p.id = od.producto_id
WHERE od.producto_id IS NULL;

**9. Mostrar el total de pedidos y ubicación de clientes usando múltiples JOIN .**

SELECT 
  c.id AS cliente_id,
  c.nombre AS cliente,
  d.dirección AS dirección_cliente,
  co.codigo AS codigoPostal_cliente,
  ci.city_name AS ciudad_cliente,
  r.region_name AS estado_cliente,
  p.country_name AS pais_cliente,
  COUNT(o.id) AS numero_pedidos
FROM customer AS c
LEFT JOIN orders o ON o.cliente_id = c.id
LEFT JOIN locations AS l ON l.cliente_id = c.id
LEFT JOIN address AS d ON d.id = l.direccion_id
LEFT JOIN zip_code AS co ON co.id = l.codigopostal_id
LEFT JOIN cities AS ci ON ci.id = co.city_id
LEFT JOIN regions AS r ON r.id = ci.region_id
LEFT JOIN countries AS p ON p.id = r.country_id
GROUP BY c.id, c.nombre, d.dirección, co.codigo, ci.city_name, r.region_name, p.country_name;

**10. Unir Proveedores , Productos , y TiposProductos para un listado completo de inventario.**

SELECT 
  p.id AS producto_id,
  p.nombre AS nombre_producto,
  p.precio,
  pr.nombre AS proveedor,
  tp.nombre AS tipo_producto
FROM products AS p
INNER JOIN suppliers AS pr ON pr.id = p.proveedor_id
INNER JOIN product_types AS tp ON tp.id = p.tipoproducto_id;

## Consultas Simples

**1. Seleccionar todos los productos con precio mayor a $50.**

SELECT
  p.id,
  p.nombre AS producto,
  p.precio
FROM products AS p
WHERE p.precio > 50;

**2. Consultar clientes registrados en una ciudad específica.**

SELECT 
  ci.city_name,
  c.id,
  c.nombre AS nombre_cliente
FROM customer AS c
INNER JOIN locations AS l ON l.cliente_id = c.id
INNER JOIN zip_code AS zc ON zc.id = l.codigopostal_id
INNER JOIN cities AS ci ON ci.id = zc.city_id
WHERE city_name = 'Sincelejo';

**3. Mostrar empleados contratados en los últimos 2 años.**

SELECT 
  ed.id,
  ed.nombre AS nombre_empleado,
  ed.fecha_contrato
FROM empleoyee_data AS ed
INNER JOIN empleoyees AS e ON e.id = ed.empleado_id
WHERE ed.fecha_contrato >= CURDATE() -INTERVAL 2 YEAR;

**4. Seleccionar proveedores que suministran más de 5 productos.**

SELECT 
  pr.id AS proveedor_id,
  pr.nombre AS proveedor,
  COUNT(p.id) AS total_productos
FROM suppliers AS pr
INNER JOIN products AS p ON p.proveedor_id = pr.id
GROUP BY pr.id, pr.nombre
HAVING COUNT(p.id) > 5;

**5. Listar clientes que no tienen dirección registrada en UbicacionCliente .**

SELECT
  c.id,
  c.nombre AS cliente
FROM customer AS c
LEFT JOIN locations AS l ON l.cliente_id = c.id
WHERE l.cliente_id IS NULL;

**6. Calcular el total de ventas por cada cliente.**

SELECT
  c.id,
  c.nombre AS cliente,
  COUNT(o.id) AS total_ventas
FROM customer AS c
INNER JOIN orders AS o ON o.cliente_id = c.id
GROUP BY c.id, c.nombre;

**7. Mostrar el salario promedio de los empleados.**

SELECT 
  AVG(ed.salario) AS promedio_salario
FROM empleoyee_data AS ed
INNER JOIN empleoyees AS e ON e.id = ed.empleado_id;

**8. Consultar el tipo de productos disponibles en TiposProductos .**

SELECT 
  tp.id,
  tp. nombre AS tipos_productos
FROM product_types AS tp;

**9. Seleccionar los 3 productos más caros.**

SELECT 
  id,
  nombre AS producto,
  precio
FROM products
ORDER BY precio DESC
LIMIT 3;

**10. Consultar el cliente con el mayor número de pedidos.**

SELECT 
  c.id,
  c.nombre AS cliente,
  COUNT(o.id) AS numero_pedidos
FROM customer AS c
INNER JOIN orders AS o ON o.cliente_id = c.id
GROUP BY c.id, c.nombre
ORDER BY numero_pedidos DESC
LIMIT 1;

## Consultas Multitabla

**1. Listar todos los pedidos y el cliente asociado.**

SELECT 
  o.id AS id_pedido,
  o.fecha,
  c.nombre AS cliente
FROM orders AS o
INNER JOIN customer AS c ON c.id = o.cliente_id;

**2. Mostrar la ubicación de cada cliente en sus pedidos.**

SELECT 
  o.id AS pedido_id, 
  o.fecha, 
  c.nombre AS cliente_nombre, 
  d.dirección AS dirección_cliente,
  co.codigo AS codigoPostal_cliente,
  ci.city_name AS ciudad_cliente,
  r.region_name AS estado_cliente,
  p.country_name AS pais_cliente
FROM orders AS o
INNER JOIN customer AS c ON c.id = o.cliente_id
INNER JOIN locations AS l ON l.cliente_id = c.id
INNER JOIN address AS d ON d.id = l.direccion_id
INNER JOIN zip_code AS co ON co.id = l.codigopostal_id
INNER JOIN cities AS ci ON ci.id = co.city_id
INNER JOIN regions AS r ON r.id = ci.region_id
INNER JOIN countries AS p ON p.id = r.country_id;

**3. Listar productos junto con el proveedor y tipo de producto.**

SELECT 
  p.nombre AS nombre_producto,
  pr.nombre AS proveedor,
  tp.nombre AS tipo_producto
FROM products AS p
INNER JOIN suppliers AS pr ON pr.id = p.proveedor_id
INNER JOIN product_types AS tp ON tp.id = p.tipoproducto_id;

**4. Consultar todos los empleados que gestionan pedidos de clientes en una ciudad específica.**

SELECT 
  ci.city_name,
  e.id AS empleado_id,
  ed.nombre AS nombre_empleado
FROM orders AS o
INNER JOIN empleoyees AS e ON e.id = o.empleado_id
INNER JOIN empleoyee_data AS ed ON ed.empleado_id = e.id
INNER JOIN customer AS c ON c.id = o.cliente_id
INNER JOIN locations AS l ON l.cliente_id = c.id
INNER JOIN zip_code AS zc ON zc.id = l.codigopostal_id
INNER JOIN cities AS ci ON ci.id = zc.city_id
WHERE city_name = 'Sincelejo';

**5. Consultar los 5 productos más vendidos.**

SELECT
  p.id,
  p.nombre AS producto,
  SUM(od.cantidad) cantidad_vendida
FROM products AS p
INNER JOIN order_detail AS od ON od.producto_id = p.id
GROUP BY p.id, p.nombre
ORDER BY cantidad_vendida DESC
LIMIT 5;

**6. Obtener la cantidad total de pedidos por cliente y ciudad.**

SELECT
  c.id AS cliente_id,
  c.nombre AS cliente,
  ci.city_name AS ciudad,
  COUNT(o.id) AS total_pedidos
FROM orders AS o
INNER JOIN customer AS c ON c.id = o.cliente_id
INNER JOIN locations AS l ON l.cliente_id = c.id
INNER JOIN zip_code AS zc ON zc.id = l.codigopostal_id
INNER JOIN cities AS ci ON ci.id = zc.city_id
GROUP BY c.id, c.nombre, ci.city_name;

**7. Listar clientes y proveedores en la misma ciudad.**

SELECT
  ci.city_name AS ciudad,
  c.id AS cliente_id,
  c.nombre AS cliente,
  pr.id AS proveedor_id,
  pr.nombre AS proveedor
FROM locations AS lc
INNER JOIN customer AS c ON c.id = lc.cliente_id
INNER JOIN zip_code AS zc ON zc.id = lc.codigopostal_id
INNER JOIN cities AS ci ON ci.id = zc.city_id
INNER JOIN locations AS lp ON lp.codigopostal_id = lc.codigopostal_id
INNER JOIN suppliers AS pr ON pr.id = lp.proveedor_id;

**8. Mostrar el total de ventas agrupado por tipo de producto.**

SELECT 
  tp.id AS id_tipoproducto,
  tp.nombre AS tipo_producto,
  SUM(od.cantidad) AS ventas_totales
FROM product_types AS tp
INNER JOIN products AS p ON p.tipoproducto_id = tp.id
INNER JOIN order_detail AS od ON od.producto_id = p.id
GROUP BY tp.id, tp.nombre;

**9. Listar empleados que gestionan pedidos de productos de un proveedor específico.**

SELECT 
  e.id,
  ed.nombre AS empleado
FROM empleoyees AS e
INNER JOIN empleoyee_data AS ed ON ed.empleado_id = e.id
INNER JOIN orders AS o ON o.empleado_id = e.id
INNER JOIN order_detail AS od ON od.pedido_id = o.id
INNER JOIN products AS p ON p.id = od.producto_id
INNER JOIN suppliers AS pr ON pr.id = p.proveedor_id
WHERE pr.nombre = 'Dulceria de la costa'
GROUP BY e.id, ed.nombre;

**10. Obtener el ingreso total de cada proveedor a partir de los productos vendidos.**

SELECT 
  pr.id AS proveedor_id,
  pr.nombre AS proveedor,
  SUM(od.precio * od.cantidad) AS ingreso_total
FROM suppliers AS pr
INNER JOIN products AS p ON p.proveedor_id = pr.id
INNER JOIN order_detail AS od ON od.producto_id = p.id
GROUP BY pr.id, pr.nombre;










