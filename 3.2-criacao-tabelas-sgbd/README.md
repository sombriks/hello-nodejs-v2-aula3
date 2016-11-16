# Criando tabelas no SGBD

- 20 minutos

## Definição

- [SQL](https://pt.wikipedia.org/wiki/SQL)
- linguagem voltada para modelagem e consulta de dados
- dados organizados em tabelas

## Aplicação

- dê fork no projeto **hello-nodejs-v2-aula3** e adicione o código abaixo:

```sql
-- contatoagenda.sql
create table contatoagenda(
  idcontato integer not null primary key,
  nomecontato varchar(255) not null,
  telefonecontato varchar(255) not null
);
```

- o que este código significa?
  - na minha agenda terei contatos e deles guardarei nome, telefone e um identificador único
- definições de tabelas contém listas de colunas (idcontato, nomecontato, telefonecontato)
- colunas tem [tipo](https://www.sqlite.org/datatype3.html) (integer, varchar, date)
- colunas tem modificadores (not null, primary key)
- tipos podem ter tamanho (varchar(255))
- mas como é que roda esse código?

### SGBD: Sistema Gerenciador de Banco de Dados

- o banco de dados quase sempre conta com uma ferramenta de administração
  - para [sqlite](http://sqlite.org/index.html) você pode usar o [dbbrowser](http://sqlitebrowser.org/)
- usando o postgresql você pode usar o pgadmin ou a ferramenta de linha de comando, o psql
- usando mysql tem o mysqladmin ou a ferramenta de linha de comando
- usando oracle você tem... você pegou o espírito

## Exemplo

- vamos criar a tabela usando sqlite
- procure no computador um programa chamado db browser

### Rodando o exemplo dado no db browser

- abra o db browser
- aperte o botão "novo banco de dados"
- salve o banco como **hello.sqlite** dentro da pasta do projeto **hello-nodejs-v2-aula3**
  - se você não deu checkout no projeto, *do it now*
- selecione a aba "executar SQL"
- copie e cole o código ali
- dê CTRL+enter ou aperte o botão de execução
- volte para a aba estrutura do banco de dados e veja sua primeira tabela

### Outros SGBD's

- demais ferramentas funcionam de modo parecido
- definir as tabelas, entretanto, é apenas a primeira parte do processo

## Exercício

- crie uma tabela chamada evento
- dos eventos desejaremos saber
  - descrição do evento
  - data de início do evento
- baseie-se no código fornecido
- crie uma coluna chamada idevento para ser a chave primária da tabela evento
- fim dos 20 minutos, vamos para a [parte 2](../3.1-revisao-sql/README.md)