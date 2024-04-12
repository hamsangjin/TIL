# 실습 문제: 은행 어플리케이션 구현

## 목표
자바를 이용하여 간단한 은행 어플리케이션을 구현합니다. 

이 어플리케이션은 기본적인 객체 지향 프로그래밍과 예외 처리 기법을 활용하여, 은행 시스템의 핵심 기능을 모델링합니다.

<br>

## 요구 사항
1. **기본 객체**
    - **은행 객체(Bank)**
      - 속성: 은행 이름, 은행에 등록된 통장 목록
      - 기능: 통장 개설, 특정 통장 정보 조회
    - **통장 객체(Account)**
      - 속성: 계좌 번호, 예금주 이름, 잔액
      - 기능: 입금, 출금
    - **은행원 객체(Banker)**
      - 속성: 이름, 은행원 ID
      - 기능: 통장 개설 승인, 출금 승인

2. **사용자 정의 예외**
    - **잔액 부족 예외(InsufficientFundsException)**
      - 출금 금액이 잔액을 초과할 때 발생
    - **통장 미존재 예외(AccountNotFoundException)**
      - 요청한 계좌 번호의 통장이 존재하지 않을 때 발생

3. **기능 구현**
    - 은행 객체는 통장 객체를 리스트로 관리하며, 통장 개설 및 통장 정보 조회 기능을 제공해야 합니다.
    - 통장 객체는 입금과 출금 기능을 가지며, 출금 시 잔액 부족 예외를 발생시킬 수 있어야 합니다.
    - 은행원 객체는 통장 개설 및 출금 승인 프로세스를 관리합니다.
    - 모든 예외 상황에서 적절한 예외 처리를 수행해야 합니다.
  
<br>

## 코드

### Bank 클래스
```java
import java.util.ArrayList;

public class Bank {
    private String bankName;
    private ArrayList<Account> AccountList = new ArrayList<>();
    private int i = 0;

    public Bank(String bankName) {
        this.bankName = bankName;
    }

    public void addAccount(Account account) throws DuplicateAccountNumberException {
        for(Account a : AccountList){
            if(a.getAccountNumber() == account.getAccountNumber()){
                throw new DuplicateAccountNumberException("해당 계좌번호의 계좌가 존재합니다.");
            }
        }
        System.out.println("계좌 생성이 완료되었습니다.");
        AccountList.add(account);
    }

    public void infoCheck(int accountNumber, int password) throws AccountNotFoundException, WrongPasswordException {
        for(Account a : AccountList){
            if(a.getAccountNumber() == accountNumber && a.getPassword() == password){
                System.out.println("현재 잔액은 " + a.getBalance() + "원입니다.");
                return;
            } else if(a.getAccountNumber() == accountNumber && a.getPassword() != password){
                throw new WrongPasswordException("비밀번호가 틀렸습니다.");
            }
        }
        throw new AccountNotFoundException("요청한 계좌 번호의 통장이 존재하지 않습니다.");
    }
}
```

### Account 클래스
```java
public class Account {
    private int accountNumber;
    private String name;
    private int balance;
    private int password;

    public int getPassword() {
        return password;
    }

    public int getAccountNumber() {
        return accountNumber;
    }

    public String getName() {
        return name;
    }

    public int getBalance() {
        return balance;
    }

    public Account(int accountNumber, String name, int balance, int password) {
        this.accountNumber = accountNumber;
        this.name = name;
        this.balance = balance;
        this.password = password;
    }

    public void deposit(Account account, int amount){
        account.balance += amount;
        System.out.println("입금이 완료되었으며, 현재 잔액은 " + account.balance + "원 입니다.");
    }

    public void withdraw(int amount, int password1, int password2) throws InsufficientFundsException, WrongPasswordException {
        if (password1 == password2) {
            if (this.balance >= amount) {
                this.balance -= amount;
                System.out.println(amount + "원을 출금했으며 남은 잔액은 " + this.balance + "원 입니다.");
            } else throw new InsufficientFundsException("잔액이 부족합니다.");
        } else {
            throw new WrongPasswordException("비밀번호가 틀렸습니다.");
        }
    }
}
```

### Banker 클래스
```java
public class Banker {
    private final String name;
    private final int id;
    private Bank bank;

    public Banker(String name, int id, Bank bank) {
        this.name = name;
        this.id = id;
        this.bank = bank;
    }

    public void addAccountOK(Account account){
        try{
            bank.addAccount(account);
        } catch (DuplicateAccountNumberException e){
            System.out.println(e.getMessage());
        }
    }

    public void withdrawOK(Account account, int amount, int password){
        try {
            account.withdraw(amount, account.getPassword(), password);
        } catch (InsufficientFundsException e){
            System.out.println(e.getMessage());
        } catch (WrongPasswordException e){
            System.out.println(e.getMessage());
        }
    }

    public void depositOK(Account account, int amount){
        account.deposit(account, amount);
    }

    public void infoCheckOK(int accountNumber, int password){
        try {
            bank.infoCheck(accountNumber, password);
        } catch (AccountNotFoundException e){
            System.out.println(e.getMessage());
        } catch (WrongPasswordException e){
            System.out.println(e.getMessage());
        }
    }
}
```

### InsufficientFundsException 클래스
```java
public class InsufficientFundsException extends Exception{
    public InsufficientFundsException(String message) {
        super(message);
    }
}
```

### AccountNotFoundException 클래스
```java
public class AccountNotFoundException extends Exception{
    public AccountNotFoundException(String message) {
        super(message);
    }
}
```
### DuplicateAccountNumberException 클래스
```java
public class DuplicateAccountNumberException extends Exception{
    public DuplicateAccountNumberException(String message) {
        super(message);
    }
}
```
### WrongPasswordException 클래스
```java
public class WrongPasswordException extends Exception{
    public WrongPasswordException(String message) {
        super(message);
    }
}
```
### BankExam 클래스
```java
public class BankExam {
    public static void main(String[] args) {

        Bank woori = new Bank("Woori");

        Account acc1 = new Account(1234567, "홍길동", 30000, 1234);
        Account acc2 = new Account(2345678, "아무개", 70000, 5678);
        Account acc3 = new Account(2345678, "함상진", 45000, 4321);

        Banker banker1 = new Banker("은행원1", 1, woori);

        banker1.addAccountOK(acc1);
        banker1.addAccountOK(acc2);
        banker1.addAccountOK(acc3);

        System.out.println();

        banker1.withdrawOK(acc1, 30000, 1234);
        banker1.withdrawOK(acc1, 30000, 1234);

        System.out.println();

        banker1.infoCheckOK(1234567, 1234);
        banker1.depositOK(acc1, 20000);
        banker1.infoCheckOK(1234567, 1234);

        banker1.infoCheckOK(1234567, 5678);

        banker1.infoCheckOK(1234568, 1234);
    }
}
```
