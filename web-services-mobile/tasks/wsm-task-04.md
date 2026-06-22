#### Exercício 04

🔍 __Âmbito:__
Atividade Conceitual: A Arquitetura e a Experiência do CRUD no Universo Mobile

🎯 __Objetivo:__
O objetivo desta atividade é fazer você refletir criticamente sobre como as quatro operações fundamentais do CRUD (Create, Read, Update, Delete) e os conceitos de responsividade se traduzem do ambiente web tradicional para o desenvolvimento de aplicativos móveis (nativos ou híbridos), considerando as limitações e as peculiaridades dos dispositivos móveis.

📝 - __Descrição:__
Imagine que a aplicação CRUD que você desenvolveu para a web (hospedada na Vercel) precisa ser transformada em um Aplicativo Móvel de grande escala. Em sistemas mobile, a persistência de dados, a interface com o usuário (UI) e a experiência do usuário (UX) seguem premissas diferentes de um navegador desktop.

Com base nos seus conhecimentos teóricos e na experiência prática de desenvolvimento do CRUD web, responda de forma dissertativa aos dois eixos temáticos abaixo:

__Eixo 1: O Ciclo de Vida do CRUD no Ambiente Mobile__
No desenvolvimento web, o "Read" e o "Update" geralmente dependem de uma conexão constante com a internet para atualizar a tela. No desenvolvimento móvel, o cenário é diferente (redes instáveis, modo avião, etc.).

Questão: Como a operação de Read (Leitura) e Update (Atualização) deve ser teoricamente tratada em um aplicativo móvel para garantir que o usuário não perca dados ou veja uma tela em branco quando estiver sem internet (offline)? Discorra sobre o conceito de armazenamento local (Local Storage/SQLite/Room) atuando como um intermediário antes do envio dos dados para o servidor remoto.

__Eixo 2: UI/UX – Da Responsividade Web aos Componentes Nativos__
Você utilizou Flexbox/Grid para garantir a responsividade em diferentes telas na web. No entanto, em aplicativos móveis, a experiência vai além de "esticar ou encolher" componentes; envolve a ergonomia do toque e os padrões de design de cada sistema operacional (Material Design para Android e Human Interface Guidelines para iOS).

- __Questão__:
  - Explique como a operação de Delete (Deletar) e Create (Cadastrar) se comporta visualmente em um app mobile. Pensando na usabilidade (UX), por que um botão de "Deletar" em um smartphone frequentemente utiliza gestos (como o swipe/deslizar para o lado) ou caixas de confirmação (modals), em vez de um simples botão fixo como na web? Qual é o impacto do tamanho dos dedos do usuário (alvos de toque) no design desse CRUD móvel?
