### Exercício 02

  Recordando o exercício anterior:
  
  Você, como estudante, foi contratado para modelar o sistema de pedidos de uma nova pizzaria. O objetivo é criar uma estrutura que suporte diferentes tipos de pizzas, acompanhamento de pedidos e cálculo de preços.
  O exercício, portanto, consiste na criação do(s) diagrama(s) UML e do código fonte, em linguagem Python, ambos referentes à funcionalidade de calcular o valor total de um pedido.
  
  - Requisitos funcionais do sistema
    - A Pizza deve ter um nome, tamanho (P, M, G) e preço base.
    - O sabor de cada pizza pode ter múltiplos ingredientes.
    - O pedido deve registrar o cliente, uma lista de pizzas pedidas e o status do pedido (Pendente, Preparando, Entregue).  

Neste contexto, o exercício 02 consiste em examinar o código-fonte, a seguir, em linguagem Python, descrevendo o que você compreendeu acerca dos conceitos:
 - Herança
 - Polimorfimso
 - Composição e Associação


#### Classe Cliente
```python
class Cliente:
    def __init__(self, nome: str, telefone: str):
        self.nome = nome
        self.telefone = telefone
```

#### Superclasse Pizza

```python
class Pizza:
    def __init__(self, nome: str, tamanho: str, preco_base: float, ingredientes: list):
        self.nome = nome
        self.tamanho = tamanho.upper()  # Garante P, M ou G
        self.preco_base = preco_base
        self.ingredientes = ingredientes

    def calcular_preco(self) -> float:
        """Método base para cálculo de preço."""
        return self.preco_base
```

#### Subclasses

```python
class PizzaTradicional(Pizza):
    """Pizza tradicional segue estritamente o preço base."""
    def __init__(self, nome: str, tamanho: str, preco_base: float, ingredientes: list):
        super().__init__(nome, tamanho, preco_base, ingredientes)

class PizzaEspecial(Pizza):
    """Pizza especial possui uma taxa adicional por conta de ingredientes nobres."""
    def __init__(self, nome: str, tamanho: str, preco_base: float, ingredientes: list, taxa_adicional: float = 10.0):
        super().__init__(nome, tamanho, preco_base, ingredientes)
        self.taxa_adicional = taxa_adicional

    def calcular_preco(self) -> float:
        """Polimorfismo: Sobrescreve o cálculo para incluir a taxa."""
        return self.preco_base + self.taxa_adicional
```

#### Classe Pedido
```python
class Pedido:
    def __init__(self, cliente: Cliente):
        self.cliente = cliente
        self.pizzas = []
        self.status = "Pendente"  # Status padrão: Pendente, Preparando, Entregue

    def adicionar_pizza(self, pizza: Pizza):
        self.pizzas.append(pizza)
        print(f"Pizza '{pizza.nome}' ({pizza.tamanho}) adicionada ao pedido!")

    def alterar_status(self, novo_status: str):
        status_validos = ["Pendente", "Preparando", "Entregue"]
        if novo_status in status_validos:
            self.status = novo_status
            print(f"Status do pedido atualizado para: {self.status}")
        else:
            print("Status inválido!")

    def calcular_total(self) -> float:
        """Calcula a soma dos preços de todas as pizzas do pedido."""
        total = sum(pizza.calcular_preco() for pizza in self.pizzas)
        return total

    def exibir_resumo(self):
        print("\n" + "="*30)
        print(f"RESUMO DO PEDIDO - CLIENTE: {self.cliente.nome}")
        print(f"Status atual: {self.status}")
        print("-"*30)
        for pizza in self.pizzas:
            print(f"- {pizza.nome} ({pizza.tamanho}) | Ingredientes: {', '.join(pizza.ingredientes)} | R$ {pizza.calcular_preco():.2f}")
        print("-"*30)
        print(f"VALOR TOTAL: R$ {self.calcular_total():.2f}")
        print("="*30)
```

#### Execução
```python
if __name__ == "__main__":
    # 1. Criando o cliente
    cliente1 = Cliente("Rodrigo Silva", "24-99999-9999")

    # 2. Criando as pizzas (Usando Herança)
    # Pizza Tradicional
    pizza_mussarela = PizzaTradicional(
        nome="Mussarela", 
        tamanho="G", 
        preco_base=45.0, 
        ingredientes=["Molho de tomate", "Mussarela", "Orégano"]
    )
    
    # Pizza Especial (com taxa adicional de R$ 15.00 por ingredientes nobres)
    pizza_camarao = PizzaEspecial(
        nome="Camarão Cremoso", 
        tamanho="M", 
        preco_base=60.0, 
        ingredientes=["Molho", "Mussarela", "Camarão", "Catupiry"],
        taxa_adicional=15.0
    )

    # 3. Criando o pedido e adicionando as pizzas
    pedido = Pedido(cliente=cliente1)
    pedido.adicionar_pizza(pizza_mussarela)
    pedido.adicionar_pizza(pizza_camarao)

    # 4. Atualizando o status e exibindo o valor total
    pedido.alterar_status("Preparando")
    pedido.exibir_resumo()
```
