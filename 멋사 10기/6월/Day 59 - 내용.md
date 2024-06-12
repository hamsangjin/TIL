# AWS SSM(AWS Systems Manager)
`AWS SSM`은 AWS 클라우드 환경 및 온프레미스 환경에서 시스템 관리를 자동화하고, 제어하며, 효율적으로 운영할 수 있도록 도와주는 서비스다. 

## AWS Parameter Store
`AWS Systems Manager`의 `Parameter Store`는 애플리케이션 구성 데이터를 중앙에서 관리 하고 보호할 수 있는 완전 관리형 서비스다. 

Parameter Store를 사용하면 중요한 구성 데이터와 암호를 텍스트 문자열로 저장하고 관리할 수 있으며, 이를 통해 애플리케이션의 보안을 강화하고 관리의 편의성을 높일 수 있다.

<br>

## Parameter Store 사용 방법
1. 매개 변수 생성: AWS Management Console, AWS CLI 또는 AWS SDK를 사용하여 매개 변수를 생성 할 수 있다.
```bash
aws ssm put-parameter --name "MyParameter" --value "MyValue" --type String
```

2. 매개 변수 조회: 저장된 매개 변수를 검색하고 값을 조회할 수 있다.
```bash
aws ssm get-parameter --name "MyParameter"
```

3. 매개 변수 업데이트: 기존 매개 변수의 값을 업데이트할 수 있다.
```bash
aws ssm put-parameter --name "MyParameter" --value "NewValue" --overwrite
```

4. 매개 변수 삭제: 더 이상 필요하지 않은 매개 변수를 삭제할 수 있습니다.
```bash  
aws ssm delete-parameter --name "MyParameter"
```

### ValidationException
AWS Parameter Store에 파라미터를 저장할 때 `ValidationException` 오류가 발생하며 `"Parameter name must be a fully qualified name"`이라는 메시지가 표시되는 경우, 파라미터 이름이 올바르지 않기 때문이다. 

AWS Parameter Store에서 파라미터 이름은 반드시 슬래시(/)로 시작해야 하며, 계층적 구조를 반영해야 한다.

**파라미터 이름 규칙**
  - `슬래시(/)로 시작`: 모든 파라미터 이름은 슬래시로 시작해야 한다.
  - `계층 구조 사용`: 파라미터 이름은 계층 구조를 반영할 수 있도록 슬래시를 사용하여 구분한다.
  - `유효한 문자 사용`: 파라미터 이름에는 알파벳 대소문자, 숫자, 하이픈(-), 언더스코어(_), 슬래시(/)만 사용할 수 있다.
  - `최대 길이 제한`: 파라미터 이름은 최대 1011자까지 가능하다.
  - `이름 제한`: 파라미터 이름은 AWS로 시작할 수 없다.

**올바른 파라미터 이름의 예**
  - `/myapplication/config/dbpassword`
  - `/myapplication/config/apikey`
  - `/project/environment/variable`

**CLI를 사용한 예**
```bash
aws ssm put-parameter --name "/myapplication/config/dbpassword" --value "mypassword" --type SecureString
```

**ValidationException 오류 해결 예**
만약 아래와 같이 잘못된 파라미터 이름을 사용했다면 오류가 발생한다.
```bash
aws ssm put-parameter --name "dbpassword" --value "mypassword" --type SecureString
```
이를 올바르게 수정하면 아래와 같다.
```bash
aws ssm put-parameter --name "/myapplication/config/dbpassword" --value "mypassword" --type SecureString
```

<br>

---

<br>

# Parameter Store로부터 설정을 읽어들여 SpringBoot App실행하기

## build.gradle
```
dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
    implementation 'org.springframework.boot:spring-boot-starter-web'
    implementation platform("io.awspring.cloud:spring-cloud-aws-dependencies:3.1.1")    // Spring Cloud AWS
    implementation 'io.awspring.cloud:spring-cloud-aws-starter-parameter-store:3.1.1'   // AWS Parameter Store
    compileOnly 'org.projectlombok:lombok'
    runtimeOnly 'com.mysql:mysql-connector-j'
    annotationProcessor 'org.projectlombok:lombok'
    testImplementation 'org.springframework.boot:spring-boot-starter-test'
    testRuntimeOnly 'org.junit.platform:junit-platform-launcher'
}
```

<br>

## application.yml
```yaml
spring:
  datasource:
    url: jdbc:mysql://my-mysql-db.cj4g8uow49kq.ap-northeast-2.rds.amazonaws.com:3306/mydatabase
    username: ${DB_USER}
    password: ${DB_PASSWORD}
    driver-class-name: com.mysql.cj.jdbc.Driver
  jpa:
    hibernate:
      ddl-auto: update
    show-sql: true
    properties:
      hibernate:
        dialect: org.hibernate.dialect.MySQLDialect
  cloud:
    aws:
      region:
        static: ap-northeast-2           // aws 리전 설정
  config:
    import:
      - aws-parameterstore:/apirdsdemo/  // 해당 경로에서 추가 설정을 가져온다.
```

<br>

## ApirdsdemoApplication.java
```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.*;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import software.amazon.awssdk.services.ssm.SsmClient;
import software.amazon.awssdk.services.ssm.model.*;

@SpringBootApplication
public class ApirdsdemoApplication implements CommandLineRunner {
    // ssmClient는 자동으로 Bean으로 설정된다.
    @Autowired
    SsmClient ssmClient;

    public static void main(String[] args) {
        SpringApplication.run(ApirdsdemoApplication.class, args);
    }

    // ParameterStore 의 값을 읽어들여 출력하기
    @Override
    public void run(String... args) throws Exception {
        GetParameterRequest request = GetParameterRequest.builder()       // 특정 매개변수 요청
                .name("/apirdsdemo/DB_USER")  // 파라미터 이름
                .withDecryption(true)         // 암호화된 값이 있는 경우 복호화
                .build();
        GetParameterResponse response = ssmClient.getParameter(request);  // 위에서 요청한 파라미터의 값을 가져옴
        System.out.println(response.parameter().value());
    }
}
```
