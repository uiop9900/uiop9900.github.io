---
title: origin과 master, 그리고 main
date: 2022-11-23 17:18:00
categories: [TIL]
tags: [til, git]  
---

git 이 꼬여서 최신의 코드를 확인할 수가 없다.. 어디서 부터 잘못된거지?

### [상황]
develop 브랜치에서 pull 받음 -> 충돌 -> git rebase --continue -> 충돌 해결 -> git rebase -> 충돌 -> ...-> 무한 반복


### [원인파악]

1. rebase를 하니 자꾸 같은 파일이 충돌이 나서 먼저 rebase를 취소했다.
```java
git rebase --abort
```

2. 그 후 다시 처음으로 돌아와 pull을 받았다.

```java
User ~/project/projectApi   develop 
	git pull origin master
```

~~pull 당기는 모습을 보고 에러를 봐주던 동료가 경악했다.~~
##### 여기다.  여기서 잘못된 브랜치를 당겨오고 있었다.

- main(master) 브랜치는 기본적으로 생성되는 브랜치이다.
- origin은 저 프로젝트의 별칭이다. 
- 그런데 나는 origin과 main(master) 을 제대로 이해하지 못해서 main(master) 이 근본 브랜치니까 무조건 저기에서 당겨오면 된다고 생각한 것이다.

---

현재 회사는 dev - qa - stage  단계로 되어있고 main(master) 브랜치는 stage와 동일시되어있다.
그렇기때문에 내가 개발하는 dev는 당연히 stage보다 앞서있고(제일 먼저 수정이되는 브랜치) 나는 그걸 모르고 계속 stage를 당겨오니 충돌이 생기는 것이다.

### [해결]

```java
User ~/project/projectApi   develop 
	git pull origin develop
```

위와 같이 develop에서 당겨오니 충돌없이 바로 Pull되었다..
항상 내가 어떤 브랜치에서 당겨와야하는지. 현재 내 브랜치가 어디에 뿌리를 두고 있는 지 확인하고 pull을 해야한다.

~~막상 알고보니 어이가 없는 실수인데.. 이걸 몰랐다니.. 이제라도 알아서 다행이다.
또  틀리면 진짜 혼날거 같아서 기록.~~




