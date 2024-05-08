# JDBC 프로그래밍

## JDBC 정의
`JDBC`란 자바를 이용한 데이터베이스 접속과 SQL 문장의 실행, 그리고 실행 결과로 얻어진 데이터의 핸들링을 제공하는 방법과 절차에 관한 규약을 말한다.
- 자바 프로그램내에서 SQL문을 실행하기 위한 `자바 API`
- `SQL`과 `프로그래밍 언어`의 통합 접근 중 한 형태
- `JAVA`는 표준 인터페이스인 `JDBC API`를 제공
- 데이터베이스 벤더, 또는 기타 써드파티에서는 `JDBC 인터페이스를 구현한 드라이버`를 제공한다.

<img width="734" alt="스크린샷 2024-05-08 09 59 15" src="https://github.com/hamsangjin/TIL/assets/103736614/b90db120-ec7a-4676-82ba-040fdcb09c57">

<br>
<br>

## 환경 구성
- JDK 설치
- JDBC 드라이버 설치
```java
// 의존성 추가 - Maven
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.31</version>
</dependency>

// 의존성 추가 - gradle
dependencies {
    implementation group: 'mysql', name: 'mysql-connector-java', version: '8.0.33'
}
```

<br>

## JDBC 프로그래밍 방법
1. `import java.sql.*;`
2. `DriverManager`를 이용해 `Connection` 객체를 생성
3. `PreparedStatement` 객체를 생성
4. `PreparedStatement`에 값을 바인딩
5. `SELECT문`일 경우 `ResultSet`을 이용하여 데이터를 읽어온다.
6. `Connection`, `PreparedStatement`, `ResultSet`을 모두 `close()`한다.
    - `Statement`, `PreparedStatement`, `CallableStatement`를 이용해 SQL을 실행할 수 있으며, `procedure`는 `CallableStatement`를 이용해 실행한다. 그 외에는 `PreparedStatement`로 실행하는 것이 좋다.

<img width="377" alt="스크린샷 2024-05-08 10 02 21" src="https://github.com/hamsangjin/TIL/assets/103736614/73ee3268-6d01-452e-9e6f-0a6e3e4c0ec4">

<br>

---

<br>

# JDBC 사용 

## DB접속 
- `IMPORT`
```java
import java.sql.*;
```

- `드라이버 로드`(8 버전부터 필요 X)
```java
Class.forName( "com.mysql.cj.jdbc.Driver" );
```

- `Connection 얻기`
```java
String dburl = "jdbc:mysql://localhost/dbName";
String username = "root";
String password = "1234";
Connection con =  DriverManager.getConnection (dburl, username, password);
```

<br>

## INSERT, UPDATE, DELETE
- emp테이블이 id, name, sal라는 3개의 칼럼을 가지고 있다고 가정하자.
```java
String sql = "insert into emp(id, name, sal) values(?, ?, ?)";
ps = conn.prepareStatement(sql);
ps.setInt(1, emp.getId());
ps.setString(2, emp.getName());
ps.setDouble(3, emp.getSal());
int updateCount = ps.executeUpdate();
```

<br>

## SELECT
```java
String sql = "select id, name from emp where sal > ?";
ps = conn.prepareStatement(sql);
ps.setDouble(1, 10.5);
rs = ps.executeQuery();
while(rs.next()){
    EmpDto emp = new EmpDto();
    emp.setId(rs.getInt("id"));
    emp.setName(rs.getString("name"));
    list.add(emp);
}
```
- `ResultSet`을 통해 데이터를 읽어오는 걸 알 수 있다.

<br>

## 트랜잭션 시작
- `auto commit` 끄기
```java
conn.setAutoCommit(false);
```
- `트랜잭션 시작`: SQL이 실행되면 자동 실행된다.
- `트랜잭션 종료`: commit or rollback
```java
conn.commit();
or
conn.rollback();
```

<br>

> 이렇게 JDBC를 사용하면서  발생되는 `예외에 대한 처리`도 해줘야한다.

<br>

---

<br>

# DAO/DTO

## DAO
`DAO`란 Data Access Object의 약자로, Database에 접근하는 역할을 하는 객체를 말한다.
- 프로젝트의 `서비스 모델`에 해당하는 부분과 `데이터베이스`를 **연결하는 역할**
- 데이터의 `CRUD 작업`을 시행하는 클래스. 즉, 데이터에 대한 `CRUD 기능을 전담`하는 오브젝트

<br>

## DTO
`DTO`란 Data Transfer Object의 약자로, 데이터를 전달하기 위한 객체를 말하며, 로직을 가지지 않는 순수한 데이터 객체이다.
- 여러 레이어(Layer)간 데이터를 주고 받을 때 사용할 수 있고 주로 `View`와 `Controller` 사이에서 활용
- 어떻게 구현하느냐에 따라 `가변 객체`로 활용할 수도 있고 `불변 객체`로 활용할 수도 있음
- D데이터 전달만을 위한 객체라는 게 핵심 정의이기 때문에 완전히 데이터 전달 용도로만 사용하는게 목적이라면, `getter/setter`만 필요하지, 이외의 비즈니스 로직은 굳이 있을 필요가 없다.

<br>

## DAO/DTO 활용
- `DeptDAO.java`
```java
package com.exam.jdbc;

import java.sql.*;
import java.util.List;

public class DeptDAO {

    // 입력
    public boolean addDept(DeptDTO deptDTO){
        Connection conn = null;
        PreparedStatement ps = null;

        boolean result = false;

        try{
            conn = DBUtil.getConnection();

            String sql = "insert into dept(deptno, dname, loc) values(?, ?, ?)";
            ps = conn.prepareStatement(sql);

            ps.setInt(1, deptDTO.getDeptno());
            ps.setString(2, deptDTO.getDname());
            ps.setString(3, deptDTO.getLoc());

            int count = ps.executeUpdate();
            System.out.println(count + "건 입력했습니다.");

            if(count > 0)   result = true;
        } catch (Exception e){
            e.printStackTrace();
        } finally {
            DBUtil.close(conn, ps);
        }
        return result;
    }
}
```

<br>

- `DeptDTO.java`
```java
public class DeptDTO {
    private int deptno;
    private String dname;
    private String loc;

    public int getDeptno() {
        return deptno;
    }

    public void setDeptno(int deptno) {
        this.deptno = deptno;
    }

    public String getDname() {
        return dname;
    }

    public void setDname(String dname) {
        this.dname = dname;
    }

    public String getLoc() {
        return loc;
    }

    public void setLoc(String loc) {
        this.loc = loc;
    }

    @Override
    public String toString() {
        return "DeptDTO{" +
                "deptno=" + deptno +
                ", dname='" + dname + '\'' +
                ", loc='" + loc + '\'' +
                '}';
    }
}
```

<br>

- `DBUtil`
```java
import java.sql.*;

public class DBUtil {
    // DB 접속 - 오버 로딩
    public static Connection getConnection() throws ClassNotFoundException, SQLException{
        Class.forName("com.mysql.cj.jdbc.Driver");
        String url = "jdbc:mysql://localhost:3306/examplesdb";
        String username = "sangjin";
        String password = "sangjin";

        Connection conn = DriverManager.getConnection(url, username, password);
        return conn;
    }

    public static Connection getConnection(String url, String username, String password) throws ClassNotFoundException, SQLException{
        Class.forName("com.mysql.cj.jdbc.Driver");
        Connection conn = DriverManager.getConnection(url, username, password);
        return conn;
    }


    // DB close - 오버 로딩
    public static void close(Connection conn, PreparedStatement ps, ResultSet rs){
        if(rs != null){
            try {
                rs.close();
            } catch (SQLException e) {
                throw new RuntimeException(e);
            }
        }
        close(conn, ps);
    }

    public static void close(Connection conn, PreparedStatement ps){
        if(ps != null){
            try {
                ps.close();
            } catch (SQLException e) {
                throw new RuntimeException(e);
            }
        }
        close(conn);
    }

    public static void close(Connection conn){
        if(conn != null){
            try {
                conn.close();
            } catch (SQLException e) {
                throw new RuntimeException(e);
            }
        }
    }
}
```

<br>

- `DeptDAORun.java`
```java
public class DeptDAORun {
    public static void main(String[] args) {
        DeptDAO deptDAO = new DeptDAO();
        DeptDTO deptDTO = new DeptDTO();

        deptDTO.setDeptno(80);
        deptDTO.setDname("like");
        deptDTO.setLoc("판교");

        boolean resultFlag = deptDAO.addDept(deptDTO);
        if(resultFlag)  System.out.println("입력성공!!");
        else  System.out.println("입력실패 ㅠㅠ");
    }
}
```
