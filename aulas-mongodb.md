Resumo - Aulas MongoDB
======================

## Aula 1

## Aula 2

### Comandos básicos

- *use*: especifica o banco de dados que será usado. Se não existir ele cria.
  - Sintaxe: use nome_do_banco
- *show dbs*: exibe os banco de dados criados e o espaço utilizado. O banco só é criado quando se faz uma primeira inserção.


### insert()

Usado para inserir documentos
- Sintaxe: db.collection.insert(doc);

### save()

Insere ou atualiza um documento. Para atualizar, é necessário recuperar o 
documento com find() - retorna um Cursor - ou findOne() - retorna um objeto -, 
realizar a alteração e depois salvar com save();
- Sintaxe: db.collection.save(doc);



## Aula 3

### Sintaxe para fazer buscas

```
db.colecao.find({clausula}, {campos})
  {clausula} - como se fosse o "where"
  {campos}   - como se fosse o "select"
  
Exemplo:
db.pokemons.find({name: 'Pikachu'}, {name: 1, description: 1, _id: 0})
```
### Operadores de comparação
```
<  é $lt
<= é $lte
>  é $gt
>= é $gte

Exemplo:
var query = {height: {$lt: 0.5}}
```

### Operadores lógicos
```
$or  é OU 
$nor é NOT OR (todos os registros que não vieram na busca com OR)
$and é AND


Exemplo:
var query = {$or: [{name: 'Pikachu'}, {height: 1.69}]}

```

### Operador existencial

Verifica se o campo existe

```
db.collection.find({campo: {$exists: true}});
```

## Aula 4 - Parte 1

### update
Diferente do save, não precisa buscar o documento antes. Possui 3 parametros:
- query
- modificação
- options

#### Sintaxe

```
db.colecao.update(query, mod, options);
```

#### Operadores de modificação

- *$set*: modifica ou cria (se não existir) um valor
  - Sintaxe: {$set: {campo: valor}}
  - db.pokemons.update({name: 'Pikachu'}, {$set: {attack: 120}});
- *$unset*: deleta campos num documento
  - Sintaxe: {$unset: {campo: 1}}
  - db.pokemons.update({name: 'Pikachu'}, {$unset: {height: 1}});
- *$inc*: incrementa um valor no campo com a quantidade desejada. Caso o campo 
  não exista, o $inc vai cria-lo. Para decrementar, basta usar o operador 
  negativo.
  - Sintaxe: {$inc: {campo: valor}}

#### Operadores de array

- *$push*: adiciona um valor num campo array. Caso o campo não exista, ele cria.
  Se o campo não for um array, ocorre um erro.
  - Sintaxe: {$push: {campo: valor}}
- *$pushAll*: Adiciona um array a outro array. Caso não exista, ele cria.
  Se o campo não for um array, ocorre um erro.
  - Sintaxe: {$pushAll: {campo: [array_de_valores]}}
- *$pull*: retira valores de um array. Não faz nada se não existir o campo.
  Se o campo não for um array, ocorre um erro.
  - Sintaxe: {$pull: {campo: valor}}
- *$pullAll*: retira um array de valores de um campo array. Não faz nada se não 
  existir o campo. Se o campo não for um array, ocorre um erro.
  - Sintaxe: {$pullAll: {campo: [array_de_valores]}}
  
  
## Aula 4 - Parte 2

### Último parâmetro do update: options

Serve para configurar valores diferentes do padrão para o update.

#### Sintaxe:
```
{
  upsert: boolean,
  multi: boolean,
  writeConcern: document
}
```

- *upsert*: caso não seja encontrado nenhum documento pela *query* passada como 
  parâmetro, o update vai inserir o objeto caso o upsert seja true. Padrão é 
  false.
  - $set: é usado para setar um valor no update
  - $setOnInsert: se ocorrer um insert no update (com upsert:true), para que 
    o mongodb não insira somente o valor do update pode ser usado o $setOnInsert
    que pode conter um objeto completo. O mongodb vai inserir o valor de $set 
    também.
  
- *multi*: o mongodb por padrão não deixa atualizar mais de um documento por 
  vez, a não ser que o parâmetro multi seja true. É uma medida de segurança, 
  é como se ele não deixasse fazer update sem where.

- *writeConcern*: relacionado a preocupação de escrita do mongodb. Quando 
  inserções, atualizações e exlusões tem um write concern fraco, operações de 
  escrita retornam rapidamente (o mongodb não verifica se a escrita realmente 
  ocorreu).
    

### Usando find com arrays

Usado para fazer busca dentro de arrays no mongodb.

#### Operadores

- $in: retorna os documentos que possuem ALGUM dos valores passados dentro do array.
  - Sintaxe: {campo: {$in: [Array_de_Valores]}}
- $nin: só retorna documentos que NÃO possuem algum dos valores passados dentro do array.
  - Sintaxe: {campo: {$nin: [Array_de_Valores]}}
- $all: só retorna documentos se TODOS os valores foram encontrados.
  - Sintaxe: {campo: {$all: [Array_de_Valores]}}


### Operadores de negação

- $ne: Not Equal. Não aceita regex e os documento precisam ter o campo pesquisado.
  - Sintaxe: {campo: {$ne: valor}}
- $not: Qualquer valor que não seja o especificado. Aceita regex.
  - Sintaxe: {campo: {$ne: valor}}

### Comando remove() 

Remover documentos do banco de dados
Para remover a coleção toda usar a função drop()

#### Sintaxe: 

```
db.collection.remove(query);
```

## Aula 5

### Performance find().length e count()

- db.restaurantes.find().length() 
  + mostra o total de registros da collection, mas dessa forma ele traz todos os registros pra memória pra depois contar.
- db.restaurantes.count() 
  + forma mais performática para saber o total de registros. 

### distinct()

Como no SQL, busca somente os valores distintos da collection.

- Sintaxe: 
  + db.collections.distinct(nome_coluna)
  + Ex.: db.pokemons.distinct('type') -> só vai listar todos os tipos de pokemons cadastrados.

### mongoexport

```
Exemplo:
mongoexport --host 127.0.0.1 --db be-mean --collection pokemons --out pokemons.json
```

### limit() e skip()

Usado para fazer paginação no mongodb.

- limit()
  - Sintaxe:
    - db.collection.find(query).limit(numero_registros)
    - limit() é como se fosse o TOP do SQL Server, traz os primeiros X registros que encontrar
- skip()
  - Sintaxe: 
    - db.collection.find(query).limit(numero_registros).skip(numero_registros_pular)
    - Dado que o limit() vai buscar sempre os X primeiros registros, o skip() pega os próximos registros de acordo com o parâmetro. Usar a formula numero_registros * pagina
    - Page 1 - db.collection.find(query).limit(10).skip(10 * 0)
    - Page 2 - db.collection.find(query).limit(10).skip(10 * 1)

### group()

Agrupar registros passando uma função de reduce no group.

#### Sintaxe:
```
db.collection.group({
  initial: {},
  cond: {},
  reduce: function(){},
  finalize: function(){}
});
```
Onde, 
- initial: definição incial do objeto que será retornado
- cond: condição para filtro do group
- reduce: função de reduce
- finalize: função que roda depois que o mongodb já processou os registros, pode-se adicionar mais propriedades no objeto retornado

#### Exemplos:
Mostra a contagem total de cada tipo dos pokemons, sendo que a defesa seja maior que 40
```
db.pokemons.group({
  initial: {total: 0},
  cond: {defense: {$gt: 40}},
  reduce: function(curr, result) {
    curr.types.forEach(function(type){
      if (result[type]) {
        result[type]++;
      } else {
        result[type] = 1;
      }
      result.total++;
    });
  }
});
```
Soma o todas as defesas e ataques dos pokemons e adiciona 2 propriedades no final que é a média dos valores.
```
db.pokemons.group({
  initial: {total: 0, defense: 0, attack: 0},
  reduce: function (current, result) {
    result.total++;
    result.defense += current.defense;
    result.attack += current.attack;
  },
  finalize: function (result) {
    result.defense_avg = result.defense / result.total;
    result.attack_avg = result.attack / result.total;
  }
});
```

### aggregate()

Parecido com o group(), mas já possui funções prontas como sum e avg.

#### Sintaxe: 
```
db.collection.aggregate([{$match: {}}, {$group: {}]);
```
Onde,
- $match: pode-se aplicar qualquer condição para filtro
- $group: atributo de agrupamento
  + $avg: faz a média
  + $sum: soma o valor do campo

#### Exemplo:

```
db.pokemons.aggregate({
  $group: {
    _id: {},
    defense_avg: {$avg: '$defense'},
    attack_avg: {$avg: '$attack'},
    defense: {$sum: '$defense'},
    attack: {$sum: '$attack'} ,
    total: {$sum: 1}
  }
});
```

## Aula 6

### Relacionamentos entre coleções

Dois tipos

#### Manual

Dado um array de ids gravado em uma coleção, cria-se uma função que recebe um id e busca na outra coleção pelo documento.

Ex:
A coleção inventário possui o seguinte elemento:
```
[{
  name: 'Inventario Teste',
  pokemons: [
    {_id: ObjectId('123')},
    {_id: ObjectId('456')},
    {_id: ObjectId('789')}
  ]
}]
```
A função deve ser criada da seguinte maneira:
```
  var pokemons = [];
  var getPokemons = function(id){pokemons.push(db.pokemons.findOne(id))};
  var invt = db.inventario.findOne();  // Vai recuperar o objeto inventário acima
  invt.pokemons.forEach(getPokemon);
```
Ao fazer isso, o array pokemons estara preenchido com os documentos completos de pokemon.


### DBRef

Convenção para representar relacionamentos de documentos

- $ref: nome da coleção referenciada
- $id: o ObjectId do documento referenciado
- $db: database onde a coleção referenciada se encontra

Usado quando o documento está em outra database.


## Aula 6 - Parte 2


### explain()

Exibe o plano do mongodb para executar uma consulta. 

Exemplo: 
```
db.restaurantes.find({"name": "Chin Chin Thai Kitchen"}).explain('executionStats');
```
  
### Indices

Assim como no SQL, usado para fazer pesquisas mais rápidas no banco. 

Criando um indice:
- Pode ser criado com -1, depende da necessidade
- Pode ser criado um indice composto (com mais de um campo)

```
db.restaurantes.createIndex({name: 1});

```

Listando os indices de uma collection:
```
db.restaurantes.getIndexes();
``` 

Removendo um indice:
- Se criar com -1, deve remover com -1 também
```
db.restaurantes.dropIndex({name: 1});
```


### GridFS

É o sistema de arquivos do mongodb para armazenar arquivos binários maiores que 
16Mb, já que até esse tamanho pode ser guardado no próprio JSON com um BSON.
Como se fosse o BLOB no SQL. 

O GridFS cria duas coleções no banco de dados:
- *fs.chunks*: fica os arquivos binários dividos em pequenas partes chamdas 
chunks. Cada chunk é um documento contendo 255Kb de dados.
- *fs.files*: é armazenado os metadados do arquivo com tamanho, md5, nome do 
arquivo, data do upload, etc.


* É recomendável criar um servidor específico somente para o GridFS para que as 
configurações sejam feitas pensando em otimizar o GridFS.

#### Como usar

Para inserir um arquivo, usar o binário _mongofiles_ passando:
- -d nome do banco
- -h host
- put nome do arquivo

Exemplo:

```
mongofiles -d be-mean-files -h 127.0.0.1 put nome_do_arquivo.mp4
```

### Replica

#### Etapas da replicação

- Initial Sync: ocorre no início, quando uma Replica copia todos os dados de 
outra.
  - Clona todos os bancos daquela replica.
  - Aplica as alterações para o conjunto de dados. Usando o oplog a partir da 
    fonte, o mongod atualiza seus dados para refletir o estado atual.
  - Construir todos os indices que faltam em todas as coleções (menos _id)
  - Muda de estado do normal para o secundário, pois foi criado o secundário.

- Replication: membros do conjunto Replica replicam dados continuamente após a 
sincronização.
  - mongodb aplica as operações no primário e em seguida no oplog do primário.
  - membros secundários copiam e aplica essas operações .

oplog: é o log de alterações com todos as operações de modificações.

#### Porque usar

Garantir que os dados existam em outro lugar e caso o servidor principal caia,
pode-se levantar outro com a replica.


#### Como usar 

- Passo 1
Criar 3 diretorios na pasta /data, rs1, rs2, rs3
- Passo 2
Iniciar as três replicas

```
mongod --replSet replica_set --port 27017 --dbpath /data/rs1
mongod --replSet replica_set --port 27018 --dbpath /data/rs2
mongod --replSet replica_set --port 27019 --dbpath /data/rs3
```

Pode-se criar um script para iniciar as replicas: 
```
mongod --replSet replica_set --port 27017 --dbpath /data/rs1 --logpath /data/rs1/log.txt --fork
mongod --replSet replica_set --port 27018 --dbpath /data/rs2 --logpath /data/rs2/log.txt --fork
mongod --replSet replica_set --port 27019 --dbpath /data/rs3 --logpath /data/rs3/log.txt --fork
```

- Passo 3
Iniciar o mongo na porta da primeira replicaset. Ela será nosso servidor primário:
```
mongo --port 27017
```

- Passo 4
Na replicaset primária, iniciar a replicação com o comando:

```

rsconf = {
  "_id": "replica_set",
  "members": [
    {
      "_id": 0,
      "host": "127.0.0.1:27017"
    }
  ]
}

rs.initiate(rsconf);
```
O terminal mudará para [PRIMARY]


- Passo 5

Adicionar as replicas secundárias. Dentro da primária, fazer:

```
rs.add("127.0.0.1:27018");
rs.add("127.0.0.1:27019");
```

* Não será possivel rodar nenhum comando nas replicas secundárias, só na 
primária.


- Passo 6

Para saber o status da replicação, rodar no primário:

```
rs.status()
```

Para ver o status do oplog, executar:

```
rs.printReplicationInfo()
```

- Passo 7

É possivel rebaixar uma replica primária de modo que force o mongodb eleger 
uma outra réplica secundária como primária. Para isso, executar na replica 
primária:

```
rs.stepDown();
```























