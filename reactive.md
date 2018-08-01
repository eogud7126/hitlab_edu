## The introduction to Reactive Programming you've been missing
(by [@andrestaltz](https://twitter.com/andrestaltz))

- - -

### This tutorial as a series of videos

**If you prefer to watch video tutorials with live-coding, then check out this series I recorded with the same contents as in this article: [Egghead.io - Introduction to Reactive Programming](https://egghead.io/series/introduction-to-reactive-programming).**

- - -

특히 이 Rx의 변종된 구성과 Bacon.js, RAC 과 기타 등등에 대해서 당신은 Reactive Programming이라 불리는 것을 배우는 것에 대해 궁금해할 것입니다

이것을 배우는 것은 어렵지만, 좋은 자료들이 부족하여 더 어려울 것입니다. 제가 시작할 때 튜토리얼에 대해 찾아보려 시도했습니다. 저는 오직 유용한 실용적인 가이드만 찾았고, 그것들은 수박 겉핥기에 불과하고 전체적인 구조에 대해서는 진입하지도 않았습니다. 라이브러리 자료는 몇 기능들을 이해하는데에 도움이 되지 않았습니다. 예를들어, 

> **Rx.Observable.prototype.flatMapLatest(selector, [thisArg])**

> 요소의 색인을 통합하여 관측 가능한 시퀀스의 각 요소를 관측 가능한 시퀀스의 새로운 시퀀스로 프로젝트한 다음 관측 가능한 시퀀스를 가장 최근에 관찰된 시퀀스에서만 값을 생성하는 관측 가능한 시퀀스입니다.

이게 무슨말인가요..

저는 책을 2권 읽었는데, 하나는 그저 큰 그림을
그려놨고, 다른 하나는 어떻게 Reactive 라이브러리로 진입하는지에 대해 다뤘습니다. 그 결과 Reactive Programming 에 대해 어려운 방법으로 배우는것으로 마무리 되었습니다. 

이 배움의 가장 어려운 부분은 **Reactive하게 생각해보기**입니다. 이는 전형적인 프로그래밍의 고질적인 형식(습관)을 버리는 내용과 당신의 뇌를 다른 패러다임으로 전환하는 내용이 많이 있습니다. 저는 이러한 방면으로 인터넷에서 아무런 가이드를 찾지 못했고, 세상이 어떻게 Reactive적으로 생각하는지에 대한 실용적인 지침을 받을만 하다고 생각했습니다. 그래서 당신은 시작할 수 있습니다. 라이브러리 자료는 당신이 좀 배우고 난 뒤에 도움이 될 수 있을것입니다. 이 자료가 당신에게 도움이 되기를 기원합니다.

## "Reactive Programming이란 무엇인가??"

많은 잘못된 설명들과 정의들이 인터넷에 있습니다. [위키피디아](https://en.wikipedia.org/wiki/Reactive_programming)는 너무 일반적이고 이론적입니다  [Stackoverflow](http://stackoverflow.com/questions/1028250/what-is-functional-reactive-programming)는 새롭게 배우는 사람들에게 너무 맞지 않습니다. [Reactive Manifesto](http://www.reactivemanifesto.org/) 당신의 회사에 PM이나 사업가들에게 보여주는 것 같이 보입니다. Microsoft's [Rx terminology](https://rx.codeplex.com/) "Rx = Observables + LINQ + Schedulers" 는 너무 무겁고 마이크로소프트하고 혼란스럽습니다. reactive와 propagation of change (전파의 변화)와 같은 용어는  구체적으로 어느것에도 전달하지 않고, 당신의 전형적인 MV*와 가장 즐겨찾는 언어와의 다른점을 전달하지 않습니다. 당연히 제  관점은 모델에 대해 맞춰져있습니다. 당연히 변화는 전파됩니다. 만약 그렇지 않다면 아무것도 만들어내지 못했을 것입니다.

그럼 이제 헛소리를 빼고 이야기 해봅시다.

#### Reactive Programming은 비동기식의 데이터 스트림으로된 프로그래밍입니다.

어떻게 보면 이것은 새로운 것이 아닙니다. 이벤트 버스 또는 당신의 전형적인 클릭 이벤트는 실제로 비동기적인 이벤트 스트림이고, 이것은 당신이 관찰하고  부작용에 대해서 무언가 할 수 있게 할 수 있습니다. Reactive는 아이디어의 스테로이드 입니다. 당신은  그저 클릭과 호버 이벤트로부터 만드는것이 아닌 모든것들의 데이터 스트림을 만들 수 있습니다. 스트림들은 저렴하고, 아주 흔합니다. 변수, 사용자 입력, 재산, 캐시, 자료구조 등 모든것들이 스트림이 될 수 있습니다. 예를 들어 당신의 트위터피드가 클릭 이벤트와 같은 방식으로 데이터 스트림이 된다고 상상해봅시다. 당신은 그 스트림에 따라 반응하고 들을 수 있습니다.


**가장 위에 보면 당신은 어떠한 스트림에 대해 조합하고, 생성하고, 정렬 하는 기능의 툴박스가 주어졌습니다.** 그것은 "기능적인" 마법의 시작인 것입니다. 스트림은 다른 스트림에 입력할 수 있도록 사용하곤 합니다. 심지어 다수의 스트림들이 여러 스트림에 입력 할 수 있습니다. 당신은 두 스트림을 합칠 수 있고, 걸러낼 수 있고, 다른 스트림으로부터 데이터를 맵핑할 수 도 있습니다. 

만약 스트림이 Reactive에 너무 가운데에 위치한다고 하면, 우리가 친숙한 버튼을 클릭하는 이벤트를 시작으로 주의 깊게 둘러봅시다.

![Click event stream](https://gist.githubusercontent.com/staltz/868e7e9bc2a7b8c1f754/raw/35cc1edb69b7175fd1308800a244410890bc9b5f/zclickstream.png)

스트림은 **시간의 흐름에 따라 진행되는 연속적인 것**입니다. 이것은 value, error, completed 신호의 세가지를 방출 할 수 있습니다. 
"completed"라는 신호가 발생했을때 현재 보고있는 버튼이 포함된 창을 닫았을 때의 예를 들어봅시다. 

 우리는 이러한 방출된 이벤트를 오직 value가 방출되었을때, error가 방출되었을때, complete가 방출되었을때의 함수를 **비동기식**으로만 포착합니다. 가끔은 이 마지막 두개가 방출되어, 당신이 그저 values를 위해 함수를 정의하는것에 집중할 수 있습니다. 그 스트림을 "listening(듣는것)"을 **subscribing(구독)** 이라고 합니다. 우리가 정의한 기능은 observers(관찰자)입니다. 그 스트림은 관찰(observe)되는 것의 주체가 되는것입니다. 이것이 바로 [Observer Design Pattern(관찰자 설계 패턴)](https://en.wikipedia.org/wiki/Observer_pattern).입니다.


이 다이어그램을 그리는 다른 방법은 이 튜토리얼의 일부에서 사용 할 ASCII를 사용하는것입니다.


```
--a---b-c---d---X---|->

a, b, c, d are emitted values
X is an error
| is the 'completed' signal
---> is the timeline
```

이것은 굉장히 친숙하게 느껴집니다. 저는 당신을 지루하게 만들지 않을 것입니다. 우리는 기존의 클릭 이벤트 스트림의 형식에서 벗어나 새로운 클릭 이벤트 스트림을 만들어 보는 새로운 것을 할 것입니다.

제일 먼저, 얼마나 자주 버튼이 클릭되었는지 가르켜주는 counter 스트림을 만들어봅시다.  일반적인 Reactive 라이브러리는 각 스트림에 ` map`, `filter`, `scan` 등의 많은 기능들이 연결되있습니다. 당신이 이러한 기능들중 하나를  호출하면(예를 들어 clickStream.map(f) ), 이것은 click stream 기반의 **새로운 스트림**으로 반환합니다. 이는 기존의 클릭스트림을 수정하지 않습니다. 이것은 **불변성(immutability)**이라고 불리는 자산이고, 이것은 Reactive 스트림과 함께 시행됩니다. (팬케이크와 시럽이 잘어울리는것 처럼) 그것은 우리가 `clickStream.map(f).scan(g)`와 같은 기능처럼 chain(엮을수) 있도록 합니다. :

```
  clickStream: ---c----c--c----c------c-->
               vvvvv map(c becomes 1) vvvv
               ---1----1--1----1------1-->
               vvvvvvvvv scan(+) vvvvvvvvv
counterStream: ---1----2--3----4------5-->
```

`map(f)` 함수는 (새로운 스트림속에서) 당신이 만들어낸 함수 f 에 따르는 각 방출된 value를 대체 합니다.  우리의 경우, 우리는 각 클릭에 숫자 1을 map 했습니다. `scan(g)` 함수는 스트림에서의 전의 모든 value들을 병합하는 기능을 합니다. `value x=g(accumulated(모아진),current(현재))` 예시에서 `g`는 그저 더하기 함수입니다. 그러면 `counterStream` 은 전체의 클릭횟수를 방출하게 됩니다. 


Reactive 의 진정한 힘을 보여주기 위해, 당신이 "더블클릭"의 스트림을 갖고싶어한다고 가정합시다. 더욱 흥미롭게 만들기 위해 더블클릭과 같이 트리플클릭 또는 여러번 클릭하는 새로운 스트림을 만들고자 한다고 칩시다. 깊은 숨을 들이 쉬고 당신은 어떻게 (앞에서 말했던) 고전적으로 필수적인 방법으로 구현해 낼지 상상해 봅시다. 저는 그것이 꽤 불쾌하게 들리고, 상태를 유지하기 위한 몇몇 변수와 시간 간격을 가지고 움직이는 변수를 포함한다고 확신합니다. 

Reactive에서는 간단합니다. 사실 이 로직은[4줄의 코드](http://jsfiddle.net/staltz/4gGgs/27/)면 끝납니다. 하지만 지금부터 코드는 무시합시다. 당신이 초급자든 전문가든 간에 다이어그램으로 생각하는것이 스트림을 만들고 이해하는데 가장 좋은 방법입니다.  


![Multiple clicks stream](https://gist.githubusercontent.com/staltz/868e7e9bc2a7b8c1f754/raw/35cc1edb69b7175fd1308800a244410890bc9b5f/zmulticlickstream.png)

회색박스들은 다른 스트림으로 변형시키는 함수입니다. 먼저 우리는 리스트에 250밀리초의"event silence"가 발생할때마다 생기는 클릭들을 모을것 입니다. `(buffer(stream.throttle(250ms)` 이 부분에서 세부적인 것에 이해하는것에 대해서는 걱정하지 마십시오. 우리는 그저 지금 Reactive에 대해 데모를 시작하는 것입니다.)
그 결과, 리스트의 스트림이 생기고, 여기서 각 리스트를 해당 리스트의 길이와 일치하는 integer로 매핑 하는 데 사용됩니다. 마침내 우리는 `filter(x>=2)` 조건을 사용하는 1 integer만큼의 함수를 무시합니다. 우리의 의도된 스트림들을 생성하기위한 3가지 연산자인 것입니다! 우리는 그럼 우리가 원하는 방식에 따라 react 하도록 `subscribe("listen")` 합니다.

저는 당신이 이 접근의 아름다움을 즐기길 원합니다. 이 예제는 빙산의 일각일 뿐입니다. 당신은 같은 operation(연산자)를 다른 종류의 스트림에 적용할 수 있습니다. 예를 들어, API의 응답에 대한 스트림에 적용할 수도 있습니다. 이처럼 많은 다른 기능들이 있다는 것입니다. 

## "왜 RP를 채택하는 것을 생각하여야 합니까?"

Reactive Programmin은 당신이 짠 코드의 축약을 늘려줄 것입니다. 그래서 당신은 많은 양의 구현에 대해 세부 사항을 지속적으로 처리할 필요 없이 사업의 로직을 정의하는 이벤트 하나만 집중할 수 있습니다. RP로 코딩하는 것은 좀 더 간결하게 할 것입니다.

이것의 장점은 현대의 다양한 UI이벤트와 관련된 데이터 이벤트와 상호 작용하는 웹앱과 모바일 앱 좀 더 분명한 것입니다. 10년전, 웹페이지와 상호작용하는 것은 긴 형식의 기본적인 백엔드를 제출하는 것이였고, 간단한 프론트엔드를 만드는것을 수행하는 것이었습니다. 앱들은 더욱 실시간으로 진화하고있습니다. 단일 폼 필드를 수정하면 자동으로 백엔드로 저장본을 트리거하고, 일부 내용들은 다른 사용자들과 연결되어 실시간으로 반영될 수 있습니다.

요즘의 앱들은 사용자에게 높은 수준의 대화형으로 가능토록 하는 모든 종류의 풍부한 실시간 이벤트를 가지고 있습니다. 우리는 때에 따라 적당한 툴이 필요합니다. 그것은 바로 Reactive Programming입니다.