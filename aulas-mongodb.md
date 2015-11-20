#MongoDB

##Aula 1

##Aula 2

##Aula 3

###Sintaxe para fazer buscas

```
db.colecao.find({clausula}, {campos})
  {clausula} - como se fosse o "where"
  {campos}   - como se fosse o "select"
  
Exemplo:
db.pokemons.find({name: 'Pikachu'}, {name: 1, description: 1, _id: 0})
```
###Operadores de comparação
```
<  é $lt
<= é $lte
>  é $gt
>= é $gte

Exemplo:
var query = {height: {$lt: 0.5}}
```

###Operadores lógicos
```
$or  é OU 
$nor é NOT OR (todos os registros que não vieram na busca com OR)
$and é AND


Exemplo:
var query = {$or: [{name: 'Pikachu'}, {height: 1.69}]}

```

###Operador existencial

Verifica se o campo existe

```
db.collection.find({campo: {$exists: true}});
```

##Aula 4 - Parte 1

###update
Diferente do save, não precisa buscar o documento antes. Possui 3 parametros:
- query
- modificação
- options

####Sintaxe

```
db.colecao.update(query, mod, options);
```

####Operadores de modificação

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

####Operadores de array

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
  
  
##Aula 4 - Parte 2

### Último parâmetro do update: options

Serve para configurar valores diferentes do padrão para o update.

####Sintaxe:
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

####Operadores

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

####Sintaxe: 

```
db.collection.remove(query);
```
































