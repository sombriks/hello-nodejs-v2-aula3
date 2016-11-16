# Revisão SQL

- 70 minutos
  - ver create table (10)
  - ver alter table (10)
  - ver drop table (10)
  - ver insert (10)
  - ver update (10)
  - ver delete (10)
  - ver select (10)

## Definição

- não dá tempo, vamos adiante
- lembre-se de salvar no seu git os exemplos de código!

## Aplicação

- SQL serve pra salvar os dados de tal modo que extrair **informação** seja tarefa praticável

## Exemplo

### Create table

```sql
-- evento.sql
create table evento(
  idevento integer not null primary key,
  dscevento varchar(255) not null,
  dataevento date not null
);
```

- uma tabela tem relação com o mundo real
- colunas tem relação com características de alguém, algum lugar, algum objeto, *conexão* etc
  - doravante chamados de *entidade*
- se **eu** quisesse representar um cachorro em uma tabela:

```sql
-- cachorro.sql
create table cachorro(
  idcachorro integer not null primary key,
  nomecachorro varchar(255) not null,
  racacachorro varchar(255),
);
```

- note que **você** pode representar um cachorro de um modo diferente
- evite acentuações ao definir colunas e tabelas. para o seu próprio bem

### Alter table

- nosso modelo de dados nem sempre sai perfeito de primeira
- o tempo passa e as coisas mudam
- seu [esquema de dados](https://pt.wikipedia.org/wiki/Esquema_de_banco_de_dados) deve refletir o mundo em seu estado atual
- se eu quiser adicionar a data de nascimento do cachorro:

```sql
-- cachorro-2.sql
alter table cachorro add column dtnascimentocachorro date;
```

- o alter table pode remover uma coluna e também mudar a definição de uma coluna
  - o sqlite [não faz](http://stackoverflow.com/questions/5938048/delete-column-from-sqlite-table/5987838#5987838) isso, mas o postgresql [faz](https://www.postgresql.org/docs/9.6/static/sql-altertable.html)

### Drop table

- remover do seu esquema de dados uma entidade ou relacionamento

```sql
-- cachorro-3.sql
drop table cachorro;
```

### Insert

- sabemos manipular as tabelas, agora vamos inserir dados nelas

```sql
-- contatoagenda-2.sql
insert into contatoagenda (nomecontato,telefonecontato) values ('Jane','123454321');
insert into contatoagenda (nomecontato,telefonecontato) values ('José','987654321');
insert into contatoagenda (nomecontato,telefonecontato) values ('Fran','543212345');
insert into contatoagenda (nomecontato,telefonecontato) values ('João','123456789');
insert into evento(dscevento,dataevento) values ('Jantar fim de semana', '2016-11-19');
insert into evento(dscevento,dataevento) values ('Pegar uma praia', '2016-11-20');
insert into evento(dscevento,dataevento) values ('Natal', '2016-12-25');
```

- não precisamos indicar um valor para a cioluna **idcontato**
- o valor da chave é gerenciado pelo SGBD 9 de 10 vezes
- datas podem ser representadas como texto. Meio doido, eu sei
- atenção para não inserir dados duplicados no banco!

### Update

- usamos quando queremos fazer ajustes em algum dado de alguma tabela

```sql
-- exemplo-update.sql
update contatoagenda set telefonecontato = '121232121' where idcontato = 1;
update evento set dscevento = 'subir a serra' where dataevento = '2016-11-19';
```

- **cuidado com o update sem where!!!**
- se não tem condição, o SQL entende que é pra aplicar em todo mundo da tabela

### Delete

- usamos quando queremos remover dados de uma tabela

```sql
-- exemplo-delete.sql
delete from contatoagenda where nomecontato = 'João';
```

- **cuidado com o delete sem where!!!**

## Select

- usamos quando desejamos consultar (sem ser pela interface gráfica do db browser) os dados de uma tabela
- é onde os **dados** podem virar **informação**
- é onde a magia negra reside
- vide exercícios de aprofundamento

```sql
-- exemplo-select.sql
select * from contatoagenda;
```

- devolve todo mundo

```sql
-- exemplo-select-2.sql
select * from contatoagenda where nomecontato = 'Fran';
```

- devolve só quem se chama Fran

```sql
-- exemplo-select-3.sql
select * from contatoagenda where idcontato = 3;
```

- devolve quem tem o id valendo 3

```sql
-- exemplo-select-4.sql
select * from contatoagenda, evento;
```

- consultas podem consultar mais de uma tabela ao mesmo tempo
- sem um critério, o SQL realiza um cruzamento cartesiano dos dados.

```sql
-- exemplo-select-5.sql
create table participantesevento(
  idcontato integer not null,
  idevento integer not null,
  foreign key (idcontato) references contatoagenda(idcontato),
  foreign key (idevento) references evento(idevento),
  primary key (idcontato,idevento)
);

insert into participantesevento (idcontato,idevento) values (1,1);
insert into participantesevento (idcontato,idevento) values (2,1);
insert into participantesevento (idcontato,idevento) values (1,3);
insert into participantesevento (idcontato,idevento) values (1,2);
insert into participantesevento (idcontato,idevento) values (2,3);
insert into participantesevento (idcontato,idevento) values (3,3);

select
  *
from
  contatoagenda, evento, participantesevento
where
    contatoagenda.idcontato      = participantesevento.idcontato
and evento.idevento              = participantesevento.idevento
and participantesevento.idevento = 3
```

- a tabela **participantesevento** faz uso do que chamamos de **chave estrangeira**
- os dados inseridos nela são chaves de outras tabelas. é bom que estas chaves existam
- na consulta exemplificada a where apresentada garante que as chaves condizem umas com as outras
  - assim evitamos o cartesiano e extraímos a informação desejada: saber quem vai participar do evento 3 (natal)

## Exercício

- a parte de consultas fica maior e mais interessante que isso.
- a tarefa é criar mais um arquivo de exemplo de sql e testá-lo no sqlite/db browser
- esta consulta deverá fazer uso das funções explicadas [aqui](https://sqlite.org/lang_aggfunc.html#count)
- uma consulta que diga quantos participantes cada evento terá
