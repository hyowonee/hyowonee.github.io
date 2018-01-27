---
title: ios 어플 실전 공부 1차
author: Choi Hyowon
---

#  CocoaPods란?
CocoaPod는 ios개발시 사용하는 의존성 프로토콜이다.

> 의존성 관리 툴이란 개발중인 프로젝트에 의존되는 Library(작은 기능들),  Framework(정해진 패턴만 사용해야함)들을 관리하는 것을 말한다.
설치 tip : 우선 ruby가 설치되어 있어야 한다.
		 sudo gem install cocoapods 명령어로 설치한다.

# 프로젝트에 라이브러리 적용하는 법
1.  Xcode 프로젝트가 있는 디렉토리로 이동한다.
2. `pod init` 명령어를 실행하여 Podfile 파일을 생성한다.
3. `pod install` 명령어를 실행한다.
4. *프로젝트이름.xcworkspace* 라는 파일이 생성된다.

# 외부 라이브러리 사용하는법
1. Pods 파일을 연다.
2. 사용할 타겟 프로젝트 내부에 `pod “라이브러리이름”, “(~> 버전)”` 을 입력한다.
```swift
target ‘프로젝트이름’ do
	use_frameworks!
	pod “라이브러리이름”, “(~>) 버전”
end
```
-> 문법은 ruby이다.
3. shell에서 `pod install` 명령어를 실행한다.
4. 나중에 library update가 필요하게 되면, shell에서 `pop update`를 실행한다.
5. Library가 설치되면 Podfile.lock파일이 생성된다. 이 파일이 생성되면 library가 중복해서 설치되지 않는다.
6. .swift 코드에서는 `import 라이브러리이름` 으로 라이브러리를 import하고 프로젝트를 clean, build 해준다.
7. shell에서 내가 쓰려는 라이브러리가 어떤 라이브러리인지 시험해보고 싶으면 shell에서 `pod try 라이브러리이름` 로 시험해볼 수 있다. 

# IOS에서 alert창 띄우는법
```swift
@IBAction func showAlert() {
	// UIAlertController를 생성해야 한다. style은 .alert로 해준다.
	let alertController = UIAlertController(title: "제목", message: "내용", preferredStyle: .alert)
	// style이 .cancle이면 bold체. handler가 nil일 경우에는 아무 일이 일어나지 않고 닫힌다.
	alertController.addAction(UIAlertAction(title: "취소", style: .cancle, handler: nil)
	// style이 .default면 평범하다. 해당 버튼을 누르면 handler의 코드가 실행된다.
	alertController.addAction(UIAlertAction(title: "확인", style: .default, handler: {alertAction in print("확인")}))
	// style이 .destructive면 빨간색 text color
	alertController.addAction(UIAlertAction(title: "삭제", style: .destructive, handler: {alertAction in print("삭제")}))
	// 실제로 화면에 보여주기 위해서는 present 메서드가 필요하다. animated : true/false로 해놓으면 애니메이션 효과가 있고/없다. present가 완료되어 화면이 보여지면 completion의 코드가 실행된다.
	self.present(alertController, animated: true, completion: nil)
}
```