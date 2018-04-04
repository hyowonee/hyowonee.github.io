---
title: SharedPreference
author: Choi Hyowon
---
# SharedPreference이란?
* 저장하고자 하는 키-값 컬렉션이 비교적 작은 경우, SharedPreferences API를 사용한다.
* SharedPreferences 객체는 키-값 쌍을 포함하는 파일을 가리키며, 키-값 쌍을 읽고 쓸 수 있는 간단한 메서드를 제공한다.
* SharedPreferences는 내부적으로는 XML 파일로 저장이 된다.
* 각 SharedPreferences 파일은 프레임워크로 관리되며, 전용 또는 공유일 수 있다.

## swift 코드
```swift
class ViewController: UIViewController {

  let currentKey = "current"
  let didSave = UserDefaults.standard.synchronize()
  override func viewDidLoad() {
    super.viewDidLoad()
    // writing
        UserDefaults.standard.set(1, forKey: "key")
    // reading
    if UserDefaults.standard.object(forKey: "key") == nil {
        //  Doesn't exist
    } else {
        _ = UserDefaults.standard.integer(forKey: "key")
    }
  }
}
```