# Mongo

## Usuń wszystko z bazy danych
```js
use dbName;
db.auth("login", "pass");
db.getCollectionNames().forEach((collection_name) => { 
  if (collection_name.indexOf("system.") == -1) 
       db[collection_name].drop();
  else  
       db.collection_name.remove({}); 
});
```
