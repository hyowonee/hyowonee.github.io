---
title: Reference Counting
author: Choi Hyowon
---
Reference Counting은 메모리에 할당된 객체에 참조중인 개수를 세는 것이다. 
-> *특정 객체에 얼마나 많은 참조가 있는지를 숫자로 세는 것이다.*

# 메모리 관리 방식
## MRC
* 수동 레퍼런스 카운트(MRC, Manual Reference Count)
* 말 그대로 개발자가 수동으로 Reference Counting을 하는 것 이다.
* Objective-c에서만 지원하며 Swift에서는 지원하지 않는다.
* 객체를 참조하고 싶을 때 [retain] // 참조를 풀고 싶을 때 [release]
```objective-c
Person *person = [[Person alloc] init]; // RC : 1
[person retain]; // RC : 2
[person release]; // RC : 1
[person release]; // RC : 0
```
> Reference Count가 0이되면 dealloc이 자동으로 호출되어 person 객체는 완전히 소멸된다.
* 만약 어떤 메소드에서 retain이 되어 release를 해주는데 만약 값을 return 해줘야 한다면? 이미 release를 통해 Reference Count가 0이 되어 객체가 이미 죽어있게 된다. 그럴경우, 어쩔 수 없이 return 값을 받는 곳에서 release를 해줘야 하는데 이럴 경우 코드가 복잡해진다. 이 때 사용할 수 있는 것이 Autorelease이다.

### Autorelease
* Autorelease는 AutoreleasePool에 release가 필요한 객체들을 보관한다.
* 어떤 시점이 되면 pool애서 drain이 되고 release가 된다.
* autorelease는 AutoreleasePool에 객체를 push 해놓고 release를 나중에 처리한다. push 된 객체는 pool에서 drain이 호출 될 때 release 되고 dealloc된다.
> 하지만  release를 쓰는 것보다 고비용이 들어 autorelease는 쓰지 않는 것이 좋다고 한다.

## ARC
* 자동 레퍼런스 카운트(Automatic Reference Count)
* 말 그대로 자동으로 메모리 관리를 처리한다.

### ARC의 규칙
* Memory Method에 접근할 수 없다.
> retain/release/autorelease 등을 부를 수 없다.
> 컴파일러가 알아서 처리한다.
* C 구조체에 객체를 넣을 수 없다.
* id와 void 간에 Casual Casting을 할 수 없다.
> compiler는 void*가 retain 되었는지 알아야 한다.
> __bridge를 이용해서 casting을 할 수 있다. (__bridge, __bridge_retain 등)
* NSAutoreleasePool을 사용할 수 없다.
>  ARC가 아니더라도 @autoreleasepool 블럭을 사용해야 한다.

# 참조(Reference)
* 객체에 대한 레퍼런스를 정의할 때 추가적인 속성을 지정할 수 있다. 
*  strong, weak, unowned 세 가지 중에 하나를 선택할 수 있다. 

## 강한 참조(Strong Reference)
* strong가 디폴트 값이다. 즉, 별도의 지시자를 기술하지 않았을 때는 strong을 명시한 것과 동일한 효과이다
> 그 의미는 해당 레퍼런스에 대해 강한 참조(strong reference)를 유지 하겠다는 뜻이 된다. 
* 이렇게 강한 참조는 Reference Count를 증가/감소 시키고 대상 객체에 대한 레퍼런스를 유지하며 순환 레퍼런스 문제를 일으킬 수 있다.
* 강한 참조의 경우는 대상 객체에 대한 소유권을 가지는 것과 같아서 레퍼런스를 유지하는 한 대상 객체의 Reference Count가 0이 될 수 없어 메모리에서 해제되지 않는다.

## 약한 참조(Weak Reference)
* 약한 참조(weak)는 대상 객체에 대해 Reference Count를 변화시키지 않는다. 
> Reference Count를 증가 시키지 않으면서 대상 객체를 참조할 수 있다는 뜻이 된다. 
> unowned 는 기본적으로 weak 처럼 대상 객체의 레퍼런스 카운트에 영향을 주지 않는 참조를 만든다. 다만 다른 점은 대상 객체가 메모리에서 해제될 때 weak 의 경우에는 nil로 설정되지만 unowned 의 경우에는 값의 변화가 없다.
* 약한 참조의 경우에는 소유권을 가지지 않고 단순히 대상 객체를 참조하는 것이므로 대상 객체는 Reference Count가 0이 되는 순간 메모리에서 삭제된다. 
> 이 때 이 객체를 참조하고 있던 약한 참조 변수는 자동으로 nil이 되어버린다. 따라서 약한 참조는 옵셔널 변수에만 가능하다. (unowned의 경우에는 옵셔널이 될 수 없다.)
