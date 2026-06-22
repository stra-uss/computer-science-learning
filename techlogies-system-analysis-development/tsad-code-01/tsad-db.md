#### SQL DDL

##### Criação da Tabela de Pedidos
```
CREATE TABLE IF NOT EXISTS pedidos (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    cliente_nome VARCHAR(100) NOT NULL,
    endereco_entrega TEXT NOT NULL,
    valor_total DECIMAL(10, 2) NOT NULL,
    status VARCHAR(20) DEFAULT 'Pendente', -- Valores: Pendente, Em Rota, Entregue, Cancelado
    criado_em TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

##### Criação da Tabela de Localização (Histórico de GPS em tempo real)

```
CREATE TABLE IF NOT EXISTS localizacoes_entregador (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    pedido_id INTEGER,
    latitude REAL NOT NULL,
    longitude REAL NOT NULL,
    atualizado_em TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (pedido_id) REFERENCES pedidos(id) ON DELETE CASCADE
);
```
