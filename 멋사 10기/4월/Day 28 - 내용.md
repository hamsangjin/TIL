# TCP를 이용한 간단한 채팅 프로그램

## 서버 구현
1. `ServerSocket 설정`: 서버는 ServerSocket 을 이용하여 특정 포트에서 클라이언트의 연결을 기다린다.
2. `클라이언트 연결 처리`: 클라이언트의 연결 요청이 들어오면 `accept()` 메서드를 통해 이를 수락하고, 각 클라이언트마다 별도의 스레드를 생성하여 독립적으로 처리한다.
3. `메시지 중계`: 서버는 클라이언트로부터 받은 메시지를 다른 클라이언트에게 전달(브로드캐스트)한다.

<br>

## 클라이언트 구현
1. `Socket을 이용한 서버 연결`: 클라이언트는 서버의 IP 주소와 포트 번호를 사용하여 서버에 연결한다.
2. `메시지 송수신`: 콘솔에서 입력받은 메시지를 서버에 보내고, 서버로부터 오는 메시지를 콘솔에 출력한다.\

<br>

## 서버 코드
```java
import java.io.*;
import java.net.*;
import java.util.*;

public class ChatServer {
    public static void main(String[] args) {
        // 1. 서버 소켓 설정 및 공유 객체 정의
        try (ServerSocket serverSocket = new ServerSocket(9999);) {
            System.out.println("서버가 준비되었습니다.");
            Map<String, PrintWriter> chatClients = new HashMap<>();

            while (true) {
                // 4. 어떤 클라이언트가 연결을 시도하면 수락
                Socket socket = serverSocket.accept();
                // 5. 쓰레드 호출
                new ChatThread(socket, chatClients).start();
            }
        } catch (Exception e) {
            System.out.println(e);
            e.printStackTrace();
        }
    }
}

class ChatThread extends Thread {
    private Socket socket;
    private String id;
    private Map<String, PrintWriter> chatClients;

    private BufferedReader in;
    PrintWriter out;

    // 6. 쓰레드가 호출되면 동시에 생성자 실행
    public ChatThread(Socket socket, Map<String, PrintWriter> chatClients) {
        this.socket = socket;
        this.chatClients = chatClients;

        try {
            // 8. 클라이언트에게 id를 입력받고, broadcast를 통해 현재 연결되어 있는 클라이언트에게 입장 알림을 보내주고, 서버에 새로운 id를 출력한다.
            out = new PrintWriter(socket.getOutputStream(), true);
            in = new BufferedReader(new InputStreamReader(socket.getInputStream()));
            id = in.readLine();
            broadcast(id + "님이 입장하셨습니다.");
            System.out.println("새로운 사용자의 아이디는 " + id + "입니다.");

            // 9. chatClients.put(this.id, out)를 실행할 때
            // synchronized를 이용해 chatClients에 다른 쓰레드가 접근할 수 없게 해준다.
            synchronized (chatClients) {
                chatClients.put(this.id, out);
            }
        } catch (Exception e) {
            System.out.println(e);
            e.printStackTrace();
        }
    }

    @Override
    public void run() {
        // 해당 클라이언트의 채팅이 시작됐다는 알림을 서버에게 전송하고,
        // 클라이언트가 /quit를 입력하기 전까지 입력하는 값들은 전부 모든 클라이언트에게 전송해준다.
        // 종료될 때까지 반복
        System.out.println(id + "사용자 채팅시작!!");
        String msg = null;
        try {
            while ((msg = in.readLine()) != null) {
                if ("/quit".equalsIgnoreCase(msg))  break;

//                if(msg.split(" ").length == 3 && "/to".equalsIgnoreCase(msg.split(" ")[0])){
                if("/to".equalsIgnoreCase(msg.split(" ")[0])){
                    sendMsg(id, msg.split(" ")[1], msg.split(" ")[2]);
                } else{
                    broadcast(id + " : " + msg);
                }
            }

        } catch (IOException e) {
            System.out.println(e);
            e.printStackTrace();
        // 해당 클라이언트가 /quit를 눌러 입력이 종료되면, 그 클라이언트를 공유객체에서 제거해주고,
        // 모든 클라이언트에게 해당 클라이언트가 나갔다는 알림을 전송한다.
        } finally {
            synchronized (chatClients) {
                chatClients.remove(id);
            }
            broadcast(id + "님이 채팅에서 나갔습니다.");

            if (in != null) {
                try {
                    in.close();
                } catch (IOException e) {
                    throw new RuntimeException(e);
                }
            }

            if (socket != null) {
                try {
                    socket.close();
                } catch (IOException e) {
                    throw new RuntimeException(e);
                }
            }
        }
    }


    public void broadcast(String msg) {
        // broadcast에서 chatClients에 접근하는 동안엔 chatClients에 클라이언트 추가/제거가 불가하게 설정
        synchronized (chatClients) {
            // chatClients의 value값들을 저장한 it 객체 생성 즉, 연결된 모든 클라이언트가 저장됨
            Iterator<PrintWriter> it = chatClients.values().iterator();

            while (it.hasNext()) {
                // 해당 클라이언트로 전송하는 out 객체 받아주기
                PrintWriter out = it.next();
                try {
                    // 해당 클라이언트에게 전송
                    out.println(msg);
                } catch (Exception e) {
                    // 뭔가 오류가 나면 해당 객체 삭제(chatClients에서도 삭제)
                    it.remove();
                    e.printStackTrace();
                }
            }
        }
    }

    public void sendMsg(String fromId, String toId, String msg) {
        synchronized (chatClients) {
            if(chatClients.containsKey(toId)){
                chatClients.get(toId).println(fromId + "님이 " + toId + "님에게 보낸 메시지: " + msg);
            } else{
                chatClients.get(fromId).println("해당 사용자가 존재하지 않습니다.");
            }
        }
    }
}
```

<br>

## 클라이언트 코드
```java
import java.io.*;
import java.net.Socket;

public class ChatClient {
    public static void main(String[] args) {
        // 2. id 입력하지 않았다면 클라이언트 연결하지 않고 프로세스 종료
        if (args.length != 1) {
            System.out.println("사용법 : java ChatClent id");
            System.exit(1);
        }

        // 3. 서버 소켓과 동일한 포트로 연결하고, 입력 출력 객체 생성
        try (Socket socket = new Socket("127.0.0.1", 9999);
             PrintWriter out = new PrintWriter(socket.getOutputStream(), true);
             BufferedReader in = new BufferedReader(new InputStreamReader(socket.getInputStream()));
             BufferedReader keyboard = new BufferedReader(new InputStreamReader(System.in));
        ) {

            // 7. 서버에게 id 전송
            out.println(args[0]);

            // 8. 쓰레드 시작
            new InputThread(socket, in).start();

            // 9. 클라이언트가 서버에게 입력값 전달(종료할 때까지 반복)
            String msg = null;
            while ((msg = keyboard.readLine()) != null) {
                out.println(msg);
                if ("/quit".equalsIgnoreCase(msg))
                    break;
            }
        } catch (Exception e) {
            System.out.println(e);
        }
    }
}

class InputThread extends Thread {
    private Socket socket;
    private BufferedReader in;

    InputThread(Socket socket, BufferedReader in) {
        this.socket = socket;
        this.in = in;
    }

    @Override
    public void run() {
        try {
            String msg = null;
            while ((msg = in.readLine()) != null) {
                System.out.println(msg);
            }
        } catch (Exception e) {
            System.out.println(e);
        } finally {
            if (in != null) {
                try {
                    in.close();
                } catch (IOException e) {
                    throw new RuntimeException(e);
                }
            }
        }
    }
}
```

<br>

# UDP
`UDP(User Datagram Protocol)`는 인터넷상에서 데이터를 전송하기 위한 다른 방식의 프로토콜이며, TCP와 달리 연결이 없는 프로토콜로, 데이터 전송 시 연결 설정 과정이 없다.

데이터는 `데이터그램`이라는 작은 패킷으로 전송되며, 독립적으로 처리된다.

<br>

## UDP의 특징
1. `비연결성`: UDP는 연결을 설정하지 않고, 데이터를 보내는 측에서 받는 측으로 직접 데이터를 전송한다.
2. `속도`: 연결 설정이 없기 때문에, TCP보다 전송 속도가 빠르다.
3. `비신뢰성`: 데이터의 도착을 보장하지 않으며, 데이터가 손상되거나 순서가 바뀌어 도착할 수 있다.

<br>

## UDP Echo 서버 구현
1. `DatagramSocket 객체 생성`: 서버는 DatagramSocket 객체를 생성하여 특정 포트에서 클라이언트의 데이터그램을 기다린다.
2. `데이터 수신 및 응답`: `DatagramPacket`을 사용하여 클라이언트로부터 데이터를 받고, 동일한 데이터를 클라이언트에게 다시 보낸다.
3. `소켓 종료`: 서버는 더 이상 데이터 송수신이 필요 없을 때 `DatagramSocket`을 닫아 자원을 해제한다.

- 코드
```java
import java.net.*;

public class UDPEchoServer {
    public static void main(String[] args) throws Exception {
        // 서버에서 사용할 DatagramSocket 생성 (특정 포트 지정)
        DatagramSocket socket = new DatagramSocket(8888);
        byte[] receiveData = new byte[1024];
        byte[] sendData;

        // 클라이언트로부터 데이터를 받기 위한 DatagramPacket 생성
        DatagramPacket receivePacket = new DatagramPacket(receiveData, receiveData.length);
        socket.receive(receivePacket);
        String message = new String(receivePacket.getData()).trim();

        // 클라이언트 정보
        InetAddress IPAddress = receivePacket.getAddress();
        int port = receivePacket.getPort();

        // 클라이언트에게 데이터 보내기
        sendData = message.getBytes();
        DatagramPacket sendPacket = new DatagramPacket(sendData, sendData.length, IPAddress, port);
        socket.send(sendPacket);

        // 소켓 종료
        socket.close();
    }
}
```

<br>

## UDP Echo 클라이언트 구현
1. `DatagramSocket 객체 생성`: 클라이언트는 `DatagramSocket` 객체를 생성하여 데이터를 전송할 준비를 합니다.
2. `데이터 전송 및 응답 수신`: 클라이언트는 `DatagramPacket`에 데이터를 담아 서버로보내고, 서버로부터 동일한 데이터를 받습니다.
3. `소켓 종료`: 데이터 송수신이 끝나면 클라이언트는 `DatagramSocket`을 닫습니다.

- 코드
```java
import java.io.*;
import java.net.*;

public class UDPEchoClient {
    public static void main(String[] args) throws Exception {
        // 클라이언트에서 사용할 DatagramSocket 생성
        try(DatagramSocket clientSocket = new DatagramSocket();
            BufferedReader in = new BufferedReader(new InputStreamReader(System.in));
        ){
            // 서버의 IP 주소 및 포트 설정
            InetAddress IPAddress = InetAddress.getByName("localhost");

            byte[] receiveData = new byte[1024];
            byte[] sendData;

            System.out.print("메시지 입력: ");
            String message = in.readLine();

            // 서버로 데이터 보내기
            sendData = message.getBytes();
            DatagramPacket sendPacket = new DatagramPacket(sendData, sendData.length, IPAddress, 8888);
            clientSocket.send(sendPacket);

            // 서버로부터 응답 받기
            DatagramPacket receivePacket = new DatagramPacket(receiveData, receiveData.length);
            clientSocket.receive(receivePacket);
            String receiveMessage = new String(receivePacket.getData()).trim();
            System.out.println("서버 출력: " + receiveMessage);

        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```
