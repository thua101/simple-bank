## why ths readme file? 
This was a hard course to me, as I don't have much experience with Go, kubernetes, gRPC nor unit testing.
this file keeps me in track with the materials, keyword, and comments, frameworks that I learned through the course.

### Migrate
1. migrate folder -> up schema: sql schema queries that build up the database, generated from dbdiagram.io
2. migrate folder -> down schema: drop all the existing tables, write by developer.
3. migrate folder was generated by mirgate plugin, can be installed for shell or as a package for go


``` bash
Mirgrate
migrate create -ext sql -dir db/migrate -seq init_schema
-ext extension of the file - sql
-dir directory
-seq generate sequential version number

migrate -path db/migrate -database "postgresql://root:qwer1234@localhost:5432/simple_bank?sslmode=disable" -verbose up

migrate -path db/migrate -database "postgresql://root:qwer1234@localhost:5432/simple_bank?sslmode=disable" -verbose up

-verbose print the logs
up: mirgrate up
```
### Docker
1. create database using postgres:alphine12 image
2. interact using system command line starts with docker exec
3. or interact by image cli provided by postgresql image
``` bash
docker run --name postgres12 -p 5432:5432 -e POSTGRES_USER=root -e POSTGRES_PASSWORD=qwer1234 -d postgres:12-alpine

docker exec -it postgres12 /bin/sh
docker exec -it posgres12 psql -U root simple_bank
```

### Makefile
1. Make tool can be installed for system cli
2. shorten commands by define them under Makefile
3. call by make 'command'

### what are we trying to achieve so far?
mirage down and up database with one command -> simple/automate
create/drop database with one command -> automate
create database container with one command -> automate

easy for later scalibiliy and deployment.

### SQLC
1. converting schema to table enetity (read by need, if one table is selected by query, it will then look for schema that build this table under schema path, then convert it to a entity object)
2. db.go is a dependency object of the database object, it's capable of running queries that generated from account.db
3. account.db are queries in go converted from query sql file under query folder
4. sqlc generate is based on the defined sqlc.yaml file under the work directory

#### if not call function of the module directly, go formtter might remove it, keep by 

```go
_ "github.com/lib/pg"
go mod tidy -> remove the /indirect tag
```

### Go Testify
1. write test cases
2. single test case should be independent of each other
3. using math/rand, and MakeFile go test to automate the process
4. fastify can validate certain arguments

```bash
go test -v -cover ./...

```

### Transaction
1. create a transfer record with amount = 10
2. create an account entry for account1 with amount = -10
3. create entry for account 2
4. subtract 10 from account 1
5. add 10 to account 1

1. providate a reliable and consist unit of work even in case of system failure
2. provide isolation between programs access the database concurrently

#### ACID property
1. atomicity
2. consistency
3. isolation
4. durability


### Deadlock
1. Deadlock can be resulted from operations in the database that referencing the same column even without changing it
2. KEYWORD like FOR NO KEY UPDATE can prevent deadlocking occurs from same referencing
3. transaction should be wrapped
4. deadlock debugging should follow test-driven development (TDD) pattern or method
5. deadlock can also caused by concurrent interactive exchange/update of data, which can be prevented from logic

Consistency
- dirty read
- non-repeatable read
- phantom read
- serialization anomaly

4 standard isolation levels
- read uncommitted
- read committed
- repeatable read
- serializable

### Github Action
1. build up Go env
2. re-test on main change
3. using postgres container as service
4. adding Mirgrate package to github action
5. run test after run migrations

Takeaway? Github action is very good service deploy and CI tool that can seperates workflows into steps.


### Gin Get Request
1. server struct takes db object, gin engine.(pointers) This can be more efficient and reduce memory usage, especially when dealing with large data structures.
2. server calls methods, main argument of the methods are gin.Context and url arguments
3. var req createAccountRequest takes in the arguments
4. ShouldBindJSON(&req) ShouldBindUri(&req) ShouldBindQuery(&req) desctrue the arguments from request
5. http.<> indicates the server status


SERVER_ADDRESS=0.0.0.0:8081 make server

### Viper
1. basically, it's dotenv of javascript
2. it has features like live watching and remote variable changing


### mock database testing
1. independent tests
2. faster tests
3. 100% coverage
1. by using Fake DB: as Memory
2. DB stubs: GOMOCK

Gin and Viper added
go install github.com/golang/mock/mockgen@v1.6.0
go mock added

go get github.com/go-playground/validator/v10


### mock testing with mockgen
mockgen db/mock -> generate struct and functions for all the existing db entity testing

using TestCases struct array for testing one by one 

basically, mock create controller, store, server local controller and recorder for sending requests/receiving responses without actually hosting a server, it can fake database error or server error for actual controller to receive, therefore compare the actual response with the expected response from the real controller.

### migrate down/up 1
migrate one version up or down with migrate module.
Constraint is better than unique key that consists two elements

### validator
when controller can potentially receives object that contains more complicated column, (think of enum that have possibly 100 values), validate can modularize this checking process by defining function outside the data entity struct


### bcrypt hashing + JWT
nearly same procedure as I implemented with ExpressJS
Unit Testing is bit funky in verify the token for login, look it up for more detail

### Authentication + user priviliages
users should only have access to their own information
using middleware to narrowing use's information as part of the payload, and therefore the payload will be used as query parameter into the handler function.

### 23/63 it's production use capable now -> dockerize
new branch -> merge master after being reviewed and tested
```zsh
git checkout -b <newBranchName>

git push origin ft/docker
```

```Dockerfile
FROM base image
WORKDIR working directory inside the image
COPY .(copy everything from current folder) .(current working directory inside the image)
RUN go build -o main main.go (building our project into a binary file called main)
EXPOSE 8080 (inform docker that docker listens on this PORT)
CMD ["/app/main"]
```