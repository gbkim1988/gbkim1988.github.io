---
title: Qt signal & slot
key: 20180824
tags: qt c++ signal slot
---

Qt에서 시그널을 생성하고 이를 slot 과 연결하는 과정을 담음 ...

<!--more-->

**참조 링크**
- [The oberver patern](https://wiki.qt.io/Qt_for_Beginners#The_observer_pattern)

# 옵저버 패턴

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

# Signals and slots

- signal : a message that an object can send, most of the time to inform of a status change.
- slot : a function that is used to accept and respond to a signal.

QPushButton 클래스의 signal 예를 들어보자. 아래의 시그널들은 사용자 클릭에 의해서 발생된다.
- clicked
- pressed
- released

타 클래스의 slot 의 예가 있다.
- QApplication::quit
- QWidget::setEnabled
- QPushButton::setText

signal 에 응답하기 위해서 slot 은 반드시 signal 에 연결되어야 한다. Qt 는 QObject::connect 메서드를 제공한다. 이 메서드를 통해 연결한다. 그리도 동시에 SIGNAL, SLOT 매크로를 함께 사용한다.

```
FooObjectA *fooA = new FooObjectA();
FooObjectB *fooB = new FooObjectB();

QObject::connect(fooA, SIGNAL(bared()), fooB, SLOT(baz()));
```

위의 코드는 FooObjectA 가 bared 시그널(signal)을 정의하고 FooObjectB 가 baz 슬롯(slot)을 정의함을 가정한다.

**Remark** : 기본적으로 시그널(signal)과 슬롯(slot)은 메서드들이다. 이 메서드들은 인자를 가질 수도 그렇지 않을 수도 있다. 하지만 절대(never) 어떠한 것도 반환(return) 하지 않는다. 메서드로서 시그널(signal)의 개념은 일반적이지 않은 반면에, 슬롯(slot)은 실제 메서드이다. 그리고 다른 메서드들에서도 호출될 수 있다.

# 정보 전송

시그널(signal) 과 슬롯(slot)은 버튼 클릭에 응답하는데 유용하다. 그러나 훨씬 더 많은 것을 할 수 있다. 예를 들어, 정보를 교환하는데 사용될 수 있다. 노래를 재생하는 중에 progress bar는 노래 종료까지 얼마 만큼 시간이 남았는지 보여줄 필요가 있다. 미디어 플레이어는 미디어의 진행을 체크하기 위한 클래스가 있어야할지도 모른다. 이 클래스의 인스턴스는 주기적으로 tick 시그널(signal) 을 전송할 것이고 이때 progress value 를 함계 전달한다. 이 시그널은 QProgressBar 에 연결될 수 있으며 이는 진행상태를 보여주는데 사용할 수 있다.

아래는 가상의 클래스로 진행률을 체크하는데 사용되며 아래의 시그니처와 같은 signal(시그널)을 보유한다.

```
void MediaProgressManager::tick(int milliseconds);
```

그리고 문서에서 알 수 있듯이 QProgressBar 는 아래의 slot 을 가진다.

```
void QProgressBar::setValue(int value);
```

위의 가상 함수 및 실제 Widget 의 slot 에서 확인할 수 있듯이 동일한 종류의 파라미터를 가졍한다. 특히 타입이 중요하다. 만약 시그널(signal)을 슬롯(slot)과 연결하되 이들의 시그니처가 다르다면 런타임 동작 중에 연결이 완료되고 아래와 같은 경고를 보게 될 것이다.

```
QObject::connect: Incompatible sender/receiver arguments
```

이는 시그널(signal)이 파라미터를 이용해 슬롯(slot)에 정보를 전달하기 때문이다. 시그널(signal)의 첫 번째 파라미터는 슬롯(slot)의 첫번째 파라미터에 전달되고 두 번째, 세 번째 등등도 각각 그러하다.

시그널(signal) 과 슬롯(slot) 을 연결하는 코드는 아래와 같다.
```
MediaProgressManager *manager = new MediaProgressManager();
QProgressBar *progress = new QProgressBar(window);

QObject::connect(manager, SIGNAL(tick(int)), progress, SLOT(setValue(int)));
```




---

If you like the post, don't forget to give me a start :star2:.

<iframe src="https://ghbtns.com/github-btn.html?user=gbkim1988&repo=gbkim1988.github.io&type=star&count=true"  frameborder="0" scrolling="0" width="170px" height="20px"></iframe>
