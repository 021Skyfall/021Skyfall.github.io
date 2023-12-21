---
title: "자바 : Json"
author: Jeremiah Lee
date: 2023-01-13
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

### JSON
- 클라이언트는 보통 jsc 쓰고 모든 협업하는 사람들이 전부 자바를 쓰는게 아니니까 상대방이 사용할 수 있도록 변경해야함

<br>

### JSON의 탄생 배경
- JavaScript Object Notation의 줄임말로, 데이터 교환을 위해 만들어진 객체 형태의 포맷
- 전송가능한 조건 (transferable condition)
- 수신자(reciever)와 발신자(sender)가 같은 프로그램을 사용
- 또는, 문자열처럼 범용적으로 읽을 수 있어야 함
- Java를 사용하지 않는 프로그램에서는 데이터를 정확하게 파악할수 없음
- 이 문제를 해결하는 방법 ~> 객체를 JSON의 형태로 변환하거나 JSON을 객체의 형태로 변환하는 방법

1. 상대 방이 볼 수 있게 jackson 라이브러리에서 제공하는 ObjectMapper클래스를 사용하여 JSON형태로 변경
2. JSON으로 변환된 문자열 송신 = writeValueAsString하는 이 과정을 직렬화(serialize)한다고 함
3. 수신해서 코드 직렬화된 JSON에 메소드 readValue을 적용하면 다시 객체의 형태로 변환 = 역직렬화

<br>

### JSON의 기본 규칙

![](/assets/img/bootcamp/JSON_basic_rules.png)


- 또, JSON은 키와 값 사이, 그리고 키-값 쌍 사이에는 공백이 있어서는 안됨

<br>

### JSON의 형태로 함수 제작 과제

```java
public String stringify(Object data) throws JsonProcessingException {
  //입력된 값이 문자열일 경우
  if (data instanceof String) {
    return "\""+String.valueOf(data)+"\"";
  }
  //입력된 값이 Integer 일 경우
  if (data instanceof Integer){
    return String.valueOf(data);
  }
  //입력된 값이 Boolean 일 경우
  if (data instanceof Boolean) {
    return String.valueOf(data);
  }
  //입력된 값이 Object[]일 경우
  // [1,2,3,4,5] < 원하는 값
  if (data instanceof Object[]) { // [1,2,3,4,5]
    Object[] arr = (Object[]) data;
    for (int i = 0; i < arr.length; i++) {
      arr[i] = stringify(arr[i]);
    }
    return Arrays.toString(arr).replaceAll(" ","");
  }
  /* 1,2,3,4,5 가 들어온다고 가정함
   * data 를 강제 형변환을 통해 Object 타입 배열인 arr 에 집어넣음
   * for 문이 돌고
   * arr 길이만큼 돌도록 설정해줌
   * 왜? 우리가 필요한건 arr 에 들어온 배열을 하나씩 돌려서 stringify 에 정의된 return 문을
   * 만나게 하기 위함이니까
   * 그럼 반복문이 5번 돌아감
   * 맨 처음 반복문이 돌아가게되면 i = 0 임
   * 그럼 arr[0] = stringify(arr[0]) 이건?
   * arr 의 0번째 배열은 stringify(1) 이랑 같다는 뜻임
   * 그럼 곧 1 이라는 매개변수 인자를 갖고서 stringify 메소드가 돌아감
   * 그럼 integer 조건에 걸려서 문자열로 형변환 후 종료됨
   * 그럼 arr 의 0번째 배열은 String 타입의 1이 되는 것
   * 이렇게 쭉 다섯 번 돌개 되면
   * arr[0] = 1
   * arr[1] = 2
   * arr[2] = 3
   * arr[3] = 4
   * arr[4] = 5
   * 전부 String 타입으로 변환되서 arr 에 다시 담김
   * 그 후 반복문이 종료되고
   * String 타입의 요소를 가진 Object 배열 arr 이 빠져나오게 되고
   * 그 시점의 arr 값은 배열을 출력했을 때의 값인 [1 ,2 ,3 ,4 ,5] 임
   * 근데 우리는 json 포맷으로 값을 출력하고자함 해당 포맷은 공배이 있으면 안됨
   * 그래서 replaceAll 로 본래 배열을 출력하면 갖고있는 공백을 지워주는 거임
   * */


  //입력된 값이 HashMap 일 경우
  // {1:"a",2:"b",3:"c"} << 원하는 값
  if (data instanceof HashMap) {
    HashMap<Object, Object> map = (HashMap<Object, Object>) data;
    HashMap<Object, Object> result = new LinkedHashMap<>();
    for (Map.Entry<Object,Object> e : map.entrySet()) {
      String key = stringify(e.getKey());
      String value = stringify(e.getValue());
      result.put(key,value);
    }
    return result.toString().replaceAll("=",":").replaceAll(" ","");
  }
  /* data 는 HashMap 임
   * 강제 형변환 해서 data 를 끌어오고
   * 해당 data 값을 map 에 담아줌 ~> 강제 형변환이 없으면 타입 변환이 불가능함
   * 그리고 순서대로 값을 담을 result 를 LinkedHashMap 으로 생성
   * HashMap 순서를 따지지 않음 그래서 뒤죽박죽 될 수 있음
   * LinkedHashMap 은 순서를 지킴
   * for-each 문을 열어서 map 의 entry, 즉 키와 값을 한친 거지
   * 이걸 순차적으로 하나씩 받아와서 Map.Entry 타입인 e 에 집어 넣음
   * 그런 다음에 아래의 key 와 value 변수에 각각 키와 값을 나누어서 stringify 조건에 걸리게
   * 재귀함수 사용
   * 그 다음 빈 LinkedHashMap 인 e 에 키와 값으로 저장
   * 풀어서 설명하면
   * {a=1,b=2,c=3} 이 들어오면
   * map = {a=1,b=2,c=3} 이거고
   * result 는 그냥 빈 LinkedHashMap
   * 이제 for-each 문을 열면 (Map.Entry ~> Entry 를 저장하기 위함)
   * map.entry 로 맨 처음 키인 a 와 값인 1 이 들어감
   * 그럼 e 에 키로 1, 값으로 a 가 할당됨
   * 그런 다음에 String key 와 String value 변수로 각각 a 와 1 이 들어가고
   * 재귀호출이 되었으니
   * 각각 stringify 의 매개변수 인자로 들어감
   * 그럼 1과 a 는 따로 가공되서 다시 나오고 LinkedHashMap 인 result 의 키와 값으로 삽입됨
   * 또 다시 for-each 문으로 돌아가서 이번엔 b, 2 가 들어감
   * e 는 b를 키로 2를 값으로 가진 Entry 임
   * 또 마찬가지로 key 변수에 b 가 value 변수에 2 가 들어감
   * 또 각각 stringify 로 가공되어 나옴
   * 그리고 result 에 저장됨
   * 마지막 c, 3 도 마찬가지로 반복되고서 for-each 문이 종료됨
   * 그럼 그 시점에 result 의 값은
   * {"a"=1, "b"=2, "c"=3} 임
   * 근데 json 포맷은 빈 공간이 없어야하고
   * = 이 아니라 : 콜론으로 되어 있어야함
   * 그래서 마지막에 result 를 toString 을 통해 문자열로 바꾸어주고
   * replaceAll 로 빈 공간과 = 을 가공함
   * 끗
   * */



  //지정되지 않은 타입의 경우에는 "null"을 리턴합니다.
  return "null";
}
```

<br>
<br>
<br>

***

[이전 블로그](https://blog.naver.com/021skyfall/222983553290)에서 옮겨짐
