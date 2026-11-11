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

INSERT INTO CATEGORIA (nombre, descripcion) VALUES

('Micheladas', 'Micheladas clasicas y especiales'),

('Cervezas', 'Cervezas nacionales y artesanales'),

('Bebidas sin alcohol', 'Gaseosas y aguas'),

('Snacks', 'Acompanantes para las bebidas'),

('Combos', 'Promociones de varios productos');
 
 
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
 
 
INSERT INTO INGREDIENTE (nombre, unidad_medida, stock_actual, stock_minimo, estado) VALUES

('Cerveza', 'botella', 60.00, 20.00, TRUE),

('Limon', 'unidad', 150.00, 40.00, TRUE),

('Sal', 'kg', 5.00, 2.00, TRUE),

('Salsa inglesa', 'litro', 3.00, 1.00, TRUE),

('Clamato', 'litro', 12.00, 5.00, TRUE),

('Pulpa de mango', 'litro', 0.40, 1.00, TRUE),

('Chamoy', 'litro', 2.50, 1.00, TRUE),

('Camaron', 'kg', 0.25, 0.50, TRUE);
