### Exercício 03

Dando prosseguimento aos dois exercícios anteriores, esse terceiro exercício consiste em analisar, conforme a seguir, o uso de classes e métodos abstratos à nossa hipotética pizzaria.

Deste modo, a resolução do exercício consiste em descrever, por meio de três parágrafos, o que você entendeu acerca do uso de classes e métodos abstratos, citando vantagens e desvantagens.

---

Para atender ao requisito de usar classes e métodos abstratos, vamos pensar no seguinte: 

Uma pizza em si é um conceito geral. Podemos ter uma classe abstrata ItemPedido ou a própria Pizza como uma classe base abstrata se quisermos derivar tipos específicos de pizzas no futuro (como pizzas tradicionais, doces ou personalizadas). Vamos seguir a linha de tornar a classe Pizza abstrata, forçando que qualquer tipo de pizza implemente seu próprio cálculo de preço com base no tamanho e ingredientes.


1. Diagrama de Classes UML
Abaixo está a representação visual das classes. Note que os nomes das classes abstratas e dos métodos abstratos estão em itálico (padrão UML).

Representação Textual do Diagrama
ItemMenu (Classe Abstrata): Tem o método abstrato calcular_preco().

Pizza (Classe Abstrata / Herda de ItemMenu): Possui os atributos nome, tamanho e preco_base, além de uma agregação com Ingrediente.

PizzaTradicional (Classe Concreta / Herda de Pizza): Implementa o método calcular_preco().

Ingrediente (Classe Concreta): Possui nome.

Pedido (Classe Concreta): Possui cliente, uma lista de Pizzas (Associação/Composição) e status. Tem o método calcular_total().

2. Código Fonte em Python
Em Python, criamos classes abstratas utilizando o módulo nativo abc (Abstract Base Classes)

```python
from abc import ABC, abstractmethod
from enum import Enum
from typing import List

class StatusPedido(Enum):
    PENDENTE = "Pendente"
    PREPARANDO = "Preparando"
    ENTREGUE = "Entregue"

class TamanhoPizza(Enum):
    P = 1.0   # Multiplicador de preço para tamanho P
    M = 1.3   # Multiplicador de preço para tamanho M
    G = 1.6   # Multiplicador de preço para tamanho G

class Ingrediente:
    def __init__(self, nome: str):
        self.nome = nome

```

```python
# --- CLASSES ABSTRATAS ---

class Pizza(ABC):
    """Classe Abstrata que serve de molde para qualquer tipo de pizza."""
    
    def __init__(self, nome: str, tamanho: TamanhoPizza, preco_base: float):
        self.nome = nome
        self.tamanho = tamanho
        self.preco_base = preco_base
        self.ingredientes: List[Ingrediente] = []

    def adicionar_ingrediente(self, ingrediente: Ingrediente):
        self.ingredientes.append(ingrediente)

    @abstractmethod
    def calcular_preco(self) -> float:
        """Método abstrato que deve ser implementado pelas classes filhas."""
        pass

```

```python
# --- CLASSES CONCRETAS ---

class PizzaTradicional(Pizza):
    """Classe concreta que implementa a regra de negócio do preço da pizza."""
    
    def calcular_preco(self) -> float:
        # Regra de negócio fictícia: Preço base multiplicado pelo fator do tamanho
        # mais um adicional por quantidade de ingredientes (R$ 2,00 por ingrediente).
        fator_tamanho = self.tamanho.value
        adicional_ingredientes = len(self.ingredientes) * 2.00
        
        preco_final = (self.preco_base * fator_tamanho) + adicional_ingredientes
        return round(preco_final, 2)

class Pedido:
    """Classe responsável por gerenciar o pedido do cliente."""
    
    def __init__(self, cliente: str):
        self.cliente = cliente
        self.pizzas: List[Pizza] = []
        self.status: StatusPedido = StatusPedido.PENDENTE

    def adicionar_pizza(self, pizza: Pizza):
        self.pizzas.append(pizza)

    def atualizar_status(self, novo_status: StatusPedido):
        self.status = novo_status

    def calcular_total(self) -> float:
        """Calcula o valor total somando o preço de cada pizza no pedido."""
        total = sum(pizza.calcular_preco() for pizza in self.pizzas)
        return round(total, 2)

```

```python
# --- Execução ---
if __name__ == "__main__":
    print("--- Inicializando o Sistema da Pizzaria --- \n")

    # 1. Criando ingredientes
    queijo = Ingrediente("Muçarela")
    tomate = Ingrediente("Tomate")
    manjericao = Ingrediente("Manjericão")
    calabresa = Ingrediente("Calabresa")

    # 2. Criando as Pizzas (instanciando a classe concreta)
    pizza1 = PizzaTradicional("Margherita", TamanhoPizza.M, 30.0)
    pizza1.adicionar_ingrediente(queijo)
    pizza1.adicionar_ingrediente(tomate)
    pizza1.adicionar_ingrediente(manjericao)

    pizza2 = PizzaTradicional("Calabresa", TamanhoPizza.G, 35.0)
    pizza2.adicionar_ingrediente(queijo)
    pizza2.adicionar_ingrediente(calabresa)

    # 3. Criando o Pedido do Cliente
    pedido_joao = Pedido("João Silva")
    pedido_joao.adicionar_pizza(pizza1)
    pedido_joao.adicionar_pizza(pizza2)

    # 4. Exibindo os resultados e alterando status
    print(f"Pedido do cliente: {pedido_joao.cliente}")
    print(f"Status Inicial: {pedido_joao.status.value}")
    
    print(f"Preço da Pizza {pizza1.nome} ({pizza1.tamanho.name}): R$ {pizza1.calcular_preco()}")
    print(f"Preço da Pizza {pizza2.nome} ({pizza2.tamanho.name}): R$ {pizza2.calcular_preco()}")
    
    print("-" * 30)
    print(f"VALOR TOTAL DO PEDIDO: R$ {pedido_joao.calcular_total()}")
    print("-" * 30)

    # Atualizando o status do processo
    pedido_joao.atualizar_status(StatusPedido.PREPARANDO)
    print(f"Novo Status: {pedido_joao.status.value}")
```

__Pontos a destacar__

- Abstração (ABC e @abstractmethod): A classe Pizza não pode ser instanciada diretamente (p = Pizza(...) daria erro). Isso garante que ninguém crie uma pizza "genérica" sem regras de preço.

- Polimorfismo: Se amanhã a pizzaria criar uma PizzaDoce com outra regra de cálculo, o método pedido.calcular_total() continuará funcionando sem mudar uma única linha de código, pois ele só se importa que o objeto seja uma extensão de Pizza e tenha o método calcular_preco().
