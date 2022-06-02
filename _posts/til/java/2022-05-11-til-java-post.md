---
layout: post
title: 예외 처리
description: >
  인프런 Do it! 자바 프로그래밍 입문 수강 중
sitemap: true
hide_last_modified: true
categories:
  - til
  - java
---

# 예외 처리

* toc
{:toc .large-only}

## 오류란?
- 컴파일 오류(compile error): 프로그램 코드 작성 중 발생하는 문법적 오류
- 실행 오류(runtime error): 실행 중인 프로그램이 의도 하지 않은 동작을 하거나(bug) 프로그램이 중지되는 오류
- 실행 오류 시 비정상 종료는 서비스 운영에 치명적
- 오류가 발생할 수 있는 경우 로그(log)를 남겨 추후 이를 분석하여 원인을 찾아야 함
- 자바는 예외 처리를 통하여 프로그램의 비정상 종료를 막고 로그를 남길 수 있음

__오류와 예외 클래스__
- 시스템 오류(error): 가상 머신에서 발생, 프로그래머가 처리할 수 없음  
ex) 동적 메모리가 없는 경우, 스택 오버 플로우 등
- 예외(Exception): 프로그램에서 제어 할 수 있는 오류  
ex) 읽어 들이려는 파일이 존재하는 경우, 네트워크 연결이 끊어진 경우 등

__예외 클래스의 종류__
- 모든 예외 클래스의 최상위 클래스는 `Exception`
- 다양한 예외 클래스가 제공 됨
- ex) `IOException`, `RuntumeEception`, `FileNotFoundException`, `SocketException`, `ArithmeticException`, `IndexOutofBoundsException`

## try-catch문

__IndexOutofBoundsException 예시__

```java
int[] arr = {1, 2, 3, 4, 5};
try {
for(int i = 0; i <= 5; i++) {
  System.out.println(arr[i]);
  }
}catch(ArrayIndexOutOfBoundsException e) {
  System.out.println(e);
  return;
}finally {
  System.out.println("finally");
}
```
`arr[5]`는 없기 때문에 Index 오류가 생기게 된다.
- `try`: 오류가 생기지 않을 때 동작
- `catch`: 해당 에러가 생겼을 때 동작
- `finally` 오류의 유무와 관계 없이 마지막에 동작 

__File input 예시__

```java
FileInputStream fis = null;
		
try {
  fis = new FileInputStream("a.txt"); //파일 불러오기
} catch (FileNotFoundException e) {
  System.out.println(e);  //불러올 파일이 없을 경우 예외 처리
}finally {
  try {
    fis.close();  //파일 닫기
  } catch (IOException e) { //닫을 파일이 없을 경우 예외 처리
    // TODO Auto-generated catch block
    e.printStackTrace();
  }
  System.out.println("finally");
}
System.out.println("end");
```

## try-with-resources문
- 리소스를 자동 해제하도록 제공해주는 구문
- 자바7부터 제공
- `clsoe()`를 명시적으로 호출하지 않아도 `try{}` 블록에서 열린 리소스는 정상적인 경우, 예외 발생한 경우 모두 자동 해제 됨
- 해당 리소스가 `AutoCloseable`을 구현해야 함
- `FileInputStream`의 경우 `AutoCloseable`을 구현하고 있음

예시
```java
try(FileInputStream fis = new FileInputStream("a.txt")) {
			
} catch (IOException e) {
  System.out.println(e);
}
System.out.println("end");
```
`close()`함수를 따로 불러와서 예외처리까지 하는 번거로움을 없애줄 수 있다.

__예외 처리 미루기__
- `throws`를 사용하여 예외처리 미루기
- 메서드 선언부에 `throws` 추가
- 예외가 발생한 메서드에서 예외 처리를 하지 않고 이 메서드를 호출한 곳에서 예외 처리를 한다는 의미
- `main()`에서 `throws`를 사용하면 가상머신에서 처리 됨

__사용자 정의 예외__
- JDK에서 제공되는 예외 클래스 외에 사용자가 필요에 의해 예외 클래스를 정의하여 사용
- 기존 JDK 예외 클래스 중 가장 유사한 클래스에서 상속
- 기본적으로 `Exception`에서 상속해도 됨

```java
public class IdFormatExeption extends Exception{
  public IDFormatException(String message){
    super(message);
  }
}
```
__예외 처리 직접 구현__
```java
public class IDFormatTest {
	private String userID;
	
	public String getUserID() {
		return userID;
	}

	public void setUserID(String userID) throws IDFormatException{  //예외 처리 정의
		if(userID == null) {
			throw new IDFormatException("아이디는 null일 수 없습니다.");
		}
		else if(userID.length() < 8 || userID.length() > 20) {
			throw new IDFormatException("아이디는 8자 이상 20자 이하로 쓰세요.");
		}
		this.userID = userID;
	}

	public static void main(String[] args) {
		IDFormatTest idTest = new IDFormatTest();
		String userID = null;
		
		try {
			idTest.setUserID(userID); //null에 대한 예외 처리 출력
		} catch (IDFormatException e) {
			System.out.println(e);
		}
	}
}
```

끝!