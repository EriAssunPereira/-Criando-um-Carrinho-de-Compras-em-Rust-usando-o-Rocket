# Criando-um-Carrinho-de-Compras-em-Rust-usando-o-Rocket

Desenvolver um carrinho de compras funcional em Rust utilizando o framework web Rocket é um excelente exercício para aprender sobre desenvolvimento web em Rust, manipulação de dados e persistência básica. Vamos estruturar o projeto em módulos e discutir cada aspecto relevante para criar uma aplicação web robusta e eficiente.

### 1. Configuração do Ambiente

#### Instalação das Ferramentas Necessárias

Certifique-se de ter Rust e Cargo instalados no seu sistema. Além disso, você precisará configurar o ambiente de desenvolvimento para trabalhar com Rocket.

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

### 2. Criando o Projeto Rocket

#### Inicialização do Projeto

Crie um novo projeto Rust utilizando Cargo:

```bash
cargo new rust_shopping_cart
cd rust_shopping_cart
```

Adicione Rocket como dependência no arquivo `Cargo.toml`:

```toml
[dependencies]
rocket = "0.5.0-rc.1"
```

### 3. Estrutura do Projeto

Organize o projeto em módulos para facilitar a manutenção e escalabilidade:

- **src/**
  - **main.rs**: Ponto de entrada da aplicação Rocket.
  - **routes/**: Módulo para definir as rotas da aplicação.
  - **models/**: Definição de estruturas de dados (por exemplo, para produtos e carrinho).
  - **services/**: Lógica de negócios e serviços relacionados ao carrinho.

### 4. Implementação do Carrinho de Compras

#### Definição das Rotas com Rocket

No arquivo `main.rs`, configure as rotas principais usando Rocket:

```rust
#[macro_use] extern crate rocket;

mod routes;
mod models;
mod services;

#[rocket::main]
async fn main() {
    rocket::build()
        .mount("/", routes![
            routes::get_cart,
            routes::add_to_cart,
            routes::remove_from_cart,
        ])
        .launch()
        .await
        .expect("Failed to launch Rocket application.");
}
```

#### Definição das Estruturas de Dados (Models)

Crie estruturas de dados simples para representar produtos e o carrinho de compras em `models/cart.rs`:

```rust
pub struct Product {
    pub id: u32,
    pub name: String,
    pub price: f32,
}

pub struct CartItem {
    pub product: Product,
    pub quantity: u32,
}

pub struct ShoppingCart {
    pub items: Vec<CartItem>,
    pub total: f32,
}
```

#### Implementação das Rotas (Routes)

Defina as rotas em `routes/cart.rs` para manipular o carrinho de compras:

```rust
use rocket::{get, post, delete};
use rocket::serde::json::Json;
use crate::models::{Product, CartItem, ShoppingCart};

#[get("/cart")]
pub async fn get_cart() -> Json<ShoppingCart> {
    // Implementação para retornar o carrinho de compras
}

#[post("/cart/add/<product_id>")]
pub async fn add_to_cart(product_id: u32) -> Json<ShoppingCart> {
    // Implementação para adicionar um produto ao carrinho
}

#[delete("/cart/remove/<product_id>")]
pub async fn remove_from_cart(product_id: u32) -> Json<ShoppingCart> {
    // Implementação para remover um produto do carrinho
}
```

#### Implementação da Lógica de Negócios (Services)

Crie funções em `services/cart.rs` para manipular o carrinho de compras:

```rust
use crate::models::{Product, CartItem, ShoppingCart};

pub fn add_product_to_cart(cart: &mut ShoppingCart, product: Product, quantity: u32) {
    // Implementação para adicionar um produto ao carrinho
}

pub fn remove_product_from_cart(cart: &mut ShoppingCart, product_id: u32) {
    // Implementação para remover um produto do carrinho
}

pub fn calculate_cart_total(cart: &ShoppingCart) -> f32 {
    // Implementação para calcular o total do carrinho
}
```

### 5. Testes

#### Implementação de Testes Unitários

Utilize o framework de testes integrado ao Rust para testar as funções e lógicas de negócio do carrinho de compras:

```rust
#[cfg(test)]
mod tests {
    use super::*;
    
    #[test]
    fn test_add_product_to_cart() {
        let mut cart = ShoppingCart {
            items: vec![],
            total: 0.0,
        };
        let product = Product {
            id: 1,
            name: "Product 1".to_string(),
            price: 10.0,
        };
        add_product_to_cart(&mut cart, product.clone(), 2);
        assert_eq!(cart.items.len(), 1);
        assert_eq!(cart.items[0].product.name, "Product 1");
        assert_eq!(cart.total, 20.0);
    }
}
```

### 6. Conclusão

Ao seguir esses passos e exemplos de código, criaremos um carrinho de compras funcional em Rust utilizando o framework web Rocket. Devemos certificarmos de explorar mais recursos do Rocket, como templates para renderização HTML, autenticação de usuários, entre outros, para expandir e melhorar sua aplicação. Este projeto proporciona uma excelente base para entender como Rust pode ser utilizado no desenvolvimento web de forma eficiente e segura.
