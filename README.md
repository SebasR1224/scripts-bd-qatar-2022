# Scripts Base de Datos Qatar 2022
Actividad 2 Bases de datos avanzadas

## Replicación de Base de Datos:

El proceso de replicación en MONGODB se hace a través del conjunto de replicación
(replica set), el cual es un conjunto de instancias de MONGO que están sincronizadas
manteniendo la misma información.

### Paso 1 - Creación del Set de Replicas

Creamos varias instancias de MongoDB que actuan como servidores y se relacionan para mantener copias de datos.

```JavaScript
    replicaSET = new ReplSetTest({name: "qatarReplicaSet", nodes: 5})
```

Contenido del objeto ReplSetTest

![replset-obj](/replset-obj.png)

### Paso 2 - Arrancar los procesos de la replica

Arrancamos los nodos de la replica

```JavaScript
    replicaSET.startSet();
```

Activamos la replica

```JavaScript
    replicaSET.initiate();
```

Se observan los puertos donde se han levantado cada nodo:
![replset-initiate](/replset-initiate.png)


### Paso 3 - Prueba del grupo de replica

Establecemos una nueva conexión con el nodo primario y creamos la DB

```JavaScript
    conn = new Mongo("DESKTOP-MMLHIU2:20000")
    qatar2022M = conn.getDB("qatar-2022")
```

preguntamos si es el nodo primario

```JavaScript
    qatar2022M.isMaster()
```

![qatar2022-node-primary](/qatar2022-node-primary.png)

### Paso 4 - Insertar un conjunto de datos en el nodo primario

__Referees:__

```JavaScript
    qatar2022M.referees.insertMany([
        {
            name: "Abdulrahman Al-Jassim",
            country: "Catar",
            confederacy: "AFC",
            role: "Main",
        },
        {
            name: "Christopher Beath",
            country: "Australia",
            confederacy: "AFC",
            role: "Main",
        },
        {
            name: "Mohammadreza Abolfazli",
            country: "Irán",
            confederacy: "AFC",
            role: "Assistant",
        },
        {
            name: "Abdulla Al-Marri",
            country: "Catar",
            confederacy: "AFC",
            role: "VAR",
        },
        {
            name: "Iván Arcides Barton Cisneros",
            country: "El Salvador",
            confederacy: "Concacaf",
            role: "Main",
        }
    ])
```
__Teams:__

```JavaScript
    qatar2022M.teams.insertMany([
    {
        name: "Países Bajos",
        group: "Grupo A",
        confederacy: "UEFA",
        coach: {
            name: "Aloysius Paulus van Gaal",
            age: 72,
            country: "Países Bajos",
        },
        base_camps: {
            city: "Doha",
            base: "The St. Regis Doha",
            training_ground: "Universidad de Catar, complejo deportivo 6"
        },
        goals_scored: 10,
        goals_received: 4,
        ranking_FIFA: "8° lugar"
    },
    {
        name: "Inglaterra",
        group: "Grupo B",
        confederacy: "UEFA",
        coach: {
            name: "Gareth Southgatel",
            age: 53,
            country: "Inglaterra",
        },
        base_camps: {
            city: "Al Wakrah",
            base: "Hotel Souq Al Wakra Qatar",
            training_ground: "Estadio Saoud bin Abdulrahman"
        },
        goals_scored: 13,
        goals_received: 4,
        ranking_FIFA: "5° lugar"
    },
    {
        name: "Argentina",
        group: "Grupo C",
        confederacy: "Conmebol",
        coach: {
            name: "Lionel Sebastián Scaloni",
            age: 45,
            country: "Argentina",
        },
        base_camps: {
            city: "Doha",
            base: "Universidad de Catar, Hostel 1",
            training_ground: "Universidad de Catar, complejo deportivo 3"
        },
        goals_scored: 15,
        goals_received: 8,
        ranking_FIFA: "3° lugar"
    },
    {
        name: "Francia",
        group: "Grupo D",
        confederacy: "UEFA",
        coach: {
            name: "Didier Claude Deschamps",
            age: 55,
            country: "Francia",
        },
        base_camps: {
            city: "Rayán",
            base: "Al Messila, Luxury Collection Resort & Spa",
            training_ground: "Estadio Jassim bin Hamad"
        },
        goals_scored: 16,
        goals_received: 8,
        ranking_FIFA: "4° lugar"
    },
    {
        name: "Japón",
        group: "Grupo E",
        confederacy: "AFC",
        coach: {
            name: "Hajime Moriyasu",
            age: 55,
            country: "Japón",
        },
        base_camps: {
            city: "Doha",
            base: "Hotel Radisson Blu Doha",
            training_ground: "Estadio Jassim bin Hamad, instalación 1"
        },
        goals_scored: 5,
        goals_received: 4,
        ranking_FIFA: "24° lugar"
    },
    {
        name: "Marruecos",
        group: "Grupo F",
        confederacy: "CAF",
        coach: {
            name: "Walid Regragui",
            age: 48,
            country: "Marruecos",
        },
        base_camps: {
            city: "Doha",
            base: "Wyndham Doha West Bay",
            training_ground: "Estadio Abdullah bin Khalifa"
        },
        goals_scored: 6,
        goals_received: 5,
        ranking_FIFA: "22° lugar"
    },
    {
        name: "Brasil",
        group: "Grupo G",
        confederacy: "Conmebol",
        coach: {
            name: "Adenor Leonardo Bacchi",
            age: 62,
            country: "Brasil",
        },
        base_camps: {
            city: "Doha",
            base: "The Westin Doha Hotel y Spa",
            training_ground: "Estadio Grand Hamad"
        },
        goals_scored: 8,
        goals_received: 3,
        ranking_FIFA: "1° lugar"
    }])
```

__Stadiums:__

```JavaScript
    qatar2022M.stadiums.insertMany([
    {
        name: "Estadio Internacional Khalifa",
        location: {
            city: "Rayán",
            municipality: "Municipio de Rayán (Área de Doha)"
        },
        capacity: 45857,
        construction_year: "2014",
        dimensions: {
            long: 100,
            high: 65,
        }
    },
    {
        name: "Estadio Al Bayt",
        location: {
            city: "Jor",
            municipality: "Municipio de Jor"
        },
        capacity: 68895,
        construction_year: "2016",
        dimensions: {
            long: 372,
            high: 310,
        }
    }])
```

__Comprobamos inserciones en el nodo primario y lectura de datos:__

* __Referees:__
    ```JavaScript
        qatar2022M.referees.count()
        qatar2022M.referees.findOne()
    ```
    ![referees](/referees.png)
* __Teams:__
    ```JavaScript
        qatar2022M.teams.count()
        qatar2022M.find.findOne()
    ```
    ![teams](/teams.png)
* __Stadiums:__
    ```JavaScript
        qatar2022M.stadiums.count()
        qatar2022M.stadiums.findOne()
    ```
    ![teams](/stadiums.png)


### Paso 5 - Comprobación de la réplica sobre los nodos secundarios

Establecemos una nueva conexión con uno de los nodos secundarios

```JavaScript
    connS = new Mongo("DESKTOP-MMLHIU2:20001")
    qatar2022S = connS.getDB("qatar-2022")
```

Comprobamos que es un nodo secundario

```JavaScript
    qatar2022S.isMaster()
```

![qatar2022-node-secondary](/qatar2022-node-secondary.png)

Habilitamos permisos de lectura

```JavaScript
    connS.setSecondaryOk()
```

Ahora si podemos realizar consultas

```JavaScript
    qatar2022S.stadiums.count()
    qatar2022S.stadiums.find()
```

![qatar2022-node-secondary-find](/qatar2022-node-secondary-find.png)

### Paso 6 - Detener el nodo primario

Empezamos por parar el nodo primario, simulando una caída o perdida de conexión

*nueva conexión con localhost*
```JavaScript
    connMain = new Mongo("localhost:20000")
    mainDB = connMain.getDB("qatar-2022")
```

*verificamos que es el nodo primario*
```JavaScript
    mainDB.isMaster()
```

![qatar2022-node-main](/qatar2022-node-main.png)

*tumbamos el nodo*
```JavaScript
    mainDB.adminCommand({shutdown : 1});
```

### Paso 7 - Comprobación del nuevo nodo primario

Vamos a comprobar que el nodo secundario paso a ser el nodo primario luego de caída del antiguo.

*nueva conexión con localhost*
```JavaScript
    connNewMain = new Mongo("localhost:20001")
    newMainDB = connNewMain.getDB("qatar-2022")
```

*verificamos que es el nuevo nodo primario*
```JavaScript
    newMainDB.isMaster()
```
![qatar2022-new-node-main](/qatar2022-new-node-main.png)