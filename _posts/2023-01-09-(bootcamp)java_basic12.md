---
title: "자바 : 파일 입출력과 compare"
author: Jeremiah Lee
date: 2023-01-09
categories: [ 자바, 기초 ]
tags: [자바, 부트캠프]
pin: false
math: true
mermaid: true
image: 
  path: /assets/img/Academic_jelly.png
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: 
---
***

### I/O

### InputStream, OutputStream
- 바이트 기반, 즉, 입출력 단위가 1byte임
- 통틀어 바이트 기반 스트림이라함
- 영어만 input 가능 // 다른 언어면 유니코드 지원을 안해서 기호로 나옴
- output스트림은 한글로 잘 되는데 input 스트림으로 값 받아와서 출력 해보니 기호로 나왔음

### FileInputStream <바이트>
- 파일에서 값을 받아오는 클래스

ex)

```java
public class File_Input {
  public static void main(String[] args) {
    try {
      FileInputStream input = new FileInputStream("code.txt");
      BufferedInputStream bufferedInputStream = new BufferedInputStream(input);
      int i = 0;
      while ((i=input.read()) != -1) {// input.read() 의 리턴 값을 i 에 저장하고
        // 값이 -1 인지 확인
        System.out.println((char) i);
      }
      input.close(); // 말 그대로 인풋 로직을 끝냄
    } catch (Exception e) {
      System.out.println(e);
    }
  }
}
// code.txt 파일을 생성한 후 이 코드를 입력하면 그 파일의 내용을 입력 받아 실행됨
```

<br>

### FileOutputStream <바이트>
- 입력한 값을 파일로 내보내는 클래스
- 해당하는 이름의 파일이 없을 경우 생성함

ex)

```java
public class File_Output {
  public static void main(String[] args) {
    try {
      FileOutputStream outputStream = new FileOutputStream("codeX.txt");
      BufferedOutputStream bufferedOutputStream = new BufferedOutputStream(outputStream);
      // Buffered 얘네는 그냥 처리해야하는 값이 아주 많을 때 사용하는 임시 저장고 같은 느낌
      // productRepository 처럼
      String word = "code\ncodes\nand\ncode";

      byte[] b = word.getBytes(); // 파일 입출력 스트림은 바이트 기반 스트림임
      outputStream.write(b); // 작성
      outputStream.close(); // 끝
    } catch (Exception e) {
      System.out.println(e);
    }
  }
}
```

<br>

### BufferedInputStream & BufferedOutputStream
- 보조 스트림
- 처리 성능이 향상
- 버퍼란 바이트 배열로서, 여러 바이트를 저장하여 한 번에 많은 양의 데이터를 입출력할 수 있도록 도와주는 임시 저장 공간   
~> 입출력 데이터가 많을 때 쓸 듯

<br>

### FileReader / FileWriter
- 앞서 말했듯이 입출력 스트림은 1byte 기반이고 자바의 char 타입 문자들은 전부 2byte임
- 거기에 맞춰 나온 것들인 리더와 라이터
- 통틀어 문자 기반 스트림이라 함
- 자바에서 사용하는 유니코드(UTF-16)간의 변환을 자동으로 처리
- FileInputStream == FileReader / FileOutputStream == FileWriter
- 마찬가지로 성능을 개선 할 수 있는 BufferedReader & BufferedWriter 존재함

<br>

### FileWriter

ex)

```java
public class FileWrite { // 문자 기반 스트림 (2byte)
    public static void main(String[] args) {
        try {
            String name = "File파일.txt";
            FileWriter writer = new FileWriter(name);
            BufferedWriter bufferedWriter = new BufferedWriter(writer);

            String str = "write 작성";
            writer.write(str);
            writer.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

<br>

### FileReader

ex)

```java
public class FileRead { // 문자 기반 스트림 (2byte)
    public static void main(String[] args) {
        try {
            String name = "File파일.txt";
            FileReader reader = new FileReader(name);
            BufferedReader bufferedReader = new BufferedReader(reader);

            int i = 0;
            while ((i=bufferedReader.read()) != -1) {
                System.out.println((char) i);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

<br>

### File
- 파일과 디렉토리에 접근, 생성, 수정

ex)

```java
public class FileExample {
    public static void main(String[] args) throws IOException {
        File file = new File("File파일.txt");
        File file1 = new File("./","new파일.txt"); // 파일 생성을 위해 인스턴스화
                                                                // 주소, 이름
        file1.createNewFile(); // 파일 생성 메소드

        System.out.println(file.getPath()); // 파일 이름? 위치?
        System.out.println(file.getParent()); // 아 이거 상위 폴더 체크
        System.out.println(file.getCanonicalPath()); // 이거 주소 찍어줌
        System.out.println(file.canWrite()); // 작성 가능 여부

        // 파일명에 덧붙이기
        File file2 = new File("./"); // 앞선 파일들 인스턴스화
        File[] list = file2.listFiles(); // 파일 타입의 배열 생성 후 파일 담기
        String fix = "code";

        for (int i = 0; i < list.length; i++) { // 배열에 담긴 파일 개수만큼 반복
            String name = list[i].getName(); // 파일 이름 얻어오기
            if (name.endsWith("txt") && !name.startsWith("code")) { // txt로 끝나거나 code로 시작하지 않는 파일 거르기
                list[i].renameTo(new File(file2,fix+name));
                // i 차수에 걸린 파일 인스턴스 화 후 이름 다시 변경
            }
        }
    }
}
```

<br>
<br>

### ++ 추가 ++

### comparator &comparable   
- 객체를 비교하기 위해서 만들어진 개념


### Comparable
- 인터페이스를 비교하고자하는 객체에 구현
- 인터페이스를 구현한 객체를 전달
- 인스턴스 자기 자신과 다른 객체 비교할때 Comparable 의 compareTo() 사용

ex)

```java
class Person extends Comparable<Person> { // 컴퍼러블 클래스 불러서 비교할 기준 정해줌
public int compareTo(Person p) {
	return p;
	}
}
```

<br>

### Comparator
- compare(a1,a2) 메소드 사용
- 보통 생성 후 익명객체를 만들어 씀

ex)

```java
Comparator<Person> comparator = new Comparator<Person>() {
public int compar(Person a1, Person a2) {
	}
}
```

<br>
<br>
<br>

***

[이전 블로그](https://blog.naver.com/021skyfall/222979311580)에서 옮겨짐
