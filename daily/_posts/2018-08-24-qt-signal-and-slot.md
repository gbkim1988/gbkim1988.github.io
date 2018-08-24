---
title: Qt signal & slot
key: 20180824
tags: qt c++ signal slot
---

Qt에서 시그널을 생성하고 이를 slot 과 연결하는 과정을 담음 ...

<!--more-->

**참조 링크**
- [The oberver patern](https://wiki.qt.io/Qt_for_Beginners#The_observer_pattern)

#옵저버 패턴

모든 UI 툴킷은 사용자 행위를 감지하고 응답하는 메커니즘을 갖고있다. 툴킷들 중에 어느 것은
callback 을 사용하고 어느 것은 listener를 사용한다. 그러나 기본적으로 옵저버 패턴에서
파생되었다.

옵저버 패턴은 observable 객체가 observer 객체에게 상태 변화를 알릴 때 사용된다.
구체적인 사례는 아래와 같다.

- 사용자는 버튼을 클리갛고 메뉴가 표시되어야 한다.
- 웹 페이지는 로딩을 끝내고 프로세스는 로드된 페이지로부터 정보를 추출해야 한다.
- 사용자는 아이템의 목록을 스크롤하며 스크롤의 끝에 도달하였을 때 다른 항목들이 로드 되어야 한다.

옵서버 패턴은 GUI 어플리케이션의 어디에서나 사용된다. 그리고 boilerplate 코드가 필요하다.
Qt 는 이러한 boilerplate 코드를 제거하고 깔끔한 문법을 제공한다. signal/slot 메커니즘이 그 답이다.

#Signals and slots


---

If you like the post, don't forget to give me a start :star2:.

<iframe src="https://ghbtns.com/github-btn.html?user=gbkim1988&repo=gbkim1988.github.io&type=star&count=true"  frameborder="0" scrolling="0" width="170px" height="20px"></iframe>
