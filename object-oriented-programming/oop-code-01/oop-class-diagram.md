'''
classDiagram
    direction topDown

    class Cliente {
        +String nome
        +String telefone
        +__init__(nome, telefone)
    }

    class ItemPedido {
        <<abstract>>
        +String nome
        +float preco_base
        +__init__(nome, preco_base)
        +calcular_preco()* float
    }

    class Pizza {
        +String tamanho
        +list ingredientes
        +__init__(nome, tamanho, preco_base, ingredientes)
        +calcular_preco() float
    }

    class Bebida {
        +int tamanho_ml
        +__init__(nome, preco_base, tamanho_ml)
        +calcular_preco() float
    }

    class Pedido {
        +Cliente cliente
        +list itens
        -String status
        +__init__(cliente)
        +obter_status() String
        +alterar_status(novo_status)
        +adicionar_item(item)
        +calcular_total() float
        +exibir_resumo()
    }

    Pedido "0..*" --> "1" Cliente : Vinculado a
    Pedido "1" *-- "0..*" ItemPedido : Contém
    Pizza --|> ItemPedido : Herança
    Bebida --|> ItemPedido : Herança
'''
