# Revisao de OO e SQL - by Prof. Nelio Alves

# Enorme agradecimento de poder realizar uma revisao com esse alto grau de conhecimento.
# --------------- https://github.com/Rogpe-Prog --------------------------

## Nesta aula vamos revisar
- Chave primaria, chave estrangeira
- DDL (create table, alter table)
- SQL
  - INSERT
  - SELECT
  - INNER JOIN
- Classes e objetos
- Encapsulamento, get/set
- Tipos enumerados
- Composicao de objetos
- Colecoes (list, map)
- Acessar dados em BD relacional e instanciar objetos correspondentes

## Pre-requisitos
- Git
- JDK
- STS (ou outra IDE)
- Servidor Postgres
- pgAdmin


## Criacao e instanciacao da base de dados
```sql
create table tb_order (
    id int8 generated by default as identity, 
    latitude float8, 
    longitude float8, 
    moment TIMESTAMP WITHOUT TIME ZONE, 
    status int4, 
    primary key (id)
);

create table tb_order_product (
    order_id int8 not null, 
    product_id int8 not null, 
    primary key (order_id, product_id)
);

create table tb_product (
    id int8 generated by default as identity, 
    description TEXT, 
    image_uri varchar(255), 
    name varchar(255), 
    price float8, 
    primary key (id)
);

alter table if exists tb_order_product add constraint fk_tb_order_product_tb_product 
foreign key (product_id) references tb_product;

alter table if exists tb_order_product add constraint fk_tb_order_product_tb_order 
foreign key (order_id) references tb_order;

INSERT INTO tb_product (name, price, image_Uri, description) VALUES 
('Pizza de Calabresa', 50.0, 'https://github.com/1.png', 'Pizza calabresa com queijo, molho e massa especial'),
('Pizza Quatro Queijos', 40.0, 'https://github.com/2.png', 'Pizza quatro queijos muito boa'),
('Pizza de Escarola', 60.0, 'https://github.com/3.png', 'Pizza escarola muito boa');

INSERT INTO tb_order (status, latitude, longitude, moment) VALUES 
(0, 213123, 12323, TIMESTAMP WITH TIME ZONE '2021-01-04T11:00:00Z'),
(1, 3453453, 3534534, TIMESTAMP WITH TIME ZONE '2021-01-05T11:00:00Z');

INSERT INTO tb_order_product (order_id, product_id) VALUES 
(1 , 1),
(1 , 2),
(2 , 2),
(2 , 3);
```

## Consulta para recuperar os pedidos com seus produtos
```sql
SELECT * FROM tb_order
INNER JOIN tb_order_product ON tb_order.id = tb_order_product.order_id
INNER JOIN tb_product ON tb_product.id = tb_order_product.product_id
```
