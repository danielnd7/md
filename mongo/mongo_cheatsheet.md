# **Comandos básicos de MongoDB**

## **`use <BD>`**
**Sintaxis:** `use BDNueva`  
**Qué hace:** Cambia la base de datos actual a `BDNueva` (la crea en el servidor al insertar datos).  
**Devuelve:** Mensaje indicando la base de datos seleccionada.  
**Ejemplo:** `use BDNueva`

## **`db`**
**Sintaxis:** `db`  
**Qué hace:** Muestra el nombre de la base de datos actual.  
**Devuelve:** El nombre de la BD (string).  
**Ejemplo:** `db` -> `BDNueva`

## **`show databases`**
**Sintaxis:** `show databases`  
**Qué hace:** Lista todas las bases de datos en el servidor con su tamaño y si existen datos.  
**Devuelve:** Listado de BDs.  
**Ejemplo:** `show databases`

## **`db.dropDatabase()`**
**Sintaxis:** `db.dropDatabase()`  
**Qué hace:** Elimina la base de datos seleccionada y todos sus datos.  
**Devuelve:** Un documento con el resultado (ok:1 si tuvo éxito).  
**Ejemplo:** `db.dropDatabase()`

## **`db.version()`**
**Sintaxis:** `db.version()`  
**Qué hace:** Muestra la versión del servidor MongoDB.  
**Devuelve:** String con la versión.  
**Ejemplo:** `db.version()`

---

# **Operaciones CRUD (colecciones y documentos)**

## **Insertar - `insertOne`**
**Sintaxis:** `db.<colección>.insertOne({<documento>})`  
**Qué hace:** Inserta un documento en la colección.  
**Devuelve:** Resultado con `acknowledged` y `insertedId`.  
**Ejemplo:**  
```javascript
db.alumnos.insertOne({"nombre":"María","edad":21})
```

## **Insertar varios - `insertMany`**
**Sintaxis:** `db.<colección>.insertMany([{doc1},{doc2},...], { ordered: false })`  
**Qué hace:** Inserta varios documentos; `ordered:false` continúa aunque haya errores.  
**Devuelve:** Resultado con `insertedIds`.  
**Ejemplo:**  
```javascript
db.alumnos.insertMany([{"n":"A"},{"n":"B"}], {ordered:false})
```

## **Listar colecciones**
**Sintaxis:** `db.getCollectionNames()`  
**Qué hace:** Devuelve array con los nombres de las colecciones de la BD actual.  
**Devuelve:** Array de strings.  
**Ejemplo:**  
```javascript
db.getCollectionNames()
```

## **Consultar - `find`**
**Sintaxis:** `db.<colección>.find(<filtro>, <proyección>)`  
**Qué hace:** Recupera documentos que coinciden con `filtro`.  
**Devuelve:** Cursor sobre los documentos (puedes iterarlo o usar `.pretty()`).  
**Ejemplo:**  
```javascript
db.clientes.find({"nombre":"María"})
```

## **Formatear salida - `pretty()`**
**Sintaxis:** `db.<colección>.find().pretty()`  
**Qué hace:** Muestra la salida del cursor de forma legible.  
**Devuelve:** Documentos formateados en consola.  
**Ejemplo:**  
```javascript
db.clientes.find().pretty()
```

## **Renombrar colección**
**Sintaxis:** `db.<colección>.renameCollection("<NuevoNombre>")`  
**Qué hace:** Cambia el nombre de la colección.  
**Devuelve:** Documento con resultado.  
**Ejemplo:**  
```javascript
db.alumnos.renameCollection("estudiantes")
```

---

# **Actualizaciones (UPDATE)**

> ⚠️ El método `update()` está **deprecado**. Se incluye solo como referencia.

**Sintaxis (deprecado):**  
```javascript
db.<colección>.update(query, update, options)
```
**Equivalencias modernas:**  
- `updateOne()` ≈ `update(..., { multi: false })`  
- `updateMany()` ≈ `update(..., { multi: true })`  

**Ejemplo (obsoleto):**
```javascript
db.alumnos.update(
  {"_id": 11},
  {$set : {"titulación" :"GII" }}
)
```

## **`updateOne`**
**Sintaxis:** `db.<colección>.updateOne(query, updateOperator, options)`  
**Qué hace:** Actualiza el primer documento que coincide con `query`.  
**Devuelve:** Resultado con `matchedCount` y `modifiedCount`.  
**Ejemplo:**  
```javascript
db.clientes.updateOne(
  {"_id": 11},
  {$set: {"titulación":"GIS","escuela":"ETSII"}}
)
```

## **`updateMany`**
**Sintaxis:** `db.<colección>.updateMany(query, updateOperator, options)`  
**Qué hace:** Actualiza todos los documentos que coinciden con `query`.  
**Devuelve:** Resultado con `matchedCount` y `modifiedCount`.  
**Ejemplo:**  
```javascript
db.clientes.updateMany(
  {"pais":"España"},
  {$set: {"tipo":"web"}}
)
```

## **Ejemplo con opciones y operadores compuestos**
```javascript
db.clientes.update(
  { "Edad": { $gte: 18 } },
  { $set: { "mayorEdad": true }, $inc: {"Edad":1} },
  { upsert: true }
)
```

---

# **Borrado (DELETE)**

## **`deleteOne`**
**Sintaxis:** `db.<colección>.deleteOne(query, options)`  
**Qué hace:** Elimina el primer documento que coincide con `query`.  
**Devuelve:** Resultado con `deletedCount`.  
**Ejemplo:**  
```javascript
db.clientes.deleteOne({_id : 11})
```

## **`deleteMany`**
**Sintaxis:** `db.<colección>.deleteMany(query, options)`  
**Qué hace:** Elimina todos los documentos que coinciden con `query`.  
**Devuelve:** Resultado con `deletedCount`.  
**Ejemplo:**  
```javascript
db.clientes.deleteMany({"titulación":"GIS"})
```

## **`remove` (antiguo)**  
**Sintaxis:** `db.<colección>.remove({<filtro>})`  
**Qué hace:** Elimina documentos; obsoleto frente a `deleteOne/deleteMany`.  
**Ejemplo:**  
```javascript
db.colección.remove({"clave":"valor"})
```

## **Borrar colección**
**Sintaxis:** `db.<colección>.drop()`  
**Qué hace:** Elimina la colección y sus índices.  
**Devuelve:** `ok:1` si tuvo éxito.  
**Ejemplo:**  
```javascript
db.alumnos.drop()
```

---

# **Operadores de comparación (consulta)**

Sintaxis general:  
```javascript
{ atributo: { $operador: valor } }
```

- `$eq`: Igual a un valor  
- `$gt`: Mayor que  
- `$gte`: Mayor o igual que  
- `$lt`: Menor que  
- `$lte`: Menor o igual que  
- `$ne`: Distinto de  
- `$in`: Coincide con valores en un array  
- `$nin`: No coincide con valores en un array  

**Ejemplo:**  
```javascript
{"Edad": { $gte: 18 }}
```

---

# **Operadores de actualización (update operators)**

Sintaxis general:  
```javascript
{ <campo>: { $operador: valor } }
```

- `$set`: Establece/añade un valor  
- `$unset`: Elimina el campo  
- `$setOnInsert`: Valor si se inserta (upsert)  
- `$inc`: Incrementa (positivo o negativo)  
- `$mul`: Multiplica el campo  
- `$rename`: Renombra un campo  
- `$min`: Actualiza si nuevo < existente  
- `$max`: Actualiza si nuevo > existente  
- `$currentDate`: Establece fecha actual (`Date` o `Timestamp`)  

**Ejemplo:**  
```javascript
{$set: {"activo": true}, $inc: {"visitas": 1}}
```

---

# **Recuperación de documentos**

## **Búsqueda por igualdad**
```javascript
db.clientes.find({"nombre": "María"})
db.alumnos.find({"nombre": {$eq:"María"}})
```

## **Búsqueda por expresión regular**
```javascript
db.clientes.find({"nombre": /A./})
db.clientes.find({"nombre": /.u./})
```
| Carácter | Descripción | Ejemplo | Coincide con... |
| :--- | :--- | :--- | :--- |
| **`^`** | Coincide con el **inicio** de la cadena. | `^App` | Cadenas que empiezan por "App". |
| **`/i`** | (al final) -> ignora mayusculas y minusculas
| **`$`** | Coincide con el **final** de la cadena. | `le$` | Cadenas que terminan en "le". |
| **`.`** | Coincide con **cualquier** carácter (excepto nueva línea por defecto). | `c.t` | "cat", "cot", "cut", etc. |
| **`*`** | Cero o más coincidencias del elemento anterior. | `ab*c` | "ac", "abc", "abbc", etc. |
| **`+`** | Una o más coincidencias del elemento anterior. | `ab+c` | "abc", "abbc", pero no "ac". |
| **`?`** | Cero o una coincidencia del elemento anterior. | `colou?r` | "color" y "colour". |
| **`[abc]`** | Coincide con **cualquiera** de los caracteres entre corchetes. | `[aeiou]` | Cualquier vocal minúscula. |
| **`[^abc]`** | Coincide con **cualquier** carácter **que no esté** entre corchetes. | `[^0-9]` | Cualquier carácter que no sea un dígito. |
| **`\d`** | Coincide con cualquier dígito. | `\d{3}` | Tres dígitos seguidos. |
| **`\|`** | Operador **OR** (O). | `(cat|dog)` | "cat" o "dog". |
| **`\`** | Se utiliza para **escapar** metacaracteres. | `\.` | Un punto literal. |


## **Comparadores numéricos**
```javascript
db.clientes.find({"Edad": {$gte: 18}})
```

## **Proyección (elegir campos)**
```javascript
db.clientes.find({"nombre":{$eq: "Alfredo"}},{_id:1, ciudad:1}) 
// Se muestran solo el ID y CIUDAD de los clintes con el nombre Alfredo
db.clientes.find({"nombre":"Alfredo"},{ciudad:0})
// Se muestra todo menos CIUDAD de clientes llamados Alfredo
db.clientes.find({},{_id:0, nombre:1})
// Se muestra el nombre de todos los clientes pero no su ID
db.clientes.find({},{_id:0})
// Se muesta todo menos ID para todos clientes
```

---

# **Operadores lógicos: AND / OR**

## **`$and`**
```javascript
db.clientes.find({
  $and: [
    {"Edad":{$gte:18}}, 
    {"nombre":"Juanito"}
  ]
})
```

## **`$or`**
```javascript
db.clientes.find({
  $and : [
    { $or : [ { "nombre":"Juanito" }, {"nombre":"Belen"} ] },
    { $or : [ { "Edad": 18 }, { "Edad": { $gt : 18 } } ] }
  ]
})
```

---

# **Documentos embebidos**

```javascript
db.clientes.find({
  "dirección": {
    "calle": "C/ Mayor, 12",
    "ciudad": "Cártama",
    "provincia": "Málaga",
    "CP" : "25933"
  }
}).pretty()

db.clientes.find({"dirección.provincia":"Málaga"})
```

---

# **Arrays**

```javascript
db.clientes.find({"teléfonos":["954556677","677445566"]})
// solo el que tenga exactamente el array especificado

db.clientes.find({"teléfonos":"954556677"})
//  Devuelve los documentos que contengan en el array teléfonos el elemento especificado.
```


---

# **Ordenación de Documentos** 
Por defecto el orden de insercion

## **`sort()`**
**Sintaxis:** `db.coleccion.sort({campo1 : tipoOrdencion, campo2 : tipoOrdenacion, ... campoN:tipoordenacion});`  
**Qué hace:** ordenar el resultado.  
**Devuelve:** Devuelve los documentos ordenados  
**Ejemplo:**  
```javascript
db.clientes.find({"nombre" : "Ana"}).sort({"_id" : 1, "edad" : 1})
// clientes cuyo nombre sea “Ana” ordenado por el id de objeto y la edad (de forma ascendente)
```
### Tipo de ordenación:  
> Ascendente:1 

> Descendente:-1

## **`count()`**
**Sintaxis:** `db.nombreColección.count(condición, opciones)`  
`db.clientes.countDocuments() == db.clientes.find().count()`   
**Devuelve:** número de documentos que satisfacen una condición  
**Ejemplo:**  
```javascript
db.clientes.find({"nombre" : "Eva"}).count()
db.clientes.countDocuments({"nombre" : "Eva"})
```

## **`limit()`**
**Qué hace:** limita el número de documentos que devuelve una consulta   
**Ejemplo:**  
```javascript
db.clientes.find().limit(2)
```

## **operador `$exists`**
**Sintaxis:** ` {campo: {$exists:<boolean>}`  
**Devuelve:** devuelve todos los documentos que tienen (o no) un determinado campo    
**Ejemplo:**  
```javascript
db.clientes.find({“primer_pedido” : {$exists:true}}) 
db.alumnos.find({“edad” : {$exists:false}})
```

## **operador `$type `**
**Sintaxis:** `{campo: {$type:<tipo>}`  
**Devuelve:** los documentos cuyo campo sea de un determinado tipo   
**Ejemplo:**  
```javascript
db.clientes.find({“edad”: {$type: “int”}})
db.clientes.find({“dirección.calle”: {$type:”string”}})
```

---

# **Índices**
## **Creación de un índice:**
**Sintaxis:** ` db.coleccion.createIndex(claves, opciones)`  
//  _id  ->  único índice creado por defecto   
**Ejemplo:** `db.clientes.createIndex({nombre:1 primer_apellido:1},{name:" nombre y apellidos"}`  
* defaul name: `db.clientes.createIndex({"edad":1}) => name: edad_1`  

## **Borrado de un índice:**
```javascript
db.coleccion.dropIndex(“nombreIndice”)
db.coleccion.dropIndex(“especificación del campo”: -1)
db.coleccion.dropIndexes() // drops all indexes
```

## **Ver los índices de una coleccion:**
`db.clientes.getIndexes()`

## **Índices simples o (de un solo campo)**
* (se aplican a un solo campo de los documentos de una colección)  
```javascript
db.clientes.createIndex({"nombre" : 1}) // orden ascendente
db.clientes.createIndex({"nombre" : -1}) // descendente
db.clientes.createIndex({"dirección.provincia" : 1}) // documentos embebidos
``` 
## **Índices compuestos**
* se aplican a varios campos de los documentos de una colección  

`b.people.createIndex({ nombre: 1, edad: 1, ciudad: 1 })`  
This index sorts and stores documents first by `nombre`, then (for documents with the same nombre) by `edad`, and finally (if both are equal) by `ciudad`.
- "Álvaro", 35
- "Álvaro", 18
- "María", 30
- "María", 21

You can use a compound index **only starting from the leftmost field(s)** — meaning:

| Query uses fields                               | Will use index? | Explanation                      |
| ----------------------------------------------- | --------------- | -------------------------------- |
| `{ nombre: "Ana" }`                             | ✅ Yes           | Uses first field                 |
| `{ nombre: "Ana", edad: 25 }`                   | ✅ Yes           | Uses first + second field        |
| `{ nombre: "Ana", edad: 25, ciudad: "Madrid" }` | ✅ Yes           | Uses all three fields            |
| `{ edad: 25 }`                                  | ❌ No            | Skips the first field (`nombre`) |
| `{ ciudad: "Madrid" }`                          | ❌ No            | Skips first and second fields    |
| `{ edad: 25, ciudad: "Madrid" }`                | ❌ No            | Still skips `nombre`             |

---
```
nombre ↑
 ├── edad ↑
 │    └── ciudad ↑
```
## **Índices únicos**
`db.pacientes.createIndex({"nombre" : 1},{"unique"  : true})`  
obligados a contener valores únicos  

- Intento insertar valores repetidos -> ERROR

## **Índices sparse (разреженный)**
`db.pacientes.createIndex({"nombre" : 1},{"sparse" : true})`  
solo incluye los documentos cuyo campo indexado existe  

- por defecto se indexan con null los docs que no tengan el campo indexado

## **Índices parciales (mas preferible que `sparce`)**
Sólo indexan los documentos de una 
colección que satisfacen una condición (expresión).  
```javascript
db.pacientes.createIndex({"nombre":1}, 
{partialFilterExpression: 
{"edad" : {$gt : 18}}
})

db.pacientes.createIndex({"nombre":1}, 
{partialFilterExpression: 
{"nombre" : {$exists : true}} 
})
```
## **Intersección de índices**
```javascript
db.clientes.createIndex( { "nombre" : 1 } )
db.clientes.createIndex( { "edad" : -1 } )
```
MongoDB puede combinar varios índices simples (por ejemplo, uno en `nombre` y otro en `edad`) para optimizar una misma consulta.

`db.pacientes.createIndex({“direccion.ciudad":1}`  
Podríamos crear el índice sobre el nodo principal 
`dirección`, pero esto solo nos ayudaría en el caso 
de que se realizaran consultas sobre el subdocumento 
completo y con los campos exactamente en el mismo orden.

## **Indexación de arrays**
- **sólo uno de los campos del índice puede ser un array.**  

` db.usuarios.createIndex({“categorias":1,“etiquetas":1})`  
```java
db.usuarios.insert({ "categorias": ["juegos","libros","películas"],  
"tags": ["horror","scifi","historia"])
// => ERROR Cannot index parallel arrays [categories] [tags]
```
-  **si realizamos consultas posicionales 
sobre el array no se usará el índice**
(elementos indexados del array no tienen 
en cuenta el orden)

### Consultas totalmente cubiertas por índices
**Son aquellas 
consultas cuyos campos consultados y devueltos están incluidas 
en el índice. (las mas eficientes)**   
`db.clientes.createIndex({“nombre":1,“edad":1})`   
Devoulviendo `nombre` o `edad`

## **`explain()`**
informacion sobre el plan de ejecucion:  
` db.colección.find().explain() o db.colección.explain().find()`  

## **`hint()`**
permite indicar en una consulta qué índice 
utilizar:  
```javascript
db.clientes.find().hint( { edad: 1 } )– O bien 
db.clientes.find().hint( “edad_1”)
```
Para obligar no usar ningún índice: `{ $natural : 1 }` natural order (or `-1` for reverced order)   
```javascript
db.clientes.find().hint( { $natural : 1 } )
```
---
---
---

## Recomendación inserción masiva en una colección:
1. Borrar los índices con db.colección.dropIndexes()
2. Inserción de documentos
3. Re-Creación de índices db.colección.createIndex()

## **`reIndex()`**
**Borra todos los índices de una colección y los 
recrea.**  
` db.clientes.reIndex() `