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
db.clientes.find({"teléfonos":"954556677"})
```

---


