# 🛒 Sistema de Catálogo e Pedidos --- Microsserviços com Spring Boot & Spring Cloud

## 📌 Sobre o projeto

Este projeto é um **sistema simples de catálogo de produtos e simulação
de pedidos**, desenvolvido com **arquitetura de microsserviços**
utilizando **Spring Boot** e **Spring Cloud**.

Ele demonstra: - **Service Discovery** com Eureka - **API Gateway** com
Spring Cloud Gateway - **Comunicação entre serviços** via Feign Client
(HTTP) - **Persistência** com PostgreSQL - **Resiliência** com
Resilience4j (Retry & Circuit Breaker)

------------------------------------------------------------------------

## 🏗 Arquitetura

                   +------------------+
                   |  API Gateway     |
                   | (Spring Cloud)   |
                   +---------+--------+
                             |
        +--------------------+--------------------+
        |                                         |
    +---v---+                               +-----v-----+
    |Catalog|                               |   Order   |
    |Service| <-- consulta/reserva produto --|  Service  |
    +-------+                               +-----------+
        |                                         |
     PostgreSQL                              PostgreSQL

                   +------------------+
                   |  Eureka Server   |
                   |  Service Discovery|
                   +------------------+

------------------------------------------------------------------------

## ⚙️ Microsserviços

### 🔍 Discovery Service

-   Implementa **Eureka Server**
-   Responsável pelo **registro e descoberta** dos serviços

### 🚪 API Gateway

-   Baseado em **Spring Cloud Gateway**
-   Faz o **roteamento** das requisições:
    -   `/catalog/**` → **catalog-service**
    -   `/orders/**` → **order-service**

### 📦 Catalog Service

-   CRUD de produtos
-   Reserva de estoque
-   Banco de dados: **PostgreSQL**

### 🧾 Order Service

-   Criação e consulta de pedidos
-   Consome o **Catalog Service** via Feign
-   Implementa **resiliência** com Resilience4j

------------------------------------------------------------------------

## 🛠 Tecnologias

-   **Java 17+**
-   **Spring Boot 3.x**
-   **Spring Cloud 2023.x**
-   **Spring Data JPA**
-   **Spring Cloud Eureka**
-   **Spring Cloud Gateway**
-   **Spring Cloud OpenFeign**
-   **Resilience4j**
-   **PostgreSQL**
-   **Docker Compose**

------------------------------------------------------------------------

## ▶️ Como rodar o projeto

### Pré-requisitos

-   **Java 17+**
-   **Maven**
-   **Docker + Docker Compose**

### Passo 1 --- Subir bancos de dados

``` bash
docker-compose up -d
```

### Passo 2 --- Rodar os microsserviços

Na ordem:

1.  `discovery-service` (porta **8761**)\
2.  `api-gateway` (porta **8080**)\
3.  `catalog-service` e `order-service` (portas aleatórias registradas
    no Eureka)

### Passo 3 --- Testar endpoints

-   Criar produto:

``` http
POST http://localhost:8080/catalog/products
Content-Type: application/json

{
  "name": "Teclado Mecânico",
  "description": "Layout ABNT2",
  "price": 350.0,
  "stock": 20
}
```

-   Listar produtos:

``` http
GET http://localhost:8080/catalog/products
```

-   Criar pedido:

``` http
POST http://localhost:8080/orders
Content-Type: application/json

{
  "items": [
    { "productId": "ID_DO_PRODUTO", "quantity": 2 }
  ]
}
```

-   Buscar pedido:

``` http
GET http://localhost:8080/orders/{id}
```

------------------------------------------------------------------------
