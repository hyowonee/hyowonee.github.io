---
title: Observer Pattern
author: Choi Hyowon
---
# Observer Pattern이란?
* Subscriber-Publisher 패턴이라고도 한다.
* Swift로 간단한 observer pattern을 구현해 보았다.

```swift
protocol Subscriber { // Subscriber은 Publisher에 의존한다.
	func receive()
}

protocol Publisher { // Publisher는 Subscriber에게 이벤트를 전달한다.
	func add(subscriber: Subscriber)
	func delete(index: Int)
	func publish()
}

class NewsSubscriber: Subscriber { // Subscriber protocol을 상속받아서 method override를 해준다.
	func receive() {
		print("I got it!") 	// Publisher로부터 받은 이벤트를 처리하는 부분
	}
}

class NewsSubscriber2: Subscriber {
	func receive() {
		print("받았어요~") // 이벤트 내용을 다르게 바꿀 수 있다.
	}
}

class NewsPublisher: Publisher { // Publisher protocol을 상속받아서 method override를 해준다.
	var subscribers: [Subscriber] = []
	
	func add(subscriber: Subscriber) { // Subscriber을 추가할 수 있다.
		self.subscribers.append(subscriber) 
	}

	func delete(index: Int) { // Subscriber을 삭제할 수 있다.
		self.subscribers.remove(at: index)
	}

	func publish() { // Subscriber들에게 이벤트를 전달하는 method
		print("발행 시작합니다.")
		for subscriber in subscribers {
			subscriber.receive()
		}
	}
}

let newsPublisher = NewsPublisher() // NewsPublisher 추가
newsPublisher.add(subscriber: NewsSubscriber()) // Subscriber 추가
newsPublisher.add(subscriber: NewsSubscriber2()) // Subscriber 추가
newsPublisher.add(subscriber: NewsSubscriber()) // Subscriber 추가

print("Publisher 생성")
print("Publish 시작")
newsPublisher.publish()
```
## 결과
![Image](/images/observer_pattern_result.png)
