---
title: "스프링부트는 뷰에서 세션을 열어둔다."
date: 2022-01-23T23:28:00+09:00
categories:
  - 메모
tags:
  - Spring
  - Design Pattern
# header:
#   teaser: /assets/images/code.jpg
---

뷰에서 세션을 여는 패턴(Open session in view pattern)은 스프링 부트에서 객체 관계 매핑을 할 때 사용하는 디자인 패턴의 하나입니다. 스프링 부트에서는 `OpenEntityManagerInViewInterceptor`를 통하여 나도 모르게 지원해주고 있습니다.

뷰에서 세션을 여는 패턴을 이해하기 위하여 먼저 다음과 같은 Team 도메인을 생각합시다. Team 도메인은 Member 도메인과 1:N의 관계를 가지고 있습니다. 한 Team에는 여러 Member들이 소속되어 있고, 한 Member는 한 Team에 속합니다.

```java
@Entity
public class Team{
  @Id
  @GeneratedValue
  private long idx;

  @Column(nullable = false, length = 20)
  private String name;

  @OneToMany(mappedBy="team")
  private List<Member> members;
}
```

보시면 One에 해당하는 클래스를 Many에 해당하는 필드와 연관 짓기 위하여 `@OneToMany` 어노테이션을 사용하는 것을 알 수 있습니다. JPA의 기본 select에서 Team을 조회하면 members 객체는 (N에 해당하기 때문에) Lazy Loading Fetch 전략에 맞춰서 데이터가 액세스 될 때에 비로소 데이터베이스로부터 가져와지게 됩니다. 다시 말해 `.getMembers()`를 통하여 객체를 호출해왔을 때가 아니라 `.iterator()` 등으로 내부 값을 호출할 때 Fetch 됩니다. 대신 실제 객체의 참조를 보관하는 프록시 객체를 생성하여 대신합니다.

JPA(보다 정확히는 Hibernate)는 Session을 통하여 영속성을 관리합니다. Session 안에는 업무 처리의 단위인 Transaction과 1대 1 대응되는 Persistence Context가 존재하기도 합니다. 하나의 Transaction 동안 조회, 수정, 삭제되던 Persistence 상태의 객체 정보는 Session 안의 Persistence Context에 기록되다가, Transaction이 종료되면 Session도 `.close()`되어 Detached 상태의 객체가 되고, 데이터 저장소에 동기화(flush)됩니다. 

그런데 다음과 같이 View 층에서 연관 객체를 사용하려고 할 때가 있습니다. 

```java
//Controller 
@GetMapping("") 
public String home(Model model){ 
  model.addAttribute("teams", teamService.findAll()); 
  return "home"; 
} 
  
//Service 
@Transactional 
public List<Team> findAll(){ 
  return teamRepository.findAll(); 
}
```

이 경우 Service 층에서 관리되는 Transaction이 View 층으로 넘어가면서 종료됩니다. 과거에는 위의 사례의 뷰 렌더링 시점에서 객체는 Detached 상태이고 Persistence Context가 존재하지 않기 때문에, `LazyInitializationException`이 발생하였습니다. 그러나 이에 대한 많은 논쟁이 있었고, 뷰에서도 세션을 열어두는 디자인 패턴에 힘이 실리게 되었습니다. 이러한 뷰에서 세션을 여는 패턴(Open session in view pattern)이 허용되면, Transaction이 종료된 후에도 View 층에서 Session이 `.close()` 되지 않았기 때문에 객체는 Persistence 상태를 유지할 수 있으며, 프록시 객체에 대한 Lazy Loading도 가능하게 됩니다. 