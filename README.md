# comercioeletronicoac2




CREATE TABLE Produtos (
    ID INT AUTO_INCREMENT PRIMARY KEY,
    Nome VARCHAR(255),
    Descricao TEXT,
    Preco DECIMAL(10, 2),
    Quantidade_Estoque INT
);




CREATE TABLE Clientes (
    ID INT AUTO_INCREMENT PRIMARY KEY,
    Nome VARCHAR(255),
    Endereco_de_Entrega TEXT,
    Email VARCHAR(255),
    Celular INT 
);


CREATE TABLE Pedidos (
    ID INT AUTO_INCREMENT PRIMARY KEY,
    DataDoPedido DATE,
    ClienteID INT,
    Status_Do_Pedido VARCHAR(50),
    FOREIGN KEY (ClienteID) REFERENCES Clientes(ID)
);


CREATE TABLE ItensDePedido (
    ID INT AUTO_INCREMENT PRIMARY KEY,
    PedidoID INT,
    ProdutoID INT,
    Quantidade INT,
    FOREIGN KEY (PedidoID) REFERENCES Pedidos(ID),
    FOREIGN KEY (ProdutoID) REFERENCES Produtos(ID)
);



SELECT * FROM Clientes;
SELECT * FROM produtos;
SELECT * FROM pedidos;
DELETE FROM pedidos;
SELECT * FROM itensdepedido;
DELETE FROM itensdepedido;
-- MySQL Workbench Forward Engineering


CREATE TABLE Produtos (
    ID INT AUTO_INCREMENT PRIMARY KEY,
    Nome VARCHAR(255),
    Descricao TEXT,
    Preco DECIMAL(10, 2),
    Quantidade_Estoque INT
);




CREATE TABLE Clientes (
    ID INT AUTO_INCREMENT PRIMARY KEY,
    Nome VARCHAR(255),
    Endereco_de_Entrega TEXT,
    Email VARCHAR(255),
    Celular INT 
);


CREATE TABLE Pedidos (
    ID INT AUTO_INCREMENT PRIMARY KEY,
    DataDoPedido DATE,
    ClienteID INT,
    Status_Do_Pedido VARCHAR(50),
    FOREIGN KEY (ClienteID) REFERENCES Clientes(ID)
);


CREATE TABLE ItensDePedido (
    ID INT AUTO_INCREMENT PRIMARY KEY,
    PedidoID INT,
    ProdutoID INT,
    Quantidade INT,
    FOREIGN KEY (PedidoID) REFERENCES Pedidos(ID),
    FOREIGN KEY (ProdutoID) REFERENCES Produtos(ID)
);



SELECT * FROM Clientes;
SELECT * FROM produtos;
SELECT * FROM pedidos;
DELETE FROM pedidos;
SELECT * FROM itensdepedido;
DELETE FROM itensdepedido;
![image](https://github.com/huankzera/comercioeletronicoac2/assets/126423433/c652a035-ef94-4b2a-ae69-eee6dfbe1303)

Pedidos: Registre detalhes de pedidos, incluindo data, cliente e status.

![image](https://github.com/huankzera/comercioeletronicoac2/assets/126423433/a1fc40ef-c052-40dc-997a-205d08c9bebb)

Clientes: Mantenha informações dos clientes, como nome, endereço de entrega e informações de contato

Itens de Pedido: Registre os produtos incluídos em cada pedido, junto com a quantidade.
![image](https://github.com/huankzera/comercioeletronicoac2/assets/126423433/4f7bc993-6de7-4f21-9548-a7a9c0af3bf5)


Relacionamentos:
Crie um relacionamento entre "Pedidos" e "Clientes" para rastrear os pedidos de cada cliente.

  
CREATE TABLE Pedidos (
    ID INT AUTO_INCREMENT PRIMARY KEY,
    DataDoPedido DATE,
    ClienteID INT,
    Status_Do_Pedido VARCHAR(50),
    FOREIGN KEY (ClienteID) REFERENCES Clientes(ID)
);
Estabeleça um relacionamento entre "Itens de Pedido" e "Produtos" para associar produtos a pedidos.

-- Tabela ItensDePedido
CREATE TABLE ItensDePedido (
    ID INT AUTO_INCREMENT PRIMARY KEY,
    PedidoID INT,
    ProdutoID INT,
    Quantidade INT,
    FOREIGN KEY (PedidoID) REFERENCES Pedidos(ID),
    FOREIGN KEY (ProdutoID) REFERENCES Produtos(ID)
);



Stored Procedures:
Implemente uma stored procedure para permitir que os clientes adicionem produtos ao carrinho de compras.

DELIMITER $

CREATE PROCEDURE AdicionarProdutoAoCarrinho(
    IN cliente_id INT,
    IN produto_id INT,
    IN quantidade INT
)
BEGIN
    DECLARE pedido_id INT;
    
    -- Verificar se o cliente já tem um carrinho ativo
    SELECT ID INTO pedido_id
    FROM Pedidos
    WHERE ClienteID = cliente_id AND Status_Do_Pedido = 'Carrinho';
    
    -- Se não houver um carrinho ativo, criar um novo
    IF pedido_id IS NULL THEN
        INSERT INTO Pedidos (ClienteID, DataDoPedido, Status_Do_Pedido)
        VALUES (cliente_id, NOW(), 'Carrinho');
        SET pedido_id = LAST_INSERT_ID();
    END IF;
    
    -- Adicionar produto ao carrinho (tabela ItensDePedido)
    INSERT INTO ItensDePedido (PedidoID, ProdutoID, Quantidade)
    VALUES (pedido_id, produto_id, quantidade);
    
END$

DELIMITER ;


Stored Procedures:
Implemente uma stored procedure para permitir que os clientes adicionem produtos ao carrinho de compras.

DELIMITER $

CREATE PROCEDURE AdicionarProdutoAoCarrinho(
    IN cliente_id INT,
    IN produto_id INT,
    IN quantidade INT
)
BEGIN
    DECLARE pedido_id INT;
    
    -- Verificar se o cliente já tem um carrinho ativo
    SELECT ID INTO pedido_id
    FROM Pedidos
    WHERE ClienteID = cliente_id AND Status_Do_Pedido = 'Carrinho';
    
    -- Se não houver um carrinho ativo, criar um novo
    IF pedido_id IS NULL THEN
        INSERT INTO Pedidos (ClienteID, DataDoPedido, Status_Do_Pedido)
        VALUES (cliente_id, NOW(), 'Carrinho');
        SET pedido_id = LAST_INSERT_ID();
    END IF;
    
    -- Adicionar produto ao carrinho (tabela ItensDePedido)
    INSERT INTO ItensDePedido (PedidoID, ProdutoID, Quantidade)
    VALUES (pedido_id, produto_id, quantidade);
    
END$

DELIMITER ;

Desenvolva uma stored procedure para calcular o total de um pedido com base nos produtos incluídos.

DELIMITER $

CREATE PROCEDURE CalcularTotalPedido(
    IN pedido_id INT,
    OUT total DECIMAL(10, 2)
)
BEGIN
    -- Calcular o total do pedido com base nos produtos incluídos
    SELECT SUM(p.Preco * i.Quantidade) INTO total
    FROM Produtos AS p
    JOIN ItensDePedido AS i ON p.ID = i.ProdutoID
    WHERE i.PedidoID = pedido_id;
    
END$

DELIMITER ;

Desenvolva uma stored procedure para calcular o total de um pedido com base nos produtos incluídos.

DELIMITER $

CREATE PROCEDURE CalcularTotalPedido(
    IN pedido_id INT,
    OUT total DECIMAL(10, 2)
)
BEGIN
    -- Calcular o total do pedido com base nos produtos incluídos
    SELECT SUM(p.Preco * i.Quantidade) INTO total
    FROM Produtos AS p
    JOIN ItensDePedido AS i ON p.ID = i.ProdutoID
    WHERE i.PedidoID = pedido_id;
    
END$

DELIMITER ;

![image](https://github.com/huankzera/comercioeletronicoac2/assets/126423433/e884a497-d926-4818-a698-e070c9d65fad)

Implemente uma view que forneça uma lista de todos os produtos disponíveis.

CREATE VIEW ProdutosDisponiveis AS
SELECT ID, Nome, Descricao, Preco, Quantidade_Estoque
FROM Produtos
WHERE Quantidade_Estoque > 0;


