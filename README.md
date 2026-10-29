# 📦 MiniStock

Aplicativo Android para **controle de estoque de pequenos vendedores e microempreendedores**.

O MiniStock foi desenvolvido com o objetivo de facilitar o gerenciamento de produtos, permitindo cadastrar itens, registrar entradas e saídas, acompanhar o estoque disponível e receber alertas quando determinado produto estiver próximo de acabar.

## 📱 Sobre o projeto

Muitos pequenos vendedores ainda controlam seus estoques utilizando cadernos, planilhas ou mensagens.

Esse tipo de controle pode dificultar o acompanhamento da quantidade disponível, causar esquecimentos na reposição de produtos e tornar difícil consultar movimentações anteriores.

O **MiniStock** busca resolver esse problema centralizando o controle de estoque em um aplicativo Android simples e acessível.

### Público-alvo

O aplicativo é voltado principalmente para:

- Microempreendedores individuais (MEIs)
- Vendedores informais
- Pequenos comércios
- Pessoas que vendem produtos através de redes sociais ou aplicativos de mensagens

---

## 🎯 Objetivos

O principal objetivo do MiniStock é permitir que pequenos vendedores tenham uma forma simples de acompanhar seus produtos e suas movimentações de estoque diretamente pelo celular.

O aplicativo permite:

- Cadastrar produtos
- Editar e excluir produtos
- Consultar produtos cadastrados
- Registrar entradas de estoque
- Registrar saídas de estoque
- Consultar o histórico de movimentações
- Definir estoque mínimo por produto
- Identificar produtos com estoque baixo
- Receber notificações de estoque baixo
- Manter os dados armazenados localmente
- Consultar informações através de uma API HTTP

---

## ⚙️ Funcionalidades

### 📦 Gerenciamento de produtos

O usuário pode cadastrar produtos informando dados como:

- Nome
- Descrição
- Categoria
- Preço de custo
- Preço de venda
- Quantidade atual
- Estoque mínimo

Os produtos cadastrados podem posteriormente ser visualizados, editados ou excluídos.

### 📥 Entrada de estoque

Permite registrar a entrada de novas unidades de determinado produto.

Ao registrar uma entrada, a quantidade informada é adicionada automaticamente ao estoque atual.

### 📤 Saída de estoque

Permite registrar vendas, perdas ou outras saídas de produtos.

A quantidade informada é descontada do estoque atual.

O sistema não permite que uma saída deixe o estoque com quantidade negativa.

### 📜 Histórico de movimentações

Cada entrada ou saída realizada é registrada no histórico contendo informações como:

- Produto
- Tipo da movimentação
- Quantidade
- Data e hora
- Observação, quando informada

### ⚠️ Estoque baixo

Cada produto possui um valor de **estoque mínimo**.

Quando:

```text
quantidade atual <= estoque mínimo
```

o produto é considerado com estoque baixo e pode ser destacado na interface.

### 🔔 Notificações

O aplicativo utiliza notificações locais para avisar o usuário quando existem produtos com estoque baixo.

### 🌐 Consulta via API

O MiniStock também realiza comunicação com uma API HTTP externa utilizando Retrofit.

Durante uma requisição, o aplicativo apresenta um indicador de carregamento e trata possíveis erros de conexão.

---

## 🛠️ Tecnologias

### Java

Linguagem utilizada para implementar a lógica do aplicativo Android.

### XML

Utilizado para construção das interfaces e layouts das telas.

### Room

Biblioteca responsável pela persistência local dos dados.

O Room fornece uma abstração sobre o SQLite e será utilizado para armazenar:

- Produtos
- Movimentações de estoque

Dessa forma, os dados continuam disponíveis mesmo depois que o aplicativo é fechado.

### Retrofit

Biblioteca utilizada para realizar requisições HTTP e consumir uma API REST externa.

### WorkManager

Responsável pela execução de tarefas periódicas em segundo plano, como a verificação de produtos com estoque baixo.

### Notification API

Utilizada para gerar notificações locais relacionadas aos alertas de estoque.

### Material Components

Utilizado na construção dos componentes visuais seguindo os padrões de interface do Android.

---

## 🏗️ Arquitetura

O projeto utiliza uma arquitetura simples em camadas:

```text
Activity / Fragment
        ↓
    ViewModel
        ↓
    Repository
     ↙     ↘
   Room   Retrofit
    ↓        ↓
 SQLite   API HTTP
```

### Activity / Fragment

Responsável pela interface e interação do usuário.

### ViewModel

Mantém e gerencia os dados utilizados pela interface.

### Repository

Centraliza o acesso aos dados e decide se determinada informação será obtida localmente ou através da rede.

### Room

Responsável pelos dados armazenados localmente.

### Retrofit

Responsável pela comunicação com serviços HTTP externos.

---

## 🗃️ Modelo de dados

### Produto

```text
Produto
├── id
├── nome
├── descricao
├── categoria
├── precoCusto
├── precoVenda
├── quantidadeAtual
├── estoqueMinimo
└── dataCriacaoAtualizacao
```

### Movimentação

```text
Movimentacao
├── id
├── produtoId
├── tipo
├── quantidade
├── observacao
└── dataHora
```

Os tipos de movimentação são:

```text
ENTRADA
SAIDA
```

---

## 📋 Regras de negócio

O MiniStock possui algumas regras básicas para manter a consistência do estoque:

1. A quantidade de uma movimentação deve ser maior que zero.
2. Uma saída não pode deixar o estoque negativo.
3. Uma entrada aumenta a quantidade atual do produto.
4. Uma saída reduz a quantidade atual do produto.
5. Um produto é considerado com estoque baixo quando sua quantidade atual for menor ou igual ao estoque mínimo.
6. Toda movimentação concluída deve ser registrada no histórico.

---

## 🔄 Fluxos principais

### Cadastro de produto

```text
Produtos
   ↓
Novo produto
   ↓
Preencher dados
   ↓
Validar formulário
   ↓
Salvar no Room
   ↓
Atualizar listagem
```

### Movimentação de estoque

```text
Selecionar produto
       ↓
Entrada ou saída
       ↓
Informar quantidade
       ↓
Validar movimentação
       ↓
Registrar movimentação
       ↓
Atualizar estoque
       ↓
Verificar estoque mínimo
```

### Verificação de estoque baixo

```text
Quantidade atual
       ↓
Comparar com estoque mínimo
       ↓
Quantidade <= mínimo?
       ↓
Produto com estoque baixo
       ↓
Gerar alerta/notificação
```

---

## 📂 Estrutura planejada

Uma possível organização do projeto é:&#x20;

```text
app/
└── src/main/
    ├── java/
    │   └── ministock/
    │       ├── data/
    │       │   ├── local/
    │       │   ├── remote/
    │       │   └── repository/
    │       │
    │       ├── model/
    │       ├── ui/
    │       ├── viewmodel/
    │       └── worker/
    │
    ├── res/
    │   ├── layout/
    │   ├── drawable/
    │   ├── mipmap/
    │   └── values/
    │
    └── AndroidManifest.xml
```

A estrutura poderá sofrer alterações conforme o desenvolvimento do aplicativo evoluir.

##

---

## 🎓 Contexto acadêmico

O MiniStock é um projeto desenvolvido para a disciplina de **Desenvolvimento de Aplicativo Mobile**, utilizando **codificação nativa Android**.

O projeto busca demonstrar conceitos como:

- Arquitetura mobile organizada
- Eventos e listeners
- Persistência local
- Comunicação HTTP
- Consumo de API
- Carregamento assíncrono
- Tratamento de erros
- Notificações
- Processamento em segundo plano

---

## 👨‍💻 Desenvolvimento

Projeto acadêmico desenvolvido utilizando **Java e Android nativo**.

---

## 📄 Licença

Este projeto possui finalidade acadêmica e educacional.
