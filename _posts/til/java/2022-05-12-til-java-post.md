---
layout: post
title: 자바 입출력
description: >
  인프런 Do it! 자바 프로그래밍 입문 수강 중
sitemap: true
hide_last_modified: true
categories:
  - til
  - java
---

## 자바 입출력

* toc
{:toc .large-only}

## 스트림이란?
>네트워크에서 자료의 흐름이 물과 같다는 의미에서 유래되었다. 다양한 입출력 장치에 독립적으로 일관성 있는 입출력을 제공하는 방식을 말한다.

입출력이 구현되는 곳: 파일 디스크, 키보드, 마우스, 메모리 네트워크 등

__스트림 구분__
- 대상 기준  
입력 스트림 / 출력 스트림
- 자료의 종류  
바이트 스트림 / 문자 스트림
- 기능  
기반 스트림 / 보조 스트림

## 입력 스트림 / 출력 스트림
- 입력 스트림: 대상으로부터 자료를 읽어 들이는 스트림
- 출력 스트림: 대상으로 자료를 출력하는 스트림
- 스트림 종류

| 종류 | 예시 |
| -- | -- |
| 입력 스트림 | `FileInputStream`, `FileReader`, `BufferedInputStream`, `BufferedReader` 등 |
| 출력 스트림 | `FileOutputStream`, `FileWriter`, `BufferedOutputStream`, `BufferedWriter` 등 |

### 표준 입출력
- `System` 클래스의 표준 입출력 멤버

```java
public class System{
  public static PrintStream out;
  public static InputStream in;
  public static PrintStream err;
}
```

- `System.out`
  - 표준 출력(모니터) 스트림
  - `System.out.println("에러메시지");`
- `System.in`
  - 표준 입력(키보드) 스트림
  - `int d = System.in.read();` //한 바이트 읽어내기

  ```java
  System.out.println("알파벡 여러개를 쓰고 엔터키를 누르세요");
  int i = 0;  //초기화
  try { //입력값이 없을 수도 있기 때문에 예외 처리
    while((i = System.in.read()) != '\n') { //엔터키를 누를때까지 입력받음
      System.out.println(i);
    }
  } catch (IOException e) {
    // TODO Auto-generated catch block
    e.printStackTrace();
  }
  ```
- `System.err`
  - 표준 에러 출력(모니터) 스트림
  - `System.err.println("데이터");`

### Scanner 클래스
- `java.util` 패키지에 있는 입력 클래스
- 문자뿐만 아니라 정수, 실수 등 다른 자료형도 읽을 수 있음
- 여러 대상에서 자료를 읽을 수 있음 (콘솔, 파일 등)
- 생성자

| 생성자 | 설명 |
| -- | -- |
| `Scanner(File source)` | 파일을 매개변수로 받아 `Scanner` 생성 |
| `Scanner(Input)` | 바이트 스트림을 매개변수로 받아 `Scanner` 생성 |
| `Scanner(String source)` | `String`을 매개변수로 받아 `Scanner` 생성 |

- 메서드

| 메서드 | 설명 |
| --- | --- |
| `boolean nextBoolean()` | `boolean` 자료형을 읽음 |
| `byte nextByte()` | 한 바이트 자료형을 읽음 |
| `short nextShort()` | `Short` 자료형을 읽음 |
| `int nextInt()` | `int` 자료형을 읽음 |
| `long nextLong()` | `long` 자료형을 읽음 |
| `float nextFloat()` | `float` 자료형을 읽음 |
| `double nextDouble()` | `double` 자료형을 읽음 |
| `String nextLine()` | 문자열 `String`을 읽음 |

__예시__
```java
Scanner scanner = new Scanner(System.in);
		
String name = scanner.nextLine();
int num = scanner.nextInt();

System.out.println(name);
System.out.println(num);
```

__Console 클래스__
- `System.in`을 사용하지 않고 콘솔에서 표준 입력 가능
- 이클립스와는 연동되지 않음
- command 창에서 입력
- 메서드

| 메서드 | 설명 |
| --- | --- |
| `String readLine()` | 문자열을 읽음 |
| `char[] readPassword()` | 사용자에게 문자열을 보여주지 않고 읽음 |
| `Reader reader()` | `Reader` 클래스 반환 |
| `PrintWriter writer()` | `PrintWriter` 클래스 반환 |




