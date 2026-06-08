#### Exercício 03

Com base nos módulos expostos nesta Fase III, resolva o problema descrito a seguir.

- ##### __Descrição:__

Uma grande rede de supermercados nacional deseja implementar um novo sistema distribuído de entregas em tempo real. O sistema precisa atender a três perfis de usuários com interfaces específicas: 
    - os clientes (que fazem os pedidos);
    - os entregadores (que atualizam o status da entrega via GPS); e
    - os administradores (que monitoram o fluxo em um painel de controle).

O sistema deve ser totalmente acessível e __responsivo__, adaptando-se perfeitamente a computadores, celulares e tablets. Toda a comunicação entre o cliente (front-end) e o servidor (back-end) deve ser feita pela rede utilizando o protocolo HTTP sob o padrão arquitetural REST.

Com base nesse cenário, responda as duas questões a seguir:

1. Questão 01: Sugestão da Arquitetura (Valor: 5,0 pontos)

Desenhe ou descreva detalhadamente a arquitetura lógica desse sistema distribuído. Sua resposta deve abordar obrigatoriamente:

Como o padrão REST e os métodos HTTP (GET, POST, PUT, DELETE) serão utilizados para gerenciar a entidade "Pedido" (Criação, Consulta, Atualização de Status e Cancelamento).

Como a arquitetura garantirá a responsividade das interfaces e a separação de conceitos entre o back-end e os diferentes dispositivos (PCs, celulares e tablets).

Qual estratégia ou protocolo complementar ao HTTP REST tradicional poderia ser adotado para resolver o problema específico da atualização em tempo real da localização do entregador para o cliente, justificando a escolha.


2. Questão 02: Tecnologias Envolvidas – Nativos vs. Híbridos (Valor: 5,0 pontos)

Para a construção dos aplicativos móveis (clientes e entregadores), a equipe de engenharia está em dúvida entre adotar uma abordagem de Desenvolvimento Nativo ou Desenvolvimento Híbrido/Multiplataforma.

Explique a diferença conceitual entre uma aplicação móvel Nativa e uma Híbrida/Multiplataforma.

Cite pelo menos duas tecnologias/frameworks amplamente utilizados no mercado para cada uma das abordagens (duas para nativo e duas para híbrido).

Considerando que o aplicativo do entregador precisa acessar constantemente o hardware de GPS em segundo plano e renderizar mapas fluidos, qual das duas abordagens (Nativa ou Híbrida) é a mais recomendada para este app específico? Justifique sua resposta com base em desempenho e acesso aos recursos do sistema operacional.