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

## 바이트 단위 스트림 / 문자단위 스트림
- 바이트 단위 스트림: 동영상, 음악 파일 등을 읽고 쓸 때 사용
- 문자 단위 스트림: 바이트 단위로 자료를 처리하면 문자는 깨지기 때문에 <span style='background-color: #f5f0ff'>2바이트</span> 단위로 처리하도록 구현된 스트림


### 바이트 단위 스트림 - InputStream
- 바이트 단위 입력용 최상위 스트림
- 추상 메서드를 포함한 추상 클래스로 하위 클래스가 구현하여 사용
- 주요 하위 클래스

| 스트림 클래스 | 설명 |
| --- | --- |
| `FileInputStream` | 파일에서 바이트 단위로 자료를 읽음 |
| `ByteArrayInputStream` | `Byte` 배열 메모리에서 바이트 단위로 자료를 읽음 |
| `FilterInputStream` | 기반 스트림에서 자료를 읽을 때 추가 기능을 제공하는 보조 스트림의 상위 클래스 |

- 메서드

| 메서드 | 설명 |
| --- | --- |
| `int read()` | 입력 스트림으로부터 한 바이트의 자료를 읽고 읽은 자료의 바이트 수 반환 |
| `int read(byte b[])` | 입력 스트림으로부터 `b[]` 크기의 자료를 `b[]`에 읽고 읽은 자료의 바이트 수 반환 |
| `int read(byte b[], int off, int len)` | 입력 스트림으로부터 `b[]`의 `off` 변수 위치부터 저장하며 `len`만큼 읽고 읽은 자료의 바이트 수 반환 |
| `void close()` | 입력 스트림과 연결된 대상 리소스를 닫음 (예: `FileInputStream`인 경우 파일 닫음) |

__예시__
```java
FileInputStream fis = null;
int i = 0;
try {
  fis = new FileInputStream("input.txt"); //파일 읽기 - 바이트 단위로 읽어서 한글은 깨짐
  while((i = fis.read()) != -1) { //읽을게 없을 때까지
    System.out.print((char)i);
  }
}catch(IOException e) {
  System.out.println(e);
}finally {
  try {
    fis.close(); //파일 닫기
  } catch (IOException e) {
    System.out.println(e);
  }catch(NullPointerException e) {
    System.out.println(e);
  }
  System.out.println("end");
}
```

__예시__
```java
try(FileInputStream fis = new FileInputStream("input2.txt")){
  byte[] bs = new byte[10];
  int i;
  while((i = fis.read(bs)) != -1) {
    for(int j = 0; j < i; j++) {  //버퍼에 남는 것 처리
      System.out.print((char)bs[j]);
    }
    System.out.println();
  }
}catch(IOException e) {
  System.out.println(e);
}
```

### 바이트 단위 스트림 - OutputStream
- 바이트 단위 출력용 최상위 스트림
- 추상 메서드를 포함한 추상 클래스로 하위 클래스가 구현하여 사용
- 주요 하위 클래스

| 스트림 클래스 | 설명 |
| --- | --- |
| `FileOutputStream` | 바이트 단위로 파일에 자료를 씀 |
| `ByteArrayOutputStream` | `Byte` 배열에 바이트 단위로 자료를 씀 |
| `FilterOutputStream` | 기반 스트림에서 자료를 쓸 때 추가 기능을 제공하는 보조 스트림의 상위 클래스 |

- 메서드

| 메서드 | 설명 |
| --- | --- |
| `void write(int b)` | 한 바이트 출력 |
| `void write(byte[] b)` | `b[]` 배열에 있는 자료 출력 |
| `void write(byte b[], int off, int len)` | `b[]` 배열에 있는 자료의 `off` 위치로부터 `len` 개수만큼 자료 출력 |
| `void flush()` | 출력을 위해 잠시 자료가 머무르는 출력 버퍼를 강제로 비워 자료 출력 |
| `void close()` | 출력 스트림과 연결된 대상 리소스를 닫고 출력 버퍼가 비워짐 (예: `FileOutputStream`인 경우 파일 닫음) |

__예시__
```java
byte[] bs = new byte[26];
byte data = 65;
for(int i = 0; i < bs.length; i++) {
  bs[i] = data++;
}

try(FileOutputStream fos = new FileOutputStream("output.txt", true)){	//true면 이어서 쓰기
  fos.write(bs);
}catch(IOException e) {
  System.out.println(e);
}
System.out.println("end");
```

```java
fos.write(bs, 2, 10); //`C`부터 10개 출력
```

__flush()와 close() 메서드__
- 출력 버퍼를 비울 때 `flush()` 사용
- `close()` 메서드 내부에서 `flush()`가 호출되기 때문에 `close()` 메서드 호출되면 자동으로 출력 버퍼가 비워 짐

### 문자 단위 스트림 - Reader
- 문자 단위로 읽는 최상위 스트림
- 하위 클래스에서 상속 받아 구현

| 스트림 클래스 | 설명 |
| --- | --- |
| `FileReader` | 파일에서 문자 단위로 읽는 스트림 클래스 |
| `InputStreamReader` | 바이트 단위로 읽은 자료를 문자로 변환해 주는 보조 스트림 클래스 |
| `BufferedReader` | 문자로 읽을 때 배열을 제공하여 한꺼번에 읽을 수 있는 기능을 제공해 주는 보조 스트림 |

- 주요 메서드

| 메서드 | 설명 |
| --- | --- |
| `int read()` | 파일로부터 한 문자를 읽고 읽은 값 반환 |
| `int read(char[] buf)` | 파일로부터 buf 배열에 문자를 읽음 |
| `int read(char[] buf, int off, int len)` | 파일로부터 `buf` 배열의 `off` 위치에서부터 `len` 개수만큼 문자를 읽음 |
| `void close()` | 스트림과 연결된 파일 리소스를 닫음 |

__예시__

```java

try(FileWriter fw = new FileWriter("writer.txt")){
  fw.write("A");
  char[] cbuf = {'B', 'C', 'D'};
  fw.write(cbuf); //배열 출력
  fw.write("안녕하세요"); //String 출력
  fw.write(cbuf, 1, 2); //cbuf[1] 부터 2개 출력
  fw.write("123");  //String 출력
}catch(IOException e) {
  System.out.println(e);
}
```

## 기반 스트림 / 보조 스트림
- 기반 스트림: 대상에 직접 자료를 읽고 쓰는 기능의 스트림
- 보조 스트림: 직접 읽고 쓰는 기능은 없이 추가적인 기능을 더해주는 스트림
- 보조 스트림은 직접 읽고 쓰는 기능은 없기 떄문에 항상 기반 스트림이나 또 다른 보조 스트림을 생성자 매개 변수로 포함
- 스트림 종류

| 종류 | 예시 |
| -- | -- |
| 기반 스트림 | `FileInputStream`, `FileOutputStream`, `FileReader`, `FileWriter` 등 |
| 보조 스트림 | `InputStreamReader`, `OutputStreamWriter`, `BufferedInputStrean`, `BufferedOutputStream` 등 |



