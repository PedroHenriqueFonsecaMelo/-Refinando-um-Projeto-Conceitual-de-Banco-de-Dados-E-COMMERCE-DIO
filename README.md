# -Refinando-um-Projeto-Conceitual-de-Banco-de-Dados-E-COMMERCE-DIO

![image](https://github.com/user-attachments/assets/262e9f19-7cab-48ad-8805-9262c90b98b6)

-- Banco de Dados para E-commerce

CREATE DATABASE Ecommerce;
USE Ecommerce;

-- Tabela Cliente (PJ ou PF, mas nunca ambos)
CREATE TABLE Cliente (
    idCliente INT PRIMARY KEY AUTO_INCREMENT,
    Identificacao VARCHAR(45) NOT NULL,
    Tipo ENUM('PJ', 'PF') NOT NULL
);

-- Tabela Pessoa Jurídica
CREATE TABLE PJ_Pessoa_Juridica (
    idPJ INT PRIMARY KEY AUTO_INCREMENT,
    idCliente INT UNIQUE NOT NULL,
    Nome VARCHAR(45) NOT NULL,
    CNPJ VARCHAR(45) UNIQUE NOT NULL,
    Endereco VARCHAR(100) NOT NULL,
    FOREIGN KEY (idCliente) REFERENCES Cliente(idCliente) ON DELETE CASCADE
);

-- Tabela Pessoa Física
CREATE TABLE PF_Pessoa_Fisica (
    idPF INT PRIMARY KEY AUTO_INCREMENT,
    idCliente INT UNIQUE NOT NULL,
    Nome VARCHAR(45) NOT NULL,
    CPF VARCHAR(45) UNIQUE NOT NULL,
    Endereco VARCHAR(100) NOT NULL,
    FOREIGN KEY (idCliente) REFERENCES Cliente(idCliente) ON DELETE CASCADE
);

-- Tabela Pedido
CREATE TABLE Pedido (
    idPedido INT PRIMARY KEY AUTO_INCREMENT,
    Status_Pedido VARCHAR(45) NOT NULL,
    Descricao VARCHAR(255),
    Frete FLOAT DEFAULT 0.0,
    idCliente INT NOT NULL,
    FOREIGN KEY (idCliente) REFERENCES Cliente(idCliente) ON DELETE CASCADE
);

-- Tabela Pagamento
CREATE TABLE Pagamento (
    idPagamento INT PRIMARY KEY AUTO_INCREMENT,
    Meio_Pagamento ENUM('Cartão', 'Boleto') NOT NULL,
    Valor FLOAT NOT NULL,
    idPedido INT NOT NULL,
    FOREIGN KEY (idPedido) REFERENCES Pedido(idPedido) ON DELETE CASCADE
);

-- Tabela Cartão (Pagamento por Cartão)
CREATE TABLE Cartao (
    idCartao INT PRIMARY KEY AUTO_INCREMENT,
    idPagamento INT NOT NULL,
    Tipo_Cartao VARCHAR(45) NOT NULL,
    Numero_Cartao VARCHAR(45) UNIQUE NOT NULL,
    Nome_Cartao VARCHAR(45) NOT NULL,
    Data_Pagamento DATE NOT NULL,
    FOREIGN KEY (idPagamento) REFERENCES Pagamento(idPagamento) ON DELETE CASCADE
);

-- Tabela Boleto (Pagamento por Boleto)
CREATE TABLE Boleto (
    idBoleto INT PRIMARY KEY AUTO_INCREMENT,
    idPagamento INT NOT NULL,
    Valor FLOAT NOT NULL,
    Parcelamento INT DEFAULT 1,
    Data_Vencimento DATE NOT NULL,
    FOREIGN KEY (idPagamento) REFERENCES Pagamento(idPagamento) ON DELETE CASCADE
);

-- Tabela Entrega
CREATE TABLE Entrega (
    idEntrega INT PRIMARY KEY AUTO_INCREMENT,
    Data_Gerado DATE NOT NULL,
    Data_Entregue DATE,
    Status_Entrega VARCHAR(45) NOT NULL,
    Codigo_Rastreio VARCHAR(45) UNIQUE NOT NULL,
    idPedido INT NOT NULL,
    FOREIGN KEY (idPedido) REFERENCES Pedido(idPedido) ON DELETE CASCADE
);

-- yuml


[Cliente|+idCliente; +Identificacao; +Tipo (PJ/PF)]<>-1>[PJ_Pessoa_Juridica|+idPJ; +Nome; +CNPJ; +Endereco]
[Cliente]<>-1>[PF_Pessoa_Fisica|+idPF; +Nome; +CPF; +Endereco]

[Cliente]1-0..*>[Pedido|+idPedido; +Status; +Descricao; +Frete]

[Pedido]1-0..*>[Pagamento|+idPagamento; +Meio_Pagamento; +Valor]

[Pagamento]1-0..*>[Cartao|+idCartao; +Tipo; +Numero; +Nome_Cartao; +Data_Pagamento]
[Pagamento]1-0..*>[Boleto|+idBoleto; +Valor; +Parcelamento; +Data_Vencimento]

[Pedido]1-1>[Entrega|+idEntrega; +Data_Gerado; +Data_Entregue; +Status_Entrega; +Codigo_Rastreio]
