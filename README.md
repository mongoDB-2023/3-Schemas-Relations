# 3-Schemas-Relations

1. [Resseting your Database](#schema1)
2. [Why do we you use schemas?](#schema2)
3. [Structuring Documents](#schema3)
4. [Data Types](#schema4)
5. [Undestanding Relations](#schema5)

<hr>

<a name="schema1"></a>

## 1. Resseting your Database

- Eliminar una base de datos

```
use databaseName
db.dropDatabase()
```
- Eliminar una colección
```

db.mycollection.drop()
```


<hr>

<a name="schema2"></a>

## 2. Why do we you use schemas?

![schema](./img/schema1.png)
![schema](./img/schema2.png)


Documentos con diferentes schemas

```
shop> db.products.find()
[
  {
    _id: ObjectId('6560910f908c91a7cd6a8ada'),
    name: 'A book',
    price: 12.99
  },
  {
    _id: ObjectId('6560917f908c91a7cd6a8adb'),
    title: 'T-shirt',
    seller: { name: 'Max', age: 29 }
  }

```

![schema](./img/schema3.png)


<hr>

<a name="schema3"></a>

## 3. Structuring Documents

![schema](./img/schema4.png)

- Diferentes eschemas
```
shop> db.products.find()
[
  {
    _id: ObjectId('656093a9908c91a7cd6a8adc'),
    name: 'A book',
    price: 12.99
  },
  {
    _id: ObjectId('656093cd908c91a7cd6a8add'),
    name: 'a T-shirt',
    price: 15
  },
  {
    _id: ObjectId('65609463908c91a7cd6a8ade'),
    name: 'A Computer',
    price: 1299,
    details: { cpu: 'Intel i7 8770' }
  }
]

```
- Esquemas iguales
```
[
  {
    _id: ObjectId('6560955e908c91a7cd6a8adf'),
    name: 'a T-shirt',
    price: 15,
    details: null
  },
  {
    _id: ObjectId('6560956c908c91a7cd6a8ae0'),
    name: 'A book',
    price: 12.99,
    details: null
  },
  {
    _id: ObjectId('6560957e908c91a7cd6a8ae1'),
    name: 'A Computer',
    price: 1299,
    details: { cpu: 'Intel i7 8770' }
  }
]
```

En MongoDB, a diferencia de las bases de datos relacionales, no se define un esquema estricto para las colecciones. 
MongoDB es una base de datos NoSQL orientada a documentos, lo que significa que los documentos en una colección 
pueden tener estructuras diferentes.

Sin embargo, aunque MongoDB es conocido por su esquema dinámico, en la práctica, a menudo es beneficioso establecer 
ciertas convenciones o acuerdos sobre la estructura de tus documentos para garantizar la coherencia y facilitar la 
consulta y el mantenimiento de los datos. Esto se llama "esquema implícito" o "esquema flexible".

![schema](./img/schema6.png)

<hr>

<a name="schema4"></a>

## 4. Data Types

![schema](./img/schema5.png)

Ejemplo

```
companyData> db.companies.find()
[
  {
    _id: ObjectId('6560a511908c91a7cd6a8ae2'),
    name: 'Fresh Apples Inc',
    isStartup: true,
    employess: 33,
    funding: 12345678900123458000,
    details: { ceo: 'Mark Super' },
    tags: [ { title: 'super' }, { title: 'perfect' } ],
    foundingDate: ISODate('2023-11-24T13:28:49.495Z'),
    insertAt: Timestamp({ t: 1700832529, i: 1 })
  }
]

```


<hr>

<a name="schema5"></a>

## 5. Undestanding Relations

![schema](./img/schem7.png)

 ### One to One Relations - Embedded
 
- Creamos la base de datos `hospital`
```
test> use hospital
switched to db hospital
```
- Creamos la colección  `patients`  con el primer paciente.
```
hospital> db.patients.insertOne({name:'Max',age:29,diseaseSummary:"summary-max-a"})
{
  acknowledged: true,
  insertedId: ObjectId('65645f1f98a22cd4957ce658')
}
```
- Creamos una segunda coleccion `summary-max-1`
```
hospital> db.diseaseSummaries.insertOne({_id:"summary-max-a",diseases:['cold','broken leg']})
{ acknowledged: true, insertedId: 'summary-max-a' }
```
- Nuestra app necesita tener todos los datos del paciente.
- 1º Localizar el paciente:
```
hospital> db.patients.find({ name: 'Max' }).forEach(function (patient) { print(patient.diseaseSummary); });
summary-max-a
```
- 2º Guardamos el valor de `diseaseSummary`
```
var dsid = db.patients.findOne().diseaseSummary
```
- 3º Obtenemos de la otra colección los valores deseados
```
hospital> db.diseaseSummaries.findOne({_id:dsid})
{ _id: 'summary-max-a', diseases: [ 'cold', 'broken leg' ] }
```

Esto no es buena manera de trabajar, vamos a ver esto con relacions 1 a 1.
```
db.patients.insertOne({name:'Max',age:29,diseaseSummary:{diseases:['cold','broken leg']}})
hospital> db.patients.findOne()
{
  _id: ObjectId('65646854404a9eb424ecebc2'),
  name: 'Max',
  age: 29,
  diseaseSummary: { diseases: [ 'cold', 'broken leg' ] }
}
```
### One to One - Using References

- Creamos una nueva base de datos
```
test> use carData
switched to db carData
carData> db.persons.insertOne({name:'Max', age:29, salary:3000})
{
  acknowledged: true,
  insertedId: ObjectId('65647a56fe3f6890e9eceb49')
}

db.persons.findOne()
{
  _id: ObjectId('65647be3fe3f6890e9eceb4a'),
  name: 'Max',
  age: 29,
  salary: 3000
}
```
- Hacemos la referencia por medio del `ObjectId ` 
```
carData> db.cars.insertOne({model:'BMW', price:40000, owner:ObjectId('65647be3fe3f6890e9eceb4a')})
{
  acknowledged: true,
  insertedId: ObjectId('65648375fe3f6890e9eceb4b')
}

```






