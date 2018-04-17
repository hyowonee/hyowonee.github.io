---
title: iOS에서 facebook 로그인/로그아웃하기
author: Choi Hyowon
---
다음의 링크를 참조하여 간단하게 버튼을 누르면 facebook 로그인 및 로그아웃을 할 수 있는 앱을 만들어 보았다.
https://developers.facebook.com/docs/facebook-login/ios

1. 위 링크에서 페이스북 로그인 기능을 이용할 앱을 선택하고 개발환경을 설정한다.
2. Podfile에 `pod 'FBSDKLoginKit'`을 추가한다.
3. 위 링크의 facebook 번들 ID에 `hyowonee.FacebookLoginExample`을 추가한다.
4. 프로젝트의 info.plist를 source code로 열고, 위 링크를 참조하여 다음 이미지와 같이 코드를 붙여넣는다.
![Image](/images/facebookLogin_info_plist.png)
5. 프로젝트의 AppDelegate class에 위 링크를 참조하여 다음 이미지와 같이 코드를 붙여넣는다.
![Image](/images/facebookLogin_AppDelegate.png)
6. ViewController에 `import FSBDKLoginKit`를 해주고 로그인 버튼을 다음과 같이 만들어준다.

## ViewController.swift
```swift
class ViewController: UIViewController {
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        let loginButton = FBSDKLoginButton() // 버튼 생성
        loginButton.center = self.view.center // 가운데로 위치시킴
        loginButton.readPermissions = ["public_profile", "email"] // 권한 요청
        self.view.addSubview(loginButton)
        
        FBSDKProfile.loadCurrentProfile { (profile, error) in // 예외처리
            guard let profile = profile else {
                print(error)
                return
            }
            
            print("\(profile.firstName) \(profile.lastName)")
        }
    }

}
```