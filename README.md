# PRUEBA
API REST para procesar transacciones bancarias usando Spring Boot, con persistencia 
en memoria con JPA y Hibernate usando base de datos H2, las transacciones se procesan 
de forma asincrona usando un threadpool.


## Especificaciones tecnicas
- Java 1.8
- Spring boot 1.4.1
- Maven 4.0
- H2 database 1.4
- JPA y Hibernate
- Log4j 2
- JUnit 4.12


## Configuracion:

-  Clonar repositorio.
```sh
$ https://github.com/Hlpenam/PruebaDav.git
```
-  Hacer el build con maven.
```sh
$ mvn clean package
```
-  Ejecutar con el tomcat embebido de spring boot.
```sh
$ mvn spring-boot:run
```
- La aplicacion queda disponible en: **http://localhost:8080**

## Descripcion
La aplicacion cuenta con 5 cuentas precargadas, y tiene disponibles los siguientes endpoint:

- **GET: /api/account**, retorna listado de cuentas en formato json 
- **GET: /api/account/{id}**, retorna el detalle de una cuenta en formato json
- **POST: /api/account**, recibe un json de cuenta sin **'id'** (id autogenerado)
- **PUT: /api/account**, recibe un json de cuenta a editar y retorna la cuenta modificada en formato json
- **DELETE: /api/account/{id}**, recibe el id de la cuenta a eliminar

ejemplo de cuenta en formato json:

``` json
  {
    "id": 100000,
    "nombre": "Leonardo",
    "identificacion": "1018427810
    "balance": 250000
  }
```
Para cargar y consultar transacciones se tienen disponibles los siguientes endpoint:

- **GET: /api/transaction**, retorna listado de las transacciones exitosas en formato json 
- **GET: /api/transaction/{id}**, retorna el detalle de una transaccion en formato json

Ejemplo de transaccion procesada:

```json
{
  "id": 1,
  "source": 100000,
  "target": 100004,
  "amount": 20000,
  "type": "TRANSFERENCIA"
  "tax": 11.100000000000001,
  "status": "EXITOSO"
}
```

- **POST: /api/transaction**, recibe un json de una transaccion que incluye: 
**id de la cuenta de origen, id de la cuenta destino y monto a transferir** Ejemplo:

``` json
  {
    "source": "100000",
    "target": "100004",
    "amount": 40000
  }
```

En la raiz del proyecto se genera un log de las transacciones en la raiz del proyecto en la ruta: **/logs/transactions.txt**

En el archivo **application.properties** se configuran las siguientes propiedades:

```sh
# Configuraciones del thread pool para procesar transacciones
threadpool.corepoolsize=10
threadpool.maxpoolsize=100

# Esta propiedad hace todos los log asincronos
Log4jContextSelector=org.apache.logging.log4j.core.async.AsyncLoggerContextSelector
```

