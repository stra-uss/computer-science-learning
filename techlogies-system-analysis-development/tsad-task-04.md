#### Exercício 04

📍​ __Âmbito:__
Atividade Prática e Conceitual: Arquitetura de sistemas e Desenvolvimento Web - Front-end e Back-end 

🎯 __Objetivo:__
Baseando-se nos exercício 03 e 04, ou seja, por meio de requisitos de desenvolvimento de um CRUD, esta tarefa tem como objetivo
implementar um sistema Web visando, de modo prático (hands-on), consolidar os conhecimentos teóricos da disciplina.

A arquitetura proposta é implementada nas tecnologias:
 - o front-end em HTML/CSS/JS,
 - o back-end em Python FastAPI; e
 - o banco de dados em SQLite,

📝 __Descrição:__

Você deverá analisar / desenvolver (ou aprimorar), a partir do código-fonte a seguir, um protótipo funcional de uma plataforma de entregas em tempo real para uma rede de supermercados. 

O foco será o Painel do Administrador, onde será possível realizar o CRUD completo da entidade Pedido (Criar, Ler, Atualizar status e Deletar).

- Requisitos Técnicos:
  - Front-end: Página web única (SPA), responsiva, utilizando apenas HTML5, CSS3 e JavaScript Moderno (Sugestão: Vanilla JS) 
  - Back-end: API RESTful construída com FastAPI (Python).
  - Banco de Dados: SQLite para persistência local dos dados.
  - Hospedagem (Simulação): O repositório deve ser estruturado para fácil deploy (sugestão: Vercel).
 
Por fim, você deverá responder:

Com base no código gerado e nas características da arquitetura cliente-servidor implementada, responda:

- 1) No código front-end desenvolvido (a seguir), a atualização da listagem de pedidos acontece de forma reativa 
(acionada manualmente pelas funções JavaScript após cada operação de mutação como POST, PUT ou DELETE). Assim, avaliando o cenário do Exercício 03 (onde o cliente precisa ver o deslocamento do entregador em tempo real no mapa),
explique por que o modelo tradicional de requisições HTTP (fetch/REST) utilizado neste código é considerado ineficiente para essa funcionalidade específica?

- 2) Qual seria o protocolo ideal para resolver este problema no ecossistema Python (FastAPI) / JavaScript Vanilla?
 
📝 __Código-fonte:__

- __Banco de dados__
  - [SQL DDL de criação de tabelas]()

- __Front-end__
  - [Front-end App]()
    
- __Back-end__
  - [Back-end App]()


