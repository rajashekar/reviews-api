# Reviews API 
Supports operations for writing reviews and listing reviews for a product but with no sorting or filtering.

### Prerequisites
MySQL needs to be installed and configured. Instructions provided separately.

### Getting Started
* Configure the MySQL Datasource in application.properties.
* Add Flyway scripts in src/main/resources/db/migration.
* Define JPA Entities and relationships.
* Define Spring Data JPA Repositories.
* Add tests for JPA Repositories.

### Reference Documentation
For further reference, please consider the following sections:

* [Official Apache Maven documentation](https://maven.apache.org/guides/index.html)

### Guides
The following guides illustrate how to use some features concretely:

* [Accessing JPA Data with REST](https://spring.io/guides/gs/accessing-data-rest/)
* [Accessing Data with JPA](https://spring.io/guides/gs/accessing-data-jpa/)
* [Accessing data with MySQL](https://spring.io/guides/gs/accessing-data-mysql/)


Starting mysql server
```
mysql.server start
```

Creating user 
```
CREATE USER review@localhost IDENTIFIED BY 'password';
grant all privileges on *.* to review@localhost with grant option;
```

Connect to mysql user with above client
```
mysql -u review -p
```
on password prompt - enter `password`

Connecting to reviewsdb
```
use reviewsdb
```

```
curl -v -H 'Content-type: application/json' \
    localhost:8080/products/ \
    -d '{"name": "iPhone", "description": "Phone with i", "price": "1000"}'
```

mysql> select * from product;
+----+--------+--------------+-------+
| ID | NAME   | DESCRIPTION  | PRICE |
+----+--------+--------------+-------+
|  1 | iPhone | Phone with i |  1000 |
+----+--------+--------------+-------+
1 row in set (0.00 sec)

```
curl localhost:8080/products/1
{"id":1,"name":"iPhone","description":"Phone with i","price":1000}
```

```
curl localhost:8080/products/
[{"id":1,"name":"iPhone","description":"Phone with i","price":1000},{"id":2,"name":"Pixel","description":"Phone with x","price":1000}]
```


```
curl -v -H 'Content-type: application/json' localhost:8080/reviews/products/1 -d '{"username": "Tyler Durden", "rating": 3, "title": "its just a phone with i", "body": "i seems more egotistical"}'
{"id":1,"username":"Tyler Durden","rating":3,"title":"its just a phone with i","body":"i seems more egotistical","product":{"id":1,"name":"iPhone","description":"Phone with i","price":1000}}
```

```
mysql> select * from review;
+----+--------------+--------+-------------------------+--------------------------+------------+
| ID | USERNAME     | RATING | TITLE                   | BODY                     | PRODUCT_ID |
+----+--------------+--------+-------------------------+--------------------------+------------+
|  1 | Tyler Durden |      3 | its just a phone with i | i seems more egotistical |          1 |
+----+--------------+--------+-------------------------+--------------------------+------------+
```

```
curl -v -H 'Content-type: application/json' localhost:8080/reviews/products/1
[{"id":1,"username":"Tyler Durden","rating":3,"title":"its just a phone with i","body":"i seems more egotistical","product":{"id":1,"name":"iPhone","description":"Phone with i","price":1000}}]
```

```
curl -v -H 'Content-type: application/json' localhost:8080/comments/reviews/1 -d '{"username": "Neo", "body": "Tyler, Stop worrying about i, follow white rabbit"}'
{"id":1,"body":"Tyler, Stop worrying about i, follow white rabbit","username":"Neo","review":{"id":1,"username":"Tyler Durden","rating":3,"title":"its just a phone with i","body":"i seems more egotistical","product":{"id":1,"name":"iPhone","description":"Phone with i","price":1000}}}
```

```
mysql> select * from comment;
+----+---------------------------------------------------+----------+-----------+
| ID | BODY                                              | USERNAME | REVIEW_ID |
+----+---------------------------------------------------+----------+-----------+
|  1 | Tyler, Stop worrying about i, follow white rabbit | Neo      |         1 |
+----+---------------------------------------------------+----------+-----------+
```

```
curl -v -H 'Content-type: application/json' localhost:8080/comments/reviews/1
[{"id":1,"body":"Tyler, Stop worrying about i, follow white rabbit","username":"Neo","review":{"id":1,"username":"Tyler Durden","rating":3,"title":"its just a phone with i","body":"i seems more egotistical","product":{"id":1,"name":"iPhone","description":"Phone with i","price":1000}}}]
```