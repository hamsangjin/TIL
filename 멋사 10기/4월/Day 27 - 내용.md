# 네트워크 프로그래밍 기본 개념

## 소켓 프로그래밍

`소켓`은 네트워크 상에서 데이터를 주고받기 위한 끝점을 의미한다.<br>
자바에서 소켓 프로그래밍을 사용하면 서버와 클라이언트 간의 연결을 설정하고 데이터를 전송할 수 있다.

네트워크 프로그래밍에서는 서버 소켓이 연결을 수신 대기하고, 클라이언트 소켓이 서버에 연결을 시도한다.

<br>

## 프로토콜
네트워크 통신에서는 여러 프로토콜이 사용되며, `프로토콜`은 데이터 전송 시 준수해야 할 규칙과 표준의 집합이다.

자바 네트워크 프로그래밍에서 주로 사용되는 프로토콜은 `TCP`와 `UDP`가 있다.

<br>

## 동기화와 스레드
네트워크 프로그래밍에서는 여러 클라이언트가 서버에 동시에 접속할 수 있으므로, `멀티스레딩`과 `동기화` 개념이 중요하다.

각 클라이언트 요청을 별도의 스레드에서 처리해 서버의 효율성을 높일 수 있다.

<br>

## 예외 처리
네트워크 프로그래밍은 예외적인 상황이 자주 발생하므로, 네트워크 지연, 연결 중단 등 다양한 예외 상황에 대비하여 예외 처리가 중요하다.

자바에서는 `try-catch` 블록을 사용해 예외를 처리할 수 있다.

<br>

---

<br>

# TCP

`TCP`는 인터넷상에서 데이터를 메시지 형태로 보내기 위해 사용되는 가장 일반적인 프로토콜 중 하나이며, TCP 통신의 이해는 자바 네트워크 프로그래밍의 핵심 부분이다.

<br>

## TCP의 특징
- `신뢰성 있는 데이터 전송`: TCP는 데이터가 목적지에 정확하고 안전하게 도착하도록 보장하며, 잘못된 데이터나 손실된 데이터는 재전송된다.
- `연결 지향적`: TCP 통신을 시작하기 전에 클라이언트와 서버 간에 연결이 설정되어야 하며, 이 연결은 통신이 끝날 때까지 유지된다.
- `순서 보장`: 데이터는 전송된 순서대로 도착하며, 만약 순서가 바뀌면 TCP는 원래의 순서대로 재정렬한다.

<br>

## TCP의 작동 방식
- `3-Way Handshake`: TCP 연결은 3단계 핸드셰이크 과정을 통해 이루어지며, 이 과정을 통해 양쪽 모두 데이터를 보내고 받을 준비가 되었음을 확인한다.
- `데이터 전송`: 연결이 설정되면 데이터가 세그먼트라는 작은 단위로 나누어져 전송된다.
- `연결 종료`: 데이터 전송이 완료되면 연결을 종료하는 과정이 진행된다.

<br>

## TCP와 자바
자바에서는 `java.net` 패키지의 `Socket` 클래스를 사용해 TCP 클라이언트를 구현한다.

서버 구현에서는 `ServerSocket` 클래스를 사용하고, 이 클래스는 클라이언트의 연결 요청을 기다리고, 연결이 수립되면 클라이언트와 통신할 수 있는 소켓을 제공한다.

<img width="443" alt="스크린샷 2024-04-22 10 36 45" src="https://github.com/hamsangjin/TIL/assets/103736614/38e153cf-1a1a-429a-aaa6-36e5c058e64d">

<br>

## TCP 프로그래밍의 기본 절차

1. `서버 생성`: `ServerSocket`을 생성하고 특정 포트에서 클라이언트의 연결을 기다린다.
2. `클라이언트 연결 요청`: 클라이언트는 `Socket`을 사용해 서버의 특정 포트로 연결을 요청한다.
3. `연결 수락 및 데이터 교환`: 서버는 클라이언트의 요청을 수락하고, `Socket`을 통해 데이터를 송수신한다.
4. `연결 종료`: 데이터 전송이 끝나면, 클라이언트와 서버 모두 연결을 종료한다.

<br>

## InetAddress
`InetAddress` 클래스는 자바에서 IP주소를 표현하고 관리하기 위해 사용되는 클래스이며, 객체를 직접 생성할 수 없는 대신, `InetAddress` 클래스의 여러 정적 메소드를 통해 객체를 얻어야한다.

`java.net` 패키지에 속해있으며, 인터넷 프로토콜 주소, 즉 IPv4와 IPv6주소를 추상화하여 제공한다.

### 사용 방법
1. `주소 조회`: 호스트 이름 또는 텍스트로 표현된 IP 주소에 대한 `InetAddress` 객체를 반환하는 메서드가 제공된다.
    - `getByName(String host)` : 주어진 호스트 이름 또는 IP 주소에 대한 `InetAddress` 객체를 반환한다.
    - `getAllByName(String host)` : 주어진 호스트 이름에 대한 모든 IP 주소를 `InetAddress` 배열로 반환한다.
2. `로컬 호스트 조회`: 로컬 컴퓨터의 IP 주소를 얻는 메서드다.
    - `getLocalHost()` : 시스템에서 구성된 호스트 이름과 IP 주소를 포함하는 `InetAddress` 객체를 반환한다.
3. `주소 정보 검색`: `InetAddress` 객체를 통해 다양한 네트워크 관련 정보를 조회할 수 있다.
    - `getHostName()` : InetAddress 객체가 나타내는 호스트의 이름을 반환한다.
    - `getHostAddress()` : 호스트의 원시 IP 주소를 문자열 형태로 반환한다.
    - `getAddress()` : 호스트의 IP 주소를 바이트 배열로 반환한다.
  
### 예제 코드
```java
import java.net.InetAddress;

public class InetAddressExample {
    public static void main(String[] args) {
        try {
            // 특정 호스트의 IP 주소 조회
            InetAddress address = InetAddress.getByName("www.google.com"); 
            System.out.println("Google's IP Address: " + address.getHostAddress());
            // 로컬 호스트의 IP 주소와 이름 조회
            InetAddress localHost = InetAddress.getLocalHost(); 
            System.out.println("Local Host Name: " + localHost.getHostName()); 
            System.out.println("Local IP Address: " + localHost.getHostAddress());
        } catch (Exception e) {
            e.printStackTrace();
        }
    } 
}
```

- `UnknownHostException`: 주로 호스트 이름을 IP 주소로 해석할 수 없을 때 발생

```java
import java.net.InetAddress;
import java.net.UnknownHostException;

public class NSLookupLocal {
    public static void main(String[] args) {
        InetAddress inetaddr = null;

        try {
            inetaddr = InetAddress.getLocalHost();
        } catch (UnknownHostException e) {
            e.printStackTrace();
        }

        System.out.println(inetaddr.getHostName());
        System.out.println(inetaddr.getHostAddress());
        System.out.println("byte[] 형식의 ip 주소 변환 결과:");

        byte[] ip = inetaddr.getAddress();
        for (int i = 0; i < ip.length; i++) {
            System.out.print(ip[i] & 0xFF); // 부호 없는 바이트로 출력
             if (i != ip.length - 1){
                 System.out.print(".");
             }
        }
    }
}
```

<br>

---

<br>

# Echo 프로그래밍(TCP)
TCP 기반의 Echo 프로그래밍은 클라이언트가 서버에 메시지를 보내면 서버가 동일한 메시지를 클라이언트에게 되돌려주는 간단한 네트워크 어플리케이션이다.

<br>

## Echo 서버 구현
1. `ServerSocket 객체 생성`: 특정 포트에서 클라이언트의 연결 요청을 기다리기 위한 `ServerSocket` 객체를 생성한다.
2. `클라이언트 연결 수락`: 클라이언트의 연결 요청이 들어오면, `accept()` 메서드를 통해 연결을 수락하고 `Socket` 객체를 생성한다.
3. `데이터 읽기 및 쓰기`: 클라이언트로부터 데이터를 읽기 위해 `InputStream`을, 데이터를 클라이언트에게 보내기 위해 `OutputStream`을 사용한다.
4. `연결 종료`: 클라이언트와의 통신이 끝나면, `Socket`을 닫아 연결을 종료한다.

<br>

## Echo 클라이언트 구현
1. `Socket 객체 생성`: 서버의 IP 주소와 포트 번호를 사용하여 `Socket` 객체를 생성하여 서버에 연결한다.
2. `데이터 전송 및 응답 수신`: 서버에 데이터를 보내기 위해 `OutputStream`을 사용하고, 서버로부터의 응답을 받기 위해 `InputStream`을 사용한다.
3. `연결 종료`: 데이터 송수신이 끝나면 `Socket`을 닫아 연결을 종료한다.

<br>

## Echo 프로그램의 특징
- `간단한 통신 원리`: 클라이언트가 보낸 메시지를 서버가 그대로 되돌려주는 단순한 구조다.
- `네트워크 기본 개념 이해`: 클라이언트와 서버 간의 기본적인 데이터 송수신 과정을 이해하는 데 도움이 된다.
- `디버깅 및 테스트 용도`: 네트워크 프로그래밍을 학습하고 테스트하는 초기 단계에 유용하다.

<br>

## Echo 서버 코드
```java
import java.io.*;
import java.net.*;

public class EchoServer {
    public static void main(String[] args) throws IOException {
        int portNumber = 12345; // 서버 포트 번호
        try (ServerSocket serverSocket = new ServerSocket(portNumber);
             Socket clientSocket = serverSocket.accept();
             PrintWriter out = new PrintWriter(clientSocket.getOutputStream(), true);
             BufferedReader in = new BufferedReader(new InputStreamReader(clientSocket.getInputStream()))) {
            String inputLine;
            while ((inputLine = in.readLine()) != null) {
                out.println(inputLine); // 클라이언트로부터 받은 메시지를 되돌려줌
            }
        } catch (IOException e) {
            System.out.println("Exception caught when trying to listen on port " + portNumber + " or listening for a connection");
            System.out.println(e.getMessage());
        }
    }
}
```

## Echo 클라이언트 코드
```java
import java.io.*;
import java.net.*;

public class EchoClient {
    public static void main(String[] args) throws IOException {
        String hostName = "127.0.0.1";  // 서버의 호스트 이름 또는 IP 주소
        int portNumber = 12345;         // 서버 포트 번호

        try (Socket echoSocket = new Socket(hostName, portNumber);
             PrintWriter out = new PrintWriter(echoSocket.getOutputStream(), true);
             BufferedReader in = new BufferedReader(new InputStreamReader(echoSocket.getInputStream()));
             BufferedReader stdIn = new BufferedReader(new InputStreamReader(System.in))) {
                String userInput;

                while ((userInput = stdIn.readLine()) != null) {
                    out.println(userInput); // 서버에 메시지 전송
                    System.out.println("echo: " + in.readLine()); // 서버로부터의 응답 출력
                }
        } catch (UnknownHostException e) {
            System.err.println("Don't know about host " + hostName);
            System.exit(1);
        } catch (IOException e) {
            System.err.println("Couldn't get I/O for the connection to " + hostName);
            System.exit(1);
        }
    }
}
```

## 여러개의 클라이언트를 받아들이는 EchoServer
```java
import java.io.*;
import java.net.*;

public class EchoThreadServer {
    public static void main(String[] args) {
        //1. ServerSocket 생성..  -- 1개만 있으면 됨.
        try (ServerSocket serverSocket = new ServerSocket(9999);){
            while (true) {
                Socket clientSock = serverSocket.accept();  //클라이언트 수만큼 반복!!
                //클라이언트마다 각자 실행 할 수 있도록 만들어야함!!  (쓰레드!!)
                EchoThread echoThread = new EchoThread(clientSock);
                echoThread.start();
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}

class EchoThread extends Thread {
    private Socket socket;

    EchoThread(Socket socket) {
        this.socket = socket;
    }

    @Override
    public void run() {
        System.out.println(socket.getInetAddress().getHostAddress() + "로 부터 연결되었습니다.");

        try (PrintWriter out = new PrintWriter(socket.getOutputStream(), true);
             BufferedReader in = new BufferedReader(new InputStreamReader(socket.getInputStream()));
        ){
            String line = null;
            while ((line = in.readLine()) != null) {
                System.out.println("클라이언트로 부터 받은 메시지 : " + line);
                out.println(line);
            }
        } catch (Exception e) {
            System.out.println(e);
        } finally {
            try {
                socket.close();
            } catch (IOException e) {
                throw new RuntimeException(e);
            }
        }
    }
}
```
