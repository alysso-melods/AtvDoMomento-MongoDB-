# Momento 

* Quantos funcionarios da empresa Momento trabalham no departamento de vendas?

R: 10 funcionários.

Q: 
```js
db.funcionarios.countDocuments({departamento: ObjectId("85992103f9b3e0b3b3c1fe71")})
```

* Inclua suas próprias informações no departamento de Tecnologia da empresa.

Q: 
```js
db.funcionarios.insertOne
({
  "nome": "Alysson Melo dos Santos",
  "telefone": "119.492.1825",
  "email": "alysson.meloDSantos@gmail.org",
  "dataAdmissao": "2005-12-30",
  "cargo": "Web Developer",
  "salario": 8000,
  "departamento": ObjectId("85992103f9b3e0b3b3c1fe74")
})
```

* Agora diga, quantos funcionários temos ao total na empresa?

R: No total, há 24 funcionários na empresa.

Q:
```js
db["funcionarios"].countDocuments()
```

* E quanto ao Departamento de Tecnologia?

R: 6 funcionários no total.

Q:
```js
db.funcionarios.countDocuments({ "departamento": ObjectId("85992103f9b3e0b3b3c1fe74") })
```

* Qual a média salarial do departamento de tecnologia?

R: A média salarial é 5133.

Q:
```js
db.funcionarios.aggregate
([
  {$match: { departamento: ObjectId("85992103f9b3e0b3b3c1fe74")}},
  {$group: { "_id": null, "mediasalarial":{ $avg: "$salario" }}}
])
```

* Quanto o departamento de Vendas recebe em salários?

R: O departamento de vendas recebe no 95.100 reais no total.

Q:
```js
db.funcionarios.aggregate
([
  {$match: { departamento: ObjectId("85992103f9b3e0b3b3c1fe71")}},
  {$group: {_id: null, totalSalario: { $sum: "$salario" }}}
])
```

* Um novo departamento foi criado. O departamento de Inovações. Ele será locado no Brasil. Por favor, adicione-o no banco de dados da empresa colocando quaisquer informações que você achar relevantes.

Q:
```js
db.departamentos.insertOne
(
{_id: ObjectId("85992103f9b3e0b3b3c1fe77"),nome:"Inovações", escritorio: ObjectId("5f8b3f3f9b3e0b3b3c1e3e3e")}
)
```

* O departamento de Inovações está sem funcionários. Inclua alguns colegas de turma nesse departamento.

Q:
```js
db.funcionarios.insertMany
({
  "nome": "Emily Santana",
  "telefone": "120.882.1565",
  "email": "emy.sAntAnA@hotmail.org",
  "dataAdmissao": "1992-10-26",
  "cargo": "Gerente de Inovação",
  "salario": 10000,
  "departamento": ObjectId("85992103f9b3e0b3b3c1fe77")
},

{
  "nome": "Everton Alves",
  "telefone": "170.122.7804",
  "email": "everton.alvesss@hotmail.org",
  "dataAdmissao": "1998-12-22",
  "cargo": "Consultor de Inovação Disruptiva",
  "salario": 6010,
  "departamento": ObjectId("85992103f9b3e0b3b3c1fe77")
},

{
  "nome": "Luiz Felipe",
  "telefone": "104.190.7154",
  "email": "Luizzz.OldAsF*@gmail.org",
  "dataAdmissao": "1899-10-22",
  "cargo": "Cientista de Dados de Inovação",
  "salario": 7600,
  "departamento": ObjectId("85992103f9b3e0b3b3c1fe77")
},

{
  "nome": "Lucas Miranda",
  "telefone": "984.107.1121",
  "email": "lucas.mirandaa@gmail.org",
  "dataAdmissao": "2028-11-23",
  "cargo": "Engenheiro de Inovação Tecnológica",
  "salario": 8500,
  "departamento": ObjectId("85992103f9b3e0b3b3c1fe77")
})
```

* Quantos funcionarios a empresa Momento tem agora?

R: 28 funcionários no total.

Q:
```js
db["funcionarios"].countDocuments()
```

* Quantos funcionários da empresa Momento possuem conjuges?

R: 21 funcionários.

Q:
```js
db["funcionarios"].countDocuments({ "dependentes.conjuge": null });
```

* Qual a média salarial dos funcionários da empresa Momento, excluindo-se o CEO?

R: a média salarial sem o CEO é 9436 reais.

Q:
```js
db.funcionarios.aggregate
([
  {$match:{cargo: {$ne: "CEO"}}},
  {$group: {"_id": null,"total": {$avg: "$salario"}}}
])
```

* Qual o departamento com a maior média salarial?

R: Recursos Humanos

Q:
```js
db.funcionarios.aggregate([
  {$match: { cargo: { $ne: "CEO" } }},
  {$group: {_id: "$departamento", mediaSalarial: { $avg: "$salario" }}},
  {$sort: { mediaSalarial: -1 }},
  {$limit: 1}
]);
```

* Qual o departamento com o menor número de funcionários?

R: Departamento de Inovações.

Q:
```js
db["funcionarios"].aggregate([ 
  {$match:{cargo: {$ne: "CEO"}}},
  { $group: { _id: "$departamento", totalFuncionarios: { $sum: 1 }}},
  { $sort: { totalFuncionarios: 1 } }, 
  { $limit: 1 } 
]);
```

* Pensando na relação quantidade e valor unitario, qual o produto mais valioso da empresa?

R: O Sabre de Luz do Mace Windu é o produto mais valioso da empresa.

Q:
```js
```

* Qual o produto mais vendido da empresa?

R: O Laço da Verdade é o produto mais vendido.

Q:
```js
db["vendas"].aggregate([
  {$group: {_id: "$produto",totalQuantidade: { $sum: "$quantidade"}}},
  {$sort: { totalQuantidade:-1}},
  {$limit:1}])
```

* Qual o produto menos vendido da empresa?

R: Os produtos menos vendidos são o Uniforme do Superman e o Fake Batarang

Q:
```js
db["vendas"].aggregate([
  {$group: {_id: "$produto",totalQuantidade: { $sum: "$quantidade"}}},
  {$sort:{ totalQuantidade:1}},
  {$limit:2}])
```
