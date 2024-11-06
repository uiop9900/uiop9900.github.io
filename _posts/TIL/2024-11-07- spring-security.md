---
title: [TIL] Spring Security
date: 2024-11-07 17:18:00
categories: [TIL]
tags: [til, Spring-Security]
---

## Spring Security에서 인증(Authentication)과 권한 부여(Authorization)의 차이점은 무엇인가요? </b>

<b> 답변 초안 </b>

: 인증은 실 사용자인지 확인하는 것을 의미하고, 권한 부여는 해당 사용자에게 권한을 주는 것을 말한다.

예를 들어, 한 명의 사용자가 로그인을 할때 해당 사용자의 정보를 기반으로 해당 사용자가 우리 서비스에 유저인지 확인하는 절차를 인증 이며,  그 사용자가 갖고있는 권한을 확인해서 그에 알맞은 권한을 부여하는 것을 권한부여 라고 한다.

이런식으로 인증/인가를 하게되면 유저마다 역할이 정해져있어서, 서비스에서도 그 역할에 따라 접근유무가 결정된다.



---

### 인증(Authentication)
: 사용자의 신원을 확인한다.

- `UsernamePasswordAuthenticationToken`이나 `OAuth` 를 이용해서 많이 신원을 확인한다.

- `UsernamePasswordAuthenticationToken`
  - 로그인 할때 ID, password로 인증 처리
- `OAuth`
  - 제 3자의 서비스(구글)을 통해서 인증 처리



필터체인 → 어플리케이션으로 유저 정보를 넘겨주면 `AuthenticationManager` 로 유저 정보를 보내서 인증처리를 한다. 그 후 유저 정보를 `SecurityContext` 에 저장한다. 해당 정보는 유저가 로그아웃 할때까지 유효하다.



### 인가(Authorization)
: 사용자의 정보를 기반으로 허용된 역할을 부여한다.

`SecurityContext`에 저장된 사용자 정보(특히 역할 또는 권한)를 참조하여 애플리케이션이 각 사용자에게 적절한 권한을 부여한다.

이때 접근제어시에 url기준으로 접근을 막을수도, 메소드 기준으로 막을 수도 있음

---

### 뜬금없는 조각 정보

둘다 스프링 자체에서 제공해주는 기능으로, 정보를 저장하고 컨텍스트 내에서 일관되게 관리한다.

- security context
  - 사용자의 정보, Spring Security
- persistence context
  - 엔티티 객체 정보, JPA
