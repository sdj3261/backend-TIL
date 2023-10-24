# JDBC & JDBC Template

### 목차

* JDBC(Java Database Connectivity)
* Jdbc Template

### 강의 정리

JDBC(Java Dtabase Connectiviy)

Java에서 RDBMS를 사용할 수 있게 해주는 API.

각 DBMS에서 제공하는 벤더를 통해 구현체를 만들어 사용한다.

```
String url = "jdbc:postgresql://localhost:5432/postgres";
Properties properties = new Properties(); properties.put("user", "postgres"); properties.put("password", "password");
Connection connection = DriverManager.getConnection(url, properties);

Statement statement = connection.createStatement();

String query = "SELECT * FROM people";

ResultSet resultSet = statement.executeQuery(query);

while (resultSet.next()) {
	String name = resultSet.getString("name");
	
	System.out.println(name);
}
```

OR 1=1&#x20;

으로 SQL Injection 공격을 할 수 있다.

이를 해결하기 위해 PreparedStatement가 등장하였다.

PreparedStatement는 SQL 쿼리에서 데이터와 명령을 명확하게 구분하기 때문에 SQL Injection 공격을 방지하는 효과적인 수단 중 하나 이다.

```
String sql = "SELECT name FROM people WHERE name LIKE ?";

jdbcTemplate.query(sql, resultSet -> {
	String name = resultSet.getString("name");

	System.out.println(name);
}, "%우");
```

