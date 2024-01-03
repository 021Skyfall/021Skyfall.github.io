---
title: "프로젝트:샐로그 / 이슈 4 - Pageable과 PageRequest"
author: Jeremiah Lee
date: 2024-01-02
categories: [ 프로젝트, 샐로그, 문제해결 ]
tags: [프로젝트, 샐로그, 문제해결, 페이징 ]
pin: false
math: true
mermaid: true
image: 
  path: /assets/img/Project_jelly.png
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: 
---
***

### ***1. 개요***

현재 한창 가계부 쪽 로직을 진행 중인데, 이전에 맞딱뜨렸던 페이징 문제에 대해 한 번 더 기록한다.

수입 전체 조회 중 페이징 쿼리 메서드 리턴 값에 대한 페이징 처리를 하고 있었는데,
"Paging query needs to have a Pageable parameter"라는 메시지의 에러가 발생해 Bean 생성 예외가 발생했다.

왜 그런지 좀 찾아봤는데, 내가 페이징 처리를 하기 위해 사용한 Pageable 인스턴스를 생성할 때 
'java.awt.print.Pageable'를 임포트해서 생긴 문제였다.

서비스 객체와 리포지터리 객체에 임포트된 Pageable을 다시 'org.springframework.data.domain.Pageable' 패키지로 임포트했다.

이로써 문제는 해결되었지만 이참에 Pageable과 PageRequest에 대한 기록을 하고자 포스트하게 되었다.

<br>
<br>

### ***2. Paging이란?***

기본적으로 페이징 처리는 DB에 저장된 엔티티들을 페이지로 나누는 것이다.

예를 들어, DB에 여러 개의 게시판 글이 작성되어 있을 때, 클라이언트가 DB에 있는 게시판을 특정 수량으로 분류해서 몇 번째 항목을 정렬해서 달라는 요청을 할 수 있다.
이 경우, 백엔드에서는 DB에 있는 데이터를 클라이언트가 원하는 모습으로 가공하여 응답해주어야한다.

이와 같이, 일정 갯수 만큼 분류하고 분류된 부분들 중 특정 부분을 보내주기 위한 기술이 페이징이다.

<br>

페이징은 정렬의 개념을 갖고 있다.

DB의 수가 적고 서비스가 작을 때는 DB의 전체 목록을 쿼리 메서드로 정렬을 하거나 원하는 부분만 추려내어도 성능에서 큰 차이는 없을 것이다.

그러나 실제 서비스의 경우 기능들이 복잡하게 얽혀있는 때가 있고, 서비스가 커진다면 위와 같은 처리는 성능에 문제가 생길 수 있다.

그렇기 때문에 SpringBoot 내부에서 제공하는 페이징을 통해 효율적으로 data를 반환하는 것이 권장된다.

[판교의 메타몽 블로그](https://astrid-dm.tistory.com/484) 참고.

<br>
<br>

### ***3. Pageable과 PageRequest의 관계***

Pageable은 페이징 정보(페이지 번호, 페이지 크기, 정렬 정보)를 추상화한 인터페이스이다.

이를 구현하여 페이징 정보를 저장하고 처리하는 것이 구현 클래스인 PageRequest이다.

다시 말해, PageRequest는 페이지 번호, 페이지 크기, 정렬 정보를 사용하여 페이징 정보를 생성할 수 있다는 뜻이다.

이 Pageable과 PageRequest는 JPA에서 주로 사용되며, 리포지터리의 메서드에서 페이징 처리를 위해 보통 사용된다.

<br>
<br>

### ***4. 사용법***

Pageable 인스턴스를 생성하려면 PageRequest의 정적 메서드 of()를 사용한다.

이 메서드는 페이지 번호와 크기를 인자로 전달 받아 PageRequest 인스턴스를 반환한다.

여기서 정렬 정보를 추가하려면 Sort 인스턴스를 추가하여 인자로 전달할 수 있다.

<br>

간단한 사용 예시는 아래와 같다.

우선 페이징 처리를 위해 JPA Repository를 선언한다.

```java
public interface PostRepository extends JpaRepository<Post, Long> {
    Page<Post> findAll(Pageable pageable);
}
```

전달 인자로 Pageable을 넣는다.

```java
@Service
public class PostService {
    private final PostRepository postRepository;

    public PostService(PostRepository postRepository) {
        this.postRepository = postRepository;
    }

    public Page<Post> getPosts(int page, int size) {
        Pageable pageable = PageRequest.of(page, size, Sort.by("createdDate").descending());
        return postRepository.findAll(pageable);
    }
}

```

다음으로는 서비스 객체에서 페이지 번호와 페이지 크기, 정렬 정보(Sort)를 인자로 받아 PageRequest 인스턴스를 생성하고, 이를 
리포지터리의 findAll(Pageable pageable) 메서드에 전달하여 페이징 처리된 Post 객체를 DB에서 가져온다.

이런식으로 Pageable과 PageRequest를 사용하여 데이터를 페이징 처리할 수 있다.

<br>
<br>

### ***5. 적용***

이제 내가 작성한 전체 조회 로직에 적용된 모습이다.

```java
        if (day == 0) {
            incomes = incomeRepository.findByMonth(year, month,
                    PageRequest.of(page - 1, size - 1, Sort.by("date").descending()));
        } else {
            incomes = incomeRepository.findByDate(year, month, day,
                    PageRequest.of(page - 1, size - 1, Sort.by("date").descending()));
        }
```

앞선 예시와 마찬가지로 PageRequest 인스턴스를 생성하여 페이지 번호, 사이즈, 정렬 정보를 리포지터리의 메서드로 전달한다.

```java
public interface IncomeRepository extends JpaRepository<Income, Long> {

    @Query("SELECT i FROM Income i WHERE YEAR(i.date) = :year AND MONTH(i.date) = :month")
    Page<Income> findByMonth(@Param("year") int year, @Param("month") int month, Pageable pageable);
    @Query("SELECT i FROM Income i WHERE YEAR(i.date) = :year AND MONTH(i.date) = :month AND DAY(i.date) = :day")
    Page<Income> findByDate(@Param("year") int year, @Param("month") int month, @Param("day") int day, Pageable pageable);
}
```

그리고, 리포지터리에서 Pageable 인터페이스를 매개변수로 입력하여 앞선 서비스 객체에서 정보를 받아온다.

이렇게 함으로써 DB의 데이터를 페이징 처리하여 리턴하게 할 수 있다.


<br>
<br>
<br>

***

로직에 대한 상세한 설명은 넘겼다.

이번 포스트는 단순히 페이징 처리에 대한 글이기 때문에 서비스, 레포 로직에 대한 설명은 배제했다.

앞으로 페이징 처리가 헷갈릴 때 여기로 와서 다시 보면 된다.
