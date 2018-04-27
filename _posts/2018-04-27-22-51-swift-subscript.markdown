---
title: Swift Subscript 문법
author: Choi Hyowon
---
* 클래스, 구조체, 열거형에는 컬렉션, 리스트, 시퀀스 등 타입의 요소에 접근하는 단축 문법인 Subscript를 정의할 수 있다.
* Subscript는 별도의 설정자(Setter) 또는 접근자(Getter) 등의 메서드를 구현하지 않아도 인덱스를 통해 값을 설정하거나 가져올 수 있다.
> 예를 들어, tmpArray라는 Array 인스턴스의 index를 통해 해당 인덱스의 값에 접근할 때는 tmpArray[index]라고 표현한다.
> tmpDictionary라는 Dictionary의 key를 통해 해당 키의 값을 가져올 때는 tmpDictionary[key]로 표현한다. 
* 한 타입에 여러 개의 Subscript를 정의할 수 있고, 다른 타입을 인덱스로 갖는 여러 개의 Subscript를 중복 정의(overload)할 수도 있다.

## Subscript 문법
* Subscript는 인스턴스의 이름 뒤에 대괄호로 감싼 값을 써줌으로써 인스턴스 내부의 특정값에 접근할 수 있다.
* Subscript는 subscript 키워드를 사용하여 정의한다.
* 인스턴스 메서드와 비슷하게 매개변수의 개수, 타입, 반환 타입 등을 지정하며, 읽고 쓰기가 가능하도록 구현하거나 읽기 전용으로만 구현할 수 있다.
*  Subscript를 정의하는 코드는 각 타입의 구현부 또는 타입의 extension 구현부에 위치해야 한다.

```swift
subscript(index: Int) -> Int {
  get {
    // 적절한 Subscript 결괏값 반환
  }
  
  set(newValue) {
    // 적절한 설정자 역할 수행
  }
}
```
> 위 코드의 Subscript 설정자의 newValue의 타입은 Subscript의 반환 타입과 동일하다.

```swift
// 읽기 전용 Subscript
subscript(index: Int) -> Int {
  get {
    // 적절한 Subscript 값 반환
  }
}

subscript(index: Int) -> Int {
  // 적절한 Subscript 결괏값 반환
}
```
> 읽기 전용 Subscript를 구현할 때는 get이나 set 키워드를 사용하지 않고 적절한 값만 반환해주는 형태로 구현해도 된다.

## 여러 개의 Subscript
* 하나의 타입이 여러 개의 Subscript를 가질 수도 있다.
* 다양한 매개변수 타입을 사용하여 Subscript를 구현하면 여러 용도로 Subscript를 사용할 수 있다.

```swift
class Hello {
    var _name: String
    
    init(name: String) {
        self._name = name
    }
    
    subscript(msg: String) -> String {
        return "(_name)님, (msg)"
    }
    
    subscript(a: Int, b: Int) -> Int {
        return a * b
    }
}

var hello = Hello(name: "Hyowon")
print(e["안녕하세요"])
print(e[100,7])
```