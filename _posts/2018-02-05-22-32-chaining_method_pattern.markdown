---
title: Method Chaining Pattern
author: Choi Hyowon
---
# Method Chaining Pattern
```swift
import Foundation

class Chain {
    var width: Int = 0
    var height: Int = 0
    var weight: Int = 0
    
    func setWidth(width: Int) -> Chain {
        self.width = width
        return self
    }
    
    func setHeight(height: Int) -> Chain {
        self.height = height
        return self
    }
    
    func setWeight(weight: Int) -> Chain {
        self.weight = weight
        return self
    }
    
    func print() {
        Swift.print("Width: \(width), Height: \(height), Weight: \(weight)")
    }
}
 
let chain = Chain()

// 메서드 체이닝 패턴을 안쓸때
chain.width = 100
chain.height = 100
chain.width = 100
chain.print()

// 메서드 체이닝 패턴을 쓸때
chain.setWidth(width: 100)
    .setHeight(height: 100)
    .setWeight(weight: 100)
    .print()
```