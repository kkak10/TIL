# Sequelize

## 개요
개인 프로잭트인 "Showing"을 개발중 Node.js에서 사용할 ORM중 "Sequelize"를 사용하기 위해 정리한 문서

## ORM이란?
ORM(Object Relation Mapper)은 객체지향 Object와 Database의 Table이 1:1 매칭되는 형태의 개념을 말한다.
해당 Object는 Table 및 Query를 추상화하고 있어서 소스상에서 Class는 Table과 매칭되고 Instance는 하나의 row와 매칭되는 형태이다.

## Getting Started

```javascript
import Sequelize from 'sequelize'

const sequelize = new Sequelize('database', 'username', 'password', {
                host: 'localhost',
                dialect: 'mysql'|'mariadb'|'sqlite'|'postgres'|'mssql',

                pool: {
                  max: 5,
                  min: 0,
                  idle: 10000
                },

                define: {
                  freezeTableName: true,
                  timestamps: false,
                }
              })
```


## Q&A
 ### 테이블 이름이 자동으로 복수형이 되는 문제.
 Sequelize는 기본적으로 TableName을 복수형으로 만들어서 Query를 날리는데 이는 설정으로 해결할 수 있다.

 DB Connection 설정을 해줄때
 ```javascript
 define: {
   freezeTableName: true,
 }
 ```
 위 처럼 설정해주면 모든 TableName을 복수형이 아닌 Model을 설정할때 이름 그대로 사용하게 해준다.

 ### Select 쿼리를 날릴때 자동으로 CreatedAt, UpdatedAt을 날리는 문제.
 Sequelize는 Select할때 자동으로 CreatedAt, UpdatedAt을 날리는데 이는 일반적으로 ORM에서 timestamp를 찍어주는 기능때문에 자동으로 CreatedAt, UpdatedAt가 날라간다. 해당 부분도 설정으로 Off할 수 있다.
 ```javascript
 define: {
   timestamps: true,
 }
 ```
위 처럼 설정하면 자동으로 CreatedAt, UpdatedAt를 조회하거나 생성하는것을 막을 수 있다.
