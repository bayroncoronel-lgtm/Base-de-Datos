# INFORME DE FIN DE UNIDAD

## Universidad Nacional de Loja
**Facultad de la Energía, las Industrias y los Recursos Naturales No Renovables**

**Carrera:** Computación

---

### 1. Datos generales

El presente informe corresponde al trabajo desarrollado durante la primera unidad de la asignatura de Base de Datos.

**Asignatura:** Base de Datos  
**Ciclo:** 3A  
**Unidad:** 1  

**Docente:** Edwin René Guamán Quinche

**Estudiante:** Bayron Alexander Coronel Sarango

**Fecha:** 10 de noviembre de 2026


# 1. TÍTULO

**Implementación de un sistema de inventario para la Gestión de un negocio de micheladas**

# 2. CONTEXTO DEL PROBLEMA

En un negocio dedicado a la venta de micheladas se necesita llevar un buen control de los productos, ingredientes, pedidos, pagos, promociones y compras a los proveedores. Sin embargo, cuando toda esta información se registra manualmente en cuadernos o en hojas sueltas, pueden surgir varios problemas, como no saber cuánto stock queda de cada ingrediente, quedarse sin algún producto durante la atención, perder información de las compras o pedidos y cometer errores al calcular los valores y descuentos. Esto afecta principalmente al propietario y al personal del negocio, ya que pierden tiempo y pueden tener pérdidas económicas por una mala organización. También afecta a los clientes, porque pueden recibir una atención más lenta o tener errores en sus pedidos, y a los proveedores, porque no se mantiene un registro ordenado de las compras realizadas.
Por esta razón, se necesita organizar toda la información del negocio mediante un sistema que permita registrar las categorías, productos, ingredientes y recetas de cada michelada, además de controlar las cantidades disponibles, las entradas y salidas del inventario, los clientes, pedidos, pagos, promociones y compras a proveedores. Esto es importante porque permitirá conocer qué productos e ingredientes están disponibles, evitar que se desperdicien o se terminen, llevar un mejor control de los ingresos y gastos, y tener información clara para tomar mejores decisiones. De esta manera, el negocio podrá trabajar de forma más ordenada y brindar una mejor atención a sus clientes.

# 3. OBJETIVOS

## 3.1. Objetivo General

Diseñar e implementar una base de datos relacional en MySQL para un negocio dedicado a la venta de micheladas, utilizando el modelo entidad relación, el modelo relacional y SQL, con el propósito de organizar y llevar un mejor control de los productos, ingredientes, pedidos, pagos, promociones, compras e inventario, facilitando así la administración y la toma de decisiones del negocio.

## 3.2. Objetivos Específicos

1. Elaborar el diagrama entidad-relación y el modelo relacional de la base de datos, identificando las entidades, atributos, claves primarias, claves foráneas y relaciones, para organizar correctamente la información del negocio y evitar datos repetidos.

2. Implementar la estructura de la base de datos en MySQ, creando todas las tablas con sus respectivas claves primarias, claves foráneas y restricciones, para establecer correctamente las relaciones entre los datos y facilitar la gestión de la información del negocio.

# 4. MODELOS CONCEPTUALES DE BASES DE DATOS

## 4.1. Modelo Entidad-Relación (E-R)


<img width="975" height="543" alt="image" src="https://github.com/user-attachments/assets/d6f9e65b-3992-4d07-beee-1a05d0c2df75" />


**Figura 1.** Diagrama Entidad-Relación del sistema de gestión de inventario para un negocio de micheladas.

## 4.2. Modelo Relacional


<img width="975" height="812" alt="image" src="https://github.com/user-attachments/assets/ec7fba31-3617-443b-941a-8fb13cc6b6f5" />


**Figura 2.** Modelo Relacional del sistema de gestión de inventario para un negocio de micheladas.

# 5. CÓDIGO SQL

## 5.1. Creación de Tablas (DDL)

CREATE DATABASE IF NOT EXISTS sistema_micheladas

CHARACTER SET utf8mb4

COLLATE utf8mb4_unicode_ci;

USE sistema_micheladas;

CREATE TABLE CATEGORIA (
    
    id_categoria INT AUTO_INCREMENT PRIMARY KEY,
    
    nombre VARCHAR(50) NOT NULL,
    
    descripcion VARCHAR(150),
    
    estado BOOLEAN DEFAULT TRUE
    
);

CREATE TABLE PRODUCTO (

    id_producto INT AUTO_INCREMENT PRIMARY KEY,
    
    id_categoria INT NOT NULL,
    
    nombre VARCHAR(100) NOT NULL,
    
    descripcion VARCHAR(200),
    
    precio_venta DECIMAL(10,2) NOT NULL,
    
    estado BOOLEAN DEFAULT TRUE,

    FOREIGN KEY (id_categoria)
    
    REFERENCES Categoria(id_categoria),

    CHECK (precio_venta >= 0)
);

CREATE TABLE INGREDIENTE (

    id_ingrediente INT AUTO_INCREMENT PRIMARY KEY,
    
    nombre VARCHAR(100) NOT NULL,
    
    unidad_medida VARCHAR(20) NOT NULL,
    
    stock_actual DECIMAL(10,2) DEFAULT 0,
    
    stock_minimo DECIMAL(10,2) DEFAULT 0,
    
    estado BOOLEAN DEFAULT TRUE,

    CHECK (stock_actual >= 0),
    
    CHECK (stock_minimo >= 0)
);



CREATE TABLE PRODUCTOINGREDIENTE (

    id_producto INT NOT NULL,
    
    id_ingrediente INT NOT NULL,
    
    cantidad DECIMAL(10,2) NOT NULL,

    PRIMARY KEY (id_producto, id_ingrediente),

    FOREIGN KEY (id_producto)
    
    REFERENCES Producto(id_producto),

    FOREIGN KEY (id_ingrediente)
    
    REFERENCES Ingrediente(id_ingrediente),

    CHECK (cantidad > 0)
);

CREATE TABLE PROMOCION (

    id_promocion INT AUTO_INCREMENT PRIMARY KEY,
    
    nombre VARCHAR(100) NOT NULL,
    
    descripcion VARCHAR(200),
    
    tipo_descuento VARCHAR(20) NOT NULL,
    
    valor_descuento DECIMAL(10,2) NOT NULL,
    
    fecha_inicio DATE NOT NULL,
    
    fecha_fin DATE NOT NULL,

    CHECK (valor_descuento >= 0),
    
    CHECK (fecha_fin >= fecha_inicio)
);

## 5.2. Inserción de Datos (DML)

### Categoria

INSERT INTO CATEGORIA (nombre, descripcion) VALUES

('Micheladas', 'Micheladas clasicas y especiales'),

('Cervezas', 'Cervezas nacionales y artesanales'),

('Bebidas sin alcohol', 'Gaseosas y aguas'),

('Snacks', 'Acompanantes para las bebidas'),

('Combos', 'Promociones de varios productos');



### Producto
 
 
INSERT INTO PRODUCTO (id_categoria, nombre, descripcion, precio_venta, estado) VALUES

(1, 'Michelada clasica', 'Cerveza, limon, sal y salsa inglesa', 3.50, TRUE),

(1, 'Michelada especial', 'Con clamato y chamoy', 4.50, TRUE),

(1, 'Michelada de mango', 'Con pulpa de mango y chamoy', 5.00, TRUE),

(1, 'Michelada de camaron', 'Con clamato y camarones', 6.50, TRUE),

(2, 'Cerveza nacional', 'Botella de 355 ml', 2.00, TRUE),

(2, 'Cerveza artesanal', 'Botella de 330 ml', 3.50, FALSE),

(3, 'Gaseosa', 'Botella individual', 1.00, TRUE),

(3, 'Agua mineral', 'Botella de 500 ml', 1.25, TRUE),

(4, 'Papas fritas', 'Porcion individual', 1.50, TRUE),

(5, 'Combo michelada y papas', 'Michelada clasica con papas fritas', 4.50, TRUE);
 


### Ingrediente


 
INSERT INTO INGREDIENTE (nombre, unidad_medida, stock_actual, stock_minimo, estado) VALUES

('Cerveza', 'botella', 60.00, 20.00, TRUE),

('Limon', 'unidad', 150.00, 40.00, TRUE),

('Sal', 'kg', 5.00, 2.00, TRUE),

('Salsa inglesa', 'litro', 3.00, 1.00, TRUE),

('Clamato', 'litro', 12.00, 5.00, TRUE),

('Pulpa de mango', 'litro', 0.40, 1.00, TRUE),

('Chamoy', 'litro', 2.50, 1.00, TRUE),

('Camaron', 'kg', 0.25, 0.50, TRUE);



## CONSULTA DE ÁLGEBRA RELACIONAL Y TRADUCIR A SQL

### Consulta 1 — Unión (∪)

### Descripción: 

Obtener un directorio único de contactos (nombre, teléfono y correo) que reúna a todos los clientes y a los proveedores activos. Se usa un renombrado (ρ) para que las dos relaciones tengan los mismos nombres de atributos [1] [2].

### Álgebra Relacional:

ρ nombre←nombres ( π nombres, telefono, correo (Cliente) )

   ∪
   
π nombre, telefono, correo ( σ estado = TRUE (Proveedor) )

### SQL:

SELECT nombres AS nombre, telefono, correo

FROM   Cliente

UNION

SELECT nombre, telefono, correo

FROM   Proveedor

WHERE  estado = TRUE

ORDER BY nombre;

### Resultado esperado: 

10 registros en total conformados por los 6 clientes y los 4 proveedores activos. Mariscos del Pacífico no aparece porque está inactivo.



<img width="975" height="614" alt="image" src="https://github.com/user-attachments/assets/4c430d12-7323-4390-b92e-5e02b2008e46" />




## Consulta 2 — Reunión natural (⋈)

### Descripción: 

Listar el detalle de los pedidos ya entregados, mostrando el cliente, la fecha, el producto, la cantidad y el subtotal. Combina Pedido, Cliente, DetallePedido y Producto mediante sus llaves foráneas [1] [2].

### Álgebra Relacional:

π c.nombres, c.apellidos, pe.id_pedido, pe.fecha_pedido, pr.nombre, dp.cantidad, dp.subtotal

   ( σ pe.estado = 'Entregado'
   
      ( ( ( Pedido pe ⋈ pe.id_cliente = c.id_cliente Cliente c )
      
            ⋈ pe.id_pedido = dp.id_pedido DetallePedido dp )
            
            ⋈ dp.id_producto = pr.id_producto Producto pr ) )

### SQL:

SELECT c.nombres, c.apellidos, pe.id_pedido, pe.fecha_pedido,

       pr.nombre AS producto, dp.cantidad, dp.subtotal
       
FROM   Pedido pe

JOIN   Cliente c        ON pe.id_cliente = c.id_cliente

JOIN   DetallePedido dp ON pe.id_pedido = dp.id_pedido

JOIN   Producto pr      ON dp.id_producto = pr.id_producto

WHERE  pe.estado = 'Entregado'

ORDER BY pe.fecha_pedido;    

### Resultado esperado: 

10 filas, una por cada producto de los pedidos entregados (1, 2, 3, 6 y 8). Por ejemplo, Ana Perez con 2 Michelada clasica por 7.00. Quedan fuera los pedidos pendiente, en preparación y cancelado.




<img width="975" height="469" alt="image" src="https://github.com/user-attachments/assets/cfbd517c-2345-4f3b-88e0-6eec87c1f7ef" />




## Consulta 3 — Diferencia (−)

### Descripción: 

Encontrar los productos que nunca se han vendido. Al conjunto de todos los productos se le restan los que aparecen en algún detalle de pedido [1] [2].

### Álgebra Relacional:

π id_producto, nombre (Producto)

   −
   
π id_producto, nombre (Producto ⋈ DetallePedido)

### SQL:

SELECT id_producto, nombre

FROM   Producto

WHERE  id_producto NOT IN (SELECT id_producto

                           FROM   DetallePedido);

### Resultado esperado: 

1 registro, Cerveza artesanal (id 6), que es el único producto que no aparece en ningún pedido.



<img width="820" height="306" alt="image" src="https://github.com/user-attachments/assets/138fc246-3f2f-45d1-acd3-2a4501bfcca0" />



## Consulta 4 — Intersección (∩)

### Descripción: 

Obtener los ingredientes que tienen en común la Michelada clásica (id 1) y la Michelada especial (id 2). Se intersectan los ingredientes de la receta de cada una y luego se une con Ingrediente para mostrar el nombre [1] [2].

### Álgebra Relacional:

π nombre ( Ingrediente ⋈

    ( π id_ingrediente ( σ id_producto = 1 (ProductoIngrediente) )
    
         ∩
         
      π id_ingrediente ( σ id_producto = 2 (ProductoIngrediente) ) ) )

### SQL:

SELECT i.nombre

FROM   Ingrediente i

WHERE  i.id_ingrediente IN (SELECT id_ingrediente

                            FROM   ProductoIngrediente
                            
                            WHERE  id_producto = 1)
                            
AND    i.id_ingrediente IN (SELECT id_ingrediente

                            FROM   ProductoIngrediente
                            
                            WHERE  id_producto = 2);

### Resultado esperado: 

3 registros de Cerveza, Limon y Sal. Son los ingredientes que llevan ambas micheladas. La salsa inglesa es solo de la clásica, y el clamato y el chamoy solo de la especial.



<img width="949" height="478" alt="image" src="https://github.com/user-attachments/assets/279dde50-2bca-41f5-b218-1db0ac608121" />



## Consulta 5 — Agrupamiento y agregación (γ)

### Descripción: 

Calcular, por producto, las unidades vendidas y el total vendido en pedidos entregados, mostrando solo los productos con ventas de 5 dólares o más, del que más vendió al que menos [3].

### Álgebra Relacional:

σ total_vendido >= 5

   ( γ pr.nombre ; SUM(dp.cantidad) → unidades_vendidas, SUM(dp.subtotal) → total_vendido
   
        ( σ pe.estado = 'Entregado'
        
             ( ( DetallePedido dp ⋈ dp.id_pedido = pe.id_pedido Pedido pe )
             
                  ⋈ dp.id_producto = pr.id_producto Producto pr ) ) )

### SQL:

SELECT   pr.nombre AS producto,

         SUM(dp.cantidad) AS unidades_vendidas,
         
         SUM(dp.subtotal) AS total_vendido
         
FROM     DetallePedido dp

JOIN     Pedido pe   ON dp.id_pedido = pe.id_pedido

JOIN     Producto pr ON dp.id_producto = pr.id_producto

WHERE    pe.estado = 'Entregado'

GROUP BY pr.id_producto, pr.nombre

HAVING   SUM(dp.subtotal) >= 5

ORDER BY total_vendido DESC;

### Resultado esperado: 

3 registros de Michelada clasica (5 unidades, 17.50), Cerveza nacional (5 unidades, 10.00) y Michelada de mango (1 unidad, 5.00). Los demás productos no llegan a 5 dólares y los filtra el HAVING.




<img width="903" height="558" alt="image" src="https://github.com/user-attachments/assets/e44ea062-6578-4828-8757-c6ea5a68396c" />



# CONCLUSIONES

1.	La elaboración del diagrama entidad-relación y del modelo relacional permitió comprender la importancia de organizar correctamente la información antes de empezar a crear la base de datos. Durante este proceso se identificaron las principales entidades del negocio de mi-cheladas, como productos, ingredientes, pedidos, compras y proveedores, y se estableció cómo se relacionan entre sí. Una de las principales dificultades fue definir correctamente las cardinalidades y las tablas intermedias, especialmente en las relaciones entre productos e ingredientes y entre promociones y productos. También fue necesario corregir algunas rela-ciones y agregar llaves foráneas para mantener la conexión entre las tablas. Con esto se lo-gró organizar la información de manera lógica, evitando datos repetidos y cumpliendo con el primer objetivo específico del proyecto.

2.	La creación de la base de datos en MySQL permitió poner en práctica el diseño realizado anteriormente y comprender cómo se construyen las tablas mediante instrucciones SQL. Durante la implementación se aprendió que es importante respetar el orden de creación de las tablas y de inserción de los datos, debido a que algunas dependen de otras mediante lla-ves foráneas. También se utilizaron restricciones como PRIMARY KEY, FOREIGN KEY y CHECK, que ayudan a mantener la información correcta y evitar errores en los datos regis-trados. Además, se pudo comprender la importancia de elegir adecuadamente los tipos de datos para cada atributo. Finalmente, la implementación permitió comprobar que las tablas y sus relaciones funcionaran correctamente, cumpliendo así con el segundo objetivo especí-fico del proyecto.

3.	El desarrollo de las consultas de álgebra relacional permitió comprender cómo obtener in-formación a partir de los datos almacenados en la base de datos. Mediante operadores como unión, intersección, diferencia, selección, proyección y reunión, fue posible plantear dife-rentes consultas para resolver necesidades del negocio. También se comprendió cómo estos operadores se relacionan con las instrucciones SQL, facilitando la elaboración de consultas sobre varias tablas. Entre los resultados obtenidos se encuentran la identificación de los productos más vendidos, la consulta de los pedidos realizados por los clientes y la identifi-cación de productos que no han tenido ventas. De esta manera, se comprobó que la base de datos no solo sirve para almacenar información, sino también para consultarla y obtener re-sultados que pueden ayudar en la administración del negocio y en la toma de decisiones.

# BIBLIOGRAFÍA

[1] R. Ramakrishnan, "Relational algebra," Database Management Systems, Module 3, Lecture 1, notas de clase, CS 143, Univ. of California, Los Angeles, otoño 2009. Accedido: 10 nov. 2026. [En línea]. Disponible: https://web.cs.ucla.edu/classes/fall09/cs143/notes/RA.pdf 

[2] M. T. Özsu, "Relational algebra," notas de clase, CS 348 Introduction to Database Management, Cheriton School of Computer Science, Univ. of Waterloo, otoño 2012. Accedido: 10 nov. 2026. [En línea]. Disponible: https://cs.uwaterloo.ca/~tozsu/courses/CS338/notes/4a-algebra-handout-notes.pdf 

[3] Wikipedia contributors, "Relational algebra," Wikipedia, The Free Encyclopedia. Accedido: 10 nov. 2026. [En línea]. Disponible: https://en.wikipedia.org/wiki/Relational_algebra
