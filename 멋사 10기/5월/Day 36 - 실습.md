# 실습 문제 

## 문제 1: liondb 라는 이름으로 데이터베이스를 생성해 주세요.
```sql
create database liondb;
```

<br>

## 문제 2: user명 lion password는 like로 user를 생성해 주세요. 
```sql
CREATE USER 'lion'@'localhost' IDENTIFIED BY 'like';
```

<br>


## 문제 3: lion user가 liondb를 사용할 수 있도록 권한을 부여해 주세요.  
```sql
GRANT ALL PRIVILEGES ON liondb.* TO 'lion'@'localhost'; -- root로 실행
```

<br>

## 문제 4: user, user_role, role, board  테이블을 제약 조건들을 적절히 이용해서 생성해 주세요.  
```sql
create table user(
    user_id int Primary Key auto_increment,
    username varchar(10) not null,
    email varchar(30) unique not null,
    password varchar(30) not null,
    check (email like '%@%.%')
);

create table role(
    role_id int Primary Key auto_increment,
    role_name varchar(30) unique not null
);

create table user_role(
    user_id int, foreign key (user_id) references user(user_id) on delete cascade,
    role_id int, foreign key (role_id) references role(role_id) on delete cascade
);

CREATE TABLE board (
    board_id int primary key auto_increment,
    board_name varchar(255) not null,
    description text,
    author_id int, foreign key (author_id) references user(user_id) on delete cascade
);
```

<br>

## 문제 5: 각 테이블에 적절한 샘플 데이터를 입력해 주세요.  
```sql
insert into user(username, email, password)
values ('함상진', 'aaa@aaa.a', '222@2@'),
       ('홍길동', 'bbb@bbb.b', '333#3#'),
       ('아무개', 'ccc@ccc.c', '444$4$');

insert into role(role_name)
values ('관리자'), ('사용자'), ('매니저');

insert into user_role
values (1, 1), (2, 2), (3, 3);

insert into board(board_name, description, author_id)
values ('글 제목 - 1', '안녕하세요 글 내용입니다. 11111', 1),
       ('글 제목 - 2', '안녕하세요 글 내용입니다. 22222', 2),
       ('글 제목 - 3', '안녕하세요 글 내용입니다. 33333', 3);
```
