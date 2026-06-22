```
from abc import ABC, abstractmethod
```

# ==========================================
###### 1. CLASSE CLIENTE
# ==========================================
```
class Cliente:
    def __init__(self, nome: str, telefone: str):
        """
        Representa o cliente que realiza o pedido.
        Associação simples com a classe Pedido.
        """
        self.nome = nome
        self.telefone = telefone
```

# ==========================================
###### 2. ESTRUTURA DE ITENS (HERANÇA E POLIMORFISMO)
# ==========================================
```
class ItemPedido(ABC):
    def __init__(self, nome: str, preco_base: float):
        """
        Classe abstrata que serve como superclasse para qualquer item
        que possa ser vendido na pizzaria (Pizzas, Bebidas, Sobremesas).
        """
        self.nome = nome
        self.preco_base = preco_base

    @abstractmethod
    def calcular_preco(self) -> float:
        """
        Método abstrato. Cada subclasse será obrigada a implementar
        sua própria regra de cálculo de preço (Polimorfismo).
        """
        pass
```
```
class Pizza(ItemPedido):
    def __init__(self, nome: str, tamanho: str, preco_base: float, ingredientes: list):
        """
        Classe Pizza herda de ItemPedido.
        Especializada com tamanho e múltiplos ingredientes.
        """
        # Invoca o construtor da superclasse (ItemPedido)
        super().__init__(nome, preco_base)
        
        # Garante que o tamanho fique sempre em maiúsculo (P, M, G)
        self.tamanho = tamanho.upper()
        self.ingredientes = ingredientes

    def calcular_preco(self) -> float:
        """
        Implementação do polimorfismo. 
        Poderia ter regras baseadas no tamanho, mas retorna o preço base conforme requisito.
        """
        return self.preco_base
```
```
class Bebida(ItemPedido):
    def __init__(self, nome: str, preco_base: float, tamanho_ml: int):
        """
        DESAFIO EXTRA 1: Classe Bebida herda de ItemPedido.
        Demonstra o reuso de código através da Herança.
        """
        super().__init__(nome, preco_base)
        self.tamanho_ml = tamanho_ml

    def calcular_preco(self) -> float:
        """
        Implementação do polimorfismo para a Bebida.
        """
        return self.preco_base
```

# ==========================================
###### 3. CLASSE PEDIDO (COMPOSIÇÃO / ENCAPSULAMENTO)
# ==========================================
```
class Pedido:
    def __init__(self, cliente: Cliente):
        """
        Gerencia o pedido da pizzaria.
        Possui uma associação com Cliente e uma composição/agregação com ItemPedido.
        """
        self.cliente = cliente
        self.itens = []  # Lista que receberá objetos do tipo Pizza e Bebida
        
        # DESAFIO EXTRA 2: Encapsulamento. 
        # O duplo underline '__' torna o atributo privado em Python.
        self.__status = "Pendente"  

    # Método Getter para permitir a leitura segura do status de fora da classe
    def obter_status(self) -> str:
        return self.__status

    # Método Setter para garantir a alteração segura e validada do status
    def alterar_status(self, novo_status: str):
        """
        Modifica o status do pedido após validar se a transição é permitida.
        """
        status_validos = ["Pendente", "Preparando", "Entregue"]
        if novo_status in status_validos:
            self.__status = novo_status
            print(f"Sucesso: O status do pedido agora é '{self.__status}'.")
        else:
            print(f"Erro: '{novo_status}' não é um status válido para o pedido.")

    def adicionar_item(self, item: ItemPedido):
        """
        Adiciona qualquer objeto que herde de ItemPedido (Pizza ou Bebida).
        """
        self.itens.append(item)
        print(f"Item '{item.nome}' adicionado com sucesso ao pedido!")

    def calcular_total(self) -> float:
        """
        MÉTODO PRINCIPAL DO REQUISITO:
        Calcula o valor total do pedido somando o preço de cada item.
        Aqui ocorre o Polimorfismo: o código chama 'calcular_preco()' sem se importar
        se o item é uma Pizza ou uma Bebida. O Python sabe qual método executar.
        """
        total = sum(item.calcular_preco() for item in self.itens)
        return total

    def exibir_resumo(self):
        """
        Gera um relatório visual do pedido no terminal.
        """
        print("\n" + "="*40)
        print(f"PIZZARIA UNIFOA - RESUMO DO PEDIDO")
        print(f"Cliente: {self.cliente.nome} | Tel: {self.cliente.telefone}")
        print(f"Status do Pedido: {self.obter_status()}")
        print("-"*40)
        
        for item in self.itens:
            # Verifica se o item é uma pizza para exibir os ingredientes
            if isinstance(item, Pizza):
                detalhes = f"({item.tamanho}) - Ingredientes: {', '.join(item.ingredientes)}"
            elif isinstance(item, Bebida):
                detalhes = f"({item.tamanho_ml}ml)"
            else:
                detalhes = ""
                
            print(f"• {item.nome} {detalhes} | R$ {item.calcular_preco():.2f}")
            
        print("-"*40)
        print(f"VALOR TOTAL DO PEDIDO: R$ {self.calcular_total():.2f}")
        print("="*40 + "\n")
```

# ==========================================
###### 4. FLUXO DE EXECUÇÃO (TESTE DO SISTEMA)
# ==========================================
```
if __name__ == "__main__":
    # Instanciando o cliente
    cliente_estudante = Cliente("Rodrigo UniFOA", "24-99999-1234")

    # Criando os itens (Pizzas e Bebidas)
    pizza1 = Pizza(nome="Calabresa", tamanho="G", preco_base=50.0, ingredientes=["Molho", "Mussarela", "Calabresa", "Cebola"])
    pizza2 = Pizza(nome="Quatro Queijos", tamanho="M", preco_base=45.0, ingredientes=["Mussarela", "Provolone", "Gorgozola", "Catupiry"])
    refrigerante = Bebida(nome="Coca-Cola Lata", preco_base=6.50, tamanho_ml=350)

    # Criando o pedido vinculado ao cliente
    pedido_pizzaria = Pedido(cliente=cliente_estudante)

    # Adicionando os itens ao pedido
    pedido_pizzaria.adicionar_item(pizza1)
    pedido_pizzaria.adicionar_item(pizza2)
    pedido_pizzaria.adicionar_item(refrigerante) # Testando o desafio 1 (Herança de bebida)

    # Atualizando o status usando o método seguro (Testando o desafio 2)
    pedido_pizzaria.alterar_status("Preparando")
    
    # Tentativa de burlar o sistema com um status inválido (Deve disparar o erro do validador)
    pedido_pizzaria.alterar_status("Pronto para comer")
```

    # Exibindo o resultado final com a soma total calculada
    pedido_pizzaria.exibir_resumo()
