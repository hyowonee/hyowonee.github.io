---
title: Delegate Pattern
author: Choi Hyowon
---
# Delegate Pattern이란?
swift에서는 interface를 protocol이라고 한다.
```swift
protocol TableViewDelegate {
    func execute() {
        print("foo!!")
    }
}
class Delegate2: TableViewDelegate { // delegate protocol을 상속받는다.
    func execute() {
        print("bar!!!")
    }
}
class TableView {
    var delegate: TableViewDelegate? // protocol을 멤버번수로 받는다.
    func executedelegate() {
        delegate?.execute() // delegate?가 nil이 아니면 받은 delegate의 메서드를 실행한다.
    }
}

let tableView = TableView()
tableView.delegate = Delegate2() // print("bar!!!")
tableView.executedelegate() // print("foo!!")
```

