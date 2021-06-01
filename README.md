# WhatIsARC

## ARC란 무엇인가(What is Automatic Reference Counting?)?

iOS 개발자로서 취업 또는 이직을 준비해보신 분들이라면 자의에 의해서든, 혹은 타의에 의해서든 ARC의 존재에 대해 들어보셨을 것입니다.
Automatic Reference Counting을 이해하기 위해서는 우선적으로 Swift의 메모리 관리에 대해 아는 것이 중요하겠습니다.

* 자료 출처: 야곰의 스위프트 프로그래밍 3판 챕터 27

매번 전달할 때마다 값을 복사해 전달하는 값 타입과는 달리 참조 타입은 하나의 인스턴스가 참조를 통해 여러 곳에서 접근하기 때문에
언제 메모리에서 해제되는지가 중요한 문제입니다.
인스턴스가 적절한 시점에 메모리에서 해제되지 않으면 한정적인 메모리 자원을 낭비하게 되며, 이는 성능의 저하로 이어지게 됩니다.
스위프트는 프로그램의 메모리 사용을 관리하기 위하여 메모리 관리 기법인 ARC를 사용합니다

*참고: ARC와 값 타입
- ARC가 관리해주는 참조 횟수 계산(Reference Counting)은 참조 타입인 클래스의 인스턴스에만 적용됩니다.
- 구조체나 열거형은 값 타입이므로 참조 횟수 계산과 무관합니다.
- 즉, 구조체나 열거형은 다른 곳에서 참조하지 않기 때문에 ARC로 관리할 필요가 없습니다.

1. ARC란
- ARC 기능은 이름에서 알 수 있듯이 자동으로 메모리를 관리해주는 방식입니다.
- 아무래도 프로그래머가 메모리 관리에 신경을 덜 쓸 수 있기에 편리합니다.
- ARC는 더이상 필요하지 않은 클래스의 인스턴스를 메모리에서 해제하는 방식으로 동작합니다.

ARC와 가비지 컬렉션의 가장 큰 차이는 참조를 계산(Count)하는 시점입니다.
ARC는 인스턴스가 언제 메모리에서 해제되어야 할지를 컴파일과 동시에 결정합니다.
가비지 컬렉션은 그렇지 않습니다. 그에 따른 장단점이 있습니다

* ARC와 가비지 컬렉션의 메모리 관리 기법 차이점 정리
(1) 참조 카운팅 시점
  - ARC: 컴파일 시
  - 가비지 컬렉션: 프로그램 동작 중

(2) 장점
  - ARC
    - 컴파일 당시 이미 인스턴스의 해제 시점이 정해져 있어서 인스턴스가 언제 메모리에서 해제될지 예측할 수 있습니다.
    - 컴파일 당시 이미 인스턴스의 해제 시점이 정해져 있어서 메모리 관리를 위한 시스템 자원을 추가할 필요가 없습니다.
  - 가비지 컬렉션
    - 상호 참조 상황 등의 복잡한 상황에서도 인스턴스를 해제할 수 있는 가능성이 더 높습니다.
    - 특별히 규칙에 신경 쓸 필요가 없습니다.

(3) 단점
  - ARC
    - ARC의 작동 규칙을 모르고 사용하면 인스턴스가 메모리에서 영원히 해제되지 않을 가능성이 있습니다.
  - 가비지 컬렉션
    - 프로그램 동작 외에 메모리 감시를 위한 추가 자원이 필요하므로 한정적인 자원 환경에서는 성능 저하가 발생할 수 있습니다.
    - 명확한 규칙이 없기 때문에 인스턴스가 정확히 언제 메모리에서 해제될지 예측하기 어렵습니다.

* ARC를 이용해 자동으로 메모리 관리를 받기 위해 알아야 할 규칙
- 가비지 컬렉션과 달리 ARC는 컴파일과 동시에 인스턴스를 메모리에서 해제하는 시점이 결정되기 때문입니다.
- 우리가 원하는 방향으로 메모리 관리가 이루어지려면 ARC에 명확한 힌트를 주어야 합니다.
- 클래스의 인스턴스를 생성할 때마다 ARC는 그 인스턴스에 대한 정보를 저장하기 위한 메모리 공간을 따로 또 할당합니다.
- 그 메모리 공간에는 인스턴스의 타입 정보와 함께 그 인스턴스와 관련된 저장 프로퍼티의 값 등을 저장합니다.
- 그 후에 인스턴스가 더 이상 필요 없는 상태가 되면 인스턴스가 차지하던 메모리 공간을 다른 용도로 활용할 수 있도록 ARC가 메모리에서 인스턴스를 없앱니다.
- 그런데 만약 아직 더 사용해야 하는 인스턴스를 메모리에서 해제시킨다면 인스턴스와 관련된 프로퍼티에 접근하거나 인스턴스의 메서드를 호출할 수 없습니다.
- 게다가 인스턴스에 강제로 접근하려 한다면 잘못된 메모리 접근으로 인해 프로그램이 강제 종료된 확률이 큽니다.
- 인스턴스가 지속해서 필요한 상황에서 ARC는 인스턴스가 메모리에서 해제되지 않도록 인스턴스 참조 여부를 계속 추적합니다.
- 다른 인스턴스의 프로퍼티나 변수, 상수 등 어느 한 곳에서 인스턴스를 참조한다면 ARC가 해당 인스턴스를 해제하지 않고 유지해야 하는 명분이 됩니다.
- 인스턴스를 메모리에 유지시키려면 이런 명분을 ARC에 제공해야 한다는 것을 명심해야 합니다.

(1) 강한참조
- 인스턴스가 계속해서 메모리에 남아있어야 하는 명분을 만들어주는 것이 바로 강한참조(Strong Reference)입니다.
- 인스턴스는 참조 횟수가 0이 되는 순간 메모리에서 해제되는데, 인스턴스를 다른 인스턴스의 프로퍼티나 변수, 상수 등에 할당할 때 강한 참조를 사용하면 참조 횟수가 1 증가합니다.
- 또, 강한 참조를 사용하는 프로퍼티, 변수, 상수 등에 nil을 할당해주면 원래 자신에게 할당되어 있던 인스턴스의 참조 횟수가 1 감소합니다.
- 참조의 기본은 강한참조이므로 클래스 타입의 프로퍼티, 변수, 상수 등을 선언할 때 별도의 식별자를 명시하지 않으면 강한참조를 합니다.
- 이제까지 우리는 알지 못하고 써왔지만 프로퍼티와 변수, 상수를 모두 강한참조로 선언해주었던 것입니다.
- (코드) 강한참조의 참조 횟수 확인
```swift
class Person {
  let name: String
  
  init(name: String) {
      self.name = name
      print("\(name) is being initialized")
  }
  
  deinit {
      print("\(name) is being deinitialized")
  }
}

var reference1: Person?
var reference2: Person?
var reference3: Person?

reference1 = Person(name: "yagom")
// yagom is being initialized
// 인스턴스의 참조 횟수: 1

reference2 = reference1 // 인스턴스의 참조 횟수: 2
reference3 = reference1 // 인스턴스의 참조 횟수: 3

reference3 = nil // 인스턴스의 참조 횟수: 2
reference2 = nil // 인스턴스의 참조 횟수: 1
reference1 = nil // 인스턴스의 참조 횟수: 0
// yagom is deinitailized

```
- reference1에 할당된 Person 클래스 타입의 인스턴스는 처음 메모리에 생성된 후 강한참조로 reference1에 할당되기 때문에 참조 횟수가 1 증가합니다.
- 그 후 reference2에 강한참조로 할당되기 때문에 참조 횟수가 1 더 증가합니다.
- 마찬가지로 이 인스턴스가 reference3에 할당될 때도 참조 횟수가 1 증가합니다.
- 하나의 인스턴스가 세 개의 변수에 강한참조로 참조하고 있는 것이죠. 따라서 계속 메모리에 살아있을 명분이 충분합니다.
- 다음은 해제입니다. 마지막에 참조되었던 reference3에서 제일 먼저 인스턴스 참조를 그만두었습니다.
- 그러면 인스턴스의 참조 횟수는 1 감소하여 2가 됩니다.
- 마찬가지로 reference2와 reference1에서 순차적으로 참조를 그만두면 참조 횟수가 0이 됩니다.
- 참조 횟수가 0이 되는 순간 인스턴스는 ARC의 규칙에 의해 메모리에서 해제되며 메모리에서 해제되기 직전에 디이니셜라이저를 호출합니다.


- (코드) 강한참조 지역변수(상수)의 참조 횟수 확인
```swift
func foo() {
  let yagom: Person = Person(name: "yagom") // yagom is being initialized
  // 인스턴스의 참조 횟수: 1
  
  // 함수 종료 시점
  // 인스턴스의 참조 횟수: 0
  // yagom is being initialized
}
foo()
```
- foo()라는 함수 내부에 yagom이라 정의한 강한참조 상수가 있습니다.
- Person 타입의 인스턴스는 이니셜라이저에 의해 생성된 후 yagom 상수에 할당할 때 참조 횟수가 1이 됩니다.
- 그리고 강한참조 지역변수(상수)가 사용된 범위의 코드 실행이 종료되면 그 지역변수(상수)가 참조하던 인스턴스의 참조 횟수가 1 감소합니다.
- 그래서 yagom 상수가 강한참조하던 인스턴스의 참조 횟수가 1 감소하여 인스턴스의 참조 횟수가 0이 됩니다.
- 인스턴스의 참조 횟수가 0이 되는 순간 인스턴스는 메모리에서 해제됩니다.

- (코드) 강한참조 지역변수(상수)의 참조 횟수 확인과 전역변수
```swift
var globalReference: Person?

func foo() {
  let yagom: Person = Person(name: "yagom") // yagom is being initialized
  // 인스턴스의 참조 횟수: 1
  
  globalReference = yagom // 인스턴스의 참조 횟수: 2
  
  // 함수 종료 시점
  // 인스턴스의 참조 횟수: 1
}
foo()
```
- 인스턴스가 foo() 함수 내부에서 생성된 후 강한참조로 yagom 상수에 참조된 것은 앞서 본 강한참조 지역변수(상수)의 참조 횟수 확인과 크게 다르지 않습니다.
- 그런데 이번엔 인스턴스가 강한참조를 하는 전역변수 globalReference에 강한참조되면서 참조 횟수가 1 더 증가하여 2가 되었습니다.
- 그 상태에서는 함수가 종료되면 참조 횟수가 1 감소하여도 여전히 참조 횟수가 1이므로 메모리에서 해제되지 않습니다.


(2) 강한참조 순환 문제
- 복합적으로 강한참조가 일어나는 상황에서 강한참조의 규칙을 모르고 사용하게 되면 문제가 발생할 수 있습니다.
- 인스턴스끼리 서로가 서로를 강한참조할 때를 대표적인 예로 들 수 있는데, 이를 강한참조 순환(Strong Reference Cycle)이라고 합니다.
- (코드) 강한참조 순환 문제
```swift
class Person {
  let name: String
  
  init(name: String) {
    self.name = name
  }
  
  var room: Room?
  
  deinit {
    print("\(name) is being deinitialized")
  }
}

class Room {
  let number: String
  
  init(number: String) {
    self.number = number
  }
  
  var host: Person?
  
  deinit {
    print("Room \(number) is being deinitialized")
  }
}

var yagom: Person? = Person(name: "yagom") // Person 인스턴스의 참조 횟수: 1
var room: Room? = Room(number: "505")

room?.host = yagom // Person 인스턴스의 참조 횟수: 2
yagom?.room = room // Room 인스턴스의 참조 횟수: 2

yagom = nil // Person 인스턴스의 참조 횟수: 1
room = nil // Room 인스턴스의 참조 횟수: 1

// Person 인스턴스를 참조할 방법 상실 - 메모리에 잔존
// Room 인스턴스를 참조할 방법 상실 - 메모리에 잔존
```
- 두 클래스 모두 클래스 정의 다음 코드를 보면 yagom은 Person? 타입의 변수고, room은 Room? 타입의 변수입니다.
- 각 변수에 맞는 타입으로 Person 클래스의 인스턴스와 Room 클래스의 인스턴스가 각각 메모리에 할당될 때 강한참조를 하므로 참조 횟수가 1씩 증가합니다.
- 두 인스턴스 모두 참조 횟수가 1인 상태에서 room이 참조하는 Room 클래스 인스턴스의 저장 프로퍼티인 host 프로퍼티에 변수 yagom이 참조하는 Person 클래스 인스턴스를 할당합니다.
- 이때 host 프로퍼티는 Room 클래스에 정의된 대로 강한참조를 하므로 변수 yagom이 참조하는 Person 클래스 인스턴스는 참조 횟수가 1 증가하여 2가 됩니다.
- 마찬가지로 yagom이 참조하는 Person 클래스 인스턴스의 저장 프로퍼티인 room 프로퍼티에 변수 room이 참조하는 Room 클래스 인스턴스를 할당하면 room 프로퍼티는 강한참조를 하므로
- 변수 room이 참조하는 Room 클래스 인스턴스는 참조 횟수가 1 증가하여 2가 됩니다.
- 서로 강한참조를 하는 상태에서 yagom 변수에 nil을 할당하면 yagom이 참조하는 인스턴스의 참조 횟수는 1 감소하여 참조 횟수가 1이 됩니다.
- 그렇지만 이제 yagom이 참조하던 인스턴스를 참조할 방법은 변수 room이 참조하는 인스턴스의 host 프로퍼티로 접근하는 방법밖에 남아 있지 않습니다.
- 다행히도 room 변수가 아직 그 인스턴스를 강한참조로 붙들고 있기 때문에 인스턴스는 메모리에서 해제되지 않은 상황입니다.
- 그렇지만 불행은 변수 room에 nil을 할당해주었을 때 일어납니다.
- room 변수가 참조하던 인스턴스는 참조 횟수가 1 감소하고 최종적으로 참조 횟수가 1이 됩니다.
- 그렇지만 이제 yagom 변수가 참조하던 Person 클래스의 인스턴스에 접근할 방법도, room 변수가 참조하던 Room 클래스의 인스턴스에 접근할 방법도 사라졌습니다.
- 참조 횟수가 0이 되지 않는 한, ARC의 규칙대로라면 인스턴스를 메모리에서 해제시키지 않기 때문에 이렇게 두 인스턴스 모두 참조 횟수 1을 남겨둔 채, 메모리에 좀비처럼 남게 됩니다.
- 이를 메모리 누수가 발생했다고 합니다.
- 디이니셜라이저가 호출되지 않은 것을 보면 메모리에서 해제되지 않고 계속 남아 있다는 것을 알 수 있습니다.
- 이렇게 두 인스턴스가 서로를 참조하는 상황에서 강한참조 순환 문제가 발생할 수 있습니다.

- (코드) 강한참조 순환 문제를 수동으로 해결
```swift
var yagom: Person? = Person(name: "yagom") // Person 인스턴스의 참조 횟수: 1
var room: Room? = Room(number: "505") // Room 인스턴스의 참조 횟수: 1

room?.host = yagom // Person 인스턴스의 참조 횟수: 2
yagom?.room = room // Room 인스턴스의 참조 횟수: 2

yagom?.room = nil // Room 인스턴스의 참조 횟수: 1
yagom = nil // Person 인스턴스의 참조 횟수: 1

room?.host = nil // Person 인스턴스의 참조 횟수: 0
// yagom is being deinitialized

room = nil // Room 인스턴스의 참조 횟수: 0
// Room 5050 is being deinitialized
```
- 변수 또는 프로퍼티에 nil을 할당하면 참조 횟수가 감소한다는 규칙을 생각하면 위와 같은 방법으로 인스턴스를 메모리에서 해제시킬 수 있습니다.
- 그렇지만 실수를 하게 되거나 해제해야 할 프로퍼티가 너무 많은 경우에는 어떻게 해야 할까요?
- 이럴 때는 약한참조와 미소유참조를 통해 문제를 해결할 수 있습니다.
