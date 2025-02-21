# 📦 Sistema de Gerenciamento de Vendas

Este projeto é um banco de dados relacional criado em **MySQL** para gerenciar vendas de uma loja de café. Ele inclui tabelas para clientes, pedidos, produtos e itens do pedido, além de **triggers** para automação de processos.

## 🚀 Funcionalidades

✅ Cadastro de Clientes 📇\
✅ Cadastro de Produtos ☕\
✅ Registro de Pedidos 🛒\
✅ Associação de Produtos aos Pedidos 🔗\
✅ Atualização automática do estoque 📉\
✅ Definição automática do preço unitário 💰

## 🏗 Estrutura do Banco de Dados

O banco de dados foi estruturado da seguinte forma:

### 📌 Tabelas Principais

- **clientes** (Cadastro de clientes)
- **produtos** (Lista de produtos disponíveis)
- **pedidos** (Registro de pedidos realizados)
- **itenspedido** (Produtos incluídos nos pedidos)

### 🔥 Triggers Implementados

1️⃣ **Atualizar Estoque:** Ao inserir um novo item no pedido, o estoque do produto é atualizado automaticamente.
2️⃣ **Atualizar Preço Unitário:** Antes de inserir um item no pedido, o preço é buscado automaticamente na tabela `produtos`.

## 📜 Scripts Utilizados

### 📌 Criação do Banco de Dados e Tabelas

```sql
CREATE DATABASE IF NOT EXISTS GerenciamentoVendas;
USE GerenciamentoVendas;

CREATE TABLE clientes (
    ClienteID INT AUTO_INCREMENT PRIMARY KEY,
    Nome VARCHAR(100) NOT NULL,
    Email VARCHAR(100) NOT NULL,
    Telefone VARCHAR(15),
    Endereco TEXT
);

CREATE TABLE produtos (
    ProdutoID INT AUTO_INCREMENT PRIMARY KEY,
    Nome VARCHAR(100) NOT NULL,
    Preco DECIMAL(10,2) NOT NULL,
    Estoque INT NOT NULL
);

CREATE TABLE pedidos (
    PedidoID INT AUTO_INCREMENT PRIMARY KEY,
    ClienteID INT,
    DataPedido DATETIME NOT NULL,
    Status VARCHAR(50) NOT NULL,
    FOREIGN KEY (ClienteID) REFERENCES clientes(ClienteID)
);

CREATE TABLE itenspedido (
    ItemID INT AUTO_INCREMENT PRIMARY KEY,
    PedidoID INT,
    ProdutoID INT,
    Quantidade INT NOT NULL,
    PrecoUnitario DECIMAL(10,2),
    FOREIGN KEY (PedidoID) REFERENCES pedidos(PedidoID),
    FOREIGN KEY (ProdutoID) REFERENCES produtos(ProdutoID)
);
```

### 🔥 Trigger para Atualizar o Estoque

```sql
DELIMITER //
CREATE TRIGGER AtualizarEstoque
AFTER INSERT ON itenspedido
FOR EACH ROW
BEGIN
    UPDATE produtos SET Estoque = Estoque - NEW.Quantidade
    WHERE ProdutoID = NEW.ProdutoID;
END;
//
DELIMITER ;
```

### 🔥 Trigger para Preencher o Preço Unitário

```sql
DELIMITER //
CREATE TRIGGER AtualizarPrecoUnitario
BEFORE INSERT ON itenspedido
FOR EACH ROW
BEGIN
    DECLARE preco DECIMAL(10,2);
    SELECT Preco INTO preco FROM produtos WHERE ProdutoID = NEW.ProdutoID;
    SET NEW.PrecoUnitario = preco;
END;
//
DELIMITER ;
```

## 🛠 Como Usar

1️⃣ Clone este repositório:

```sh
git clone https://github.com/seu-usuario/gerenciamento-vendas.git
```

2️⃣ Importe o banco de dados no **MySQL Workbench** ou outro gerenciador.

3️⃣ Execute os scripts SQL fornecidos para criar as tabelas e triggers.

4️⃣ Insira dados fictícios para testar:

```sql
INSERT INTO clientes (Nome, Email, Telefone, Endereco) VALUES
('João Silva', 'joao@email.com', '11999999999', 'Rua A, 123'),
('Maria Souza', 'maria@email.com', '11888888888', 'Rua B, 456');
```

```sql
INSERT INTO produtos (Nome, Preco, Estoque) VALUES
('Café Expresso', 9.90, 100),
('Café Latte', 11.00, 50),
('Cappuccino', 12.50, 30),
('Mocha', 14.00, 20);
```

```sql
INSERT INTO pedidos (ClienteID, DataPedido, Status) VALUES
(1, '2025-02-21 10:00:00', 'Finalizado'),
(2, '2025-02-21 11:30:00', 'Em Andamento');
```

```sql
INSERT INTO itenspedido (PedidoID, ProdutoID, Quantidade) VALUES
(1, 1, 2),
(1, 3, 1),
(2, 2, 3),
(3, 4, 1);
```

5️⃣ Consulte os dados:

```sql
SELECT * FROM View_PedidosDetalhados;
```

## 🏆 Conclusão

Este projeto representa um marco importante no meu aprendizado de **MySQL** e **bancos de dados relacionais**. 🚀\
Agora consigo gerenciar pedidos, atualizar estoques automaticamente e definir preços sem complicação! 🎉

---

💡 **Criado por [Álvaro Silva]** | 📅 **Data: 21/02/2025**

