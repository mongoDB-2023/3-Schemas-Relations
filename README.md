# 3-Schemas-Relations

1. [Resseting your Database](#schema1)
2. [Why do we you use schemas?](#schema2)
3. [Structuring Documents](#schema3)

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
