---
title: iOS에서 Login Page 만들기(without storyboard)
author: Choi Hyowon
---
* 지난번에 한 아이디와 비밀번호를 입력하여 로그인을 하는 페이지를 Snapkit을 이용하고 storyboard의 사용 없이 만들어 보았다.
* 아이디는 이메일 형식이어야 하고, 비밀번호는 숫자 + 영어 8글자 이상이 아닐 경우 검증 실패 alert를 띄웠다.
* 아이디와 비밀번호에 정규표현식을 사용하여 검증 하였다.
* 아이디: hyowon@test.com, 비밀번호: 1hyowon2일 경우에는 로그인하면 바로 세팅창으로 넘어가게 하였다.
* UITapGestureRecognizer와 imagePickerController를 이용하여 tap으로 프로필과 배경 사진을 바꿀 수 있게 하였다.

## Appdelegate.swift
앱이 실행될때 가장 처음 진입되는 부분
```swift
class AppDelegate: UIResponder, UIApplicationDelegate {

    var window: UIWindow?

    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool {
        
        window = UIWindow(frame: UIScreen.main.bounds)
        let viewController = ViewController()
        // viewController -> SettingViewController로 이동하기 위한 navigationController
        // viewController가 navigationController의 rootController이다. 스택처럼 생각
        let navigationController = UINavigationController(rootViewController: viewController)
        
        window!.backgroundColor = .white
        window!.rootViewController = navigationController
        // 윈도우를 화면에 보여주는 method
        window!.makeKeyAndVisible()
        
        return true
    }

}
```

## ViewController.swift
```swift
class ViewController: UIViewController {
    
    // MARK: Properties

    var mainLabel = UILabel()
    var emailField = UITextField()
    var passwordField = UITextField()
    var loginButton = UIButton()
    var backgroundImage = UIImageView()
    var profileImage = UIImageView()
    var profileImagePicker = UIImagePickerController()
    var backgroundImagePicker = UIImagePickerController()
    
    // MARK: LifeCycle
    
    override func viewDidLoad() {
        super.viewDidLoad()
        self.view.addSubview(mainLabel)
        self.view.addSubview(emailField)
        self.view.addSubview(passwordField)
        self.view.addSubview(loginButton)
        self.view.addSubview(backgroundImage)
        self.view.addSubview(profileImage)
        
        self.mainLableLayout()
        self.emailFieldLayout()
        self.passwordFieldLayout()
        self.loginButtonLayout()
        self.backgroundImageLayout()
        self.profileImageLayout()
        self.tapImage(imageView: backgroundImage)
        self.tapImage(imageView: profileImage)
        
        self.loginButton.addTarget(self, action: #selector(self.login), for: .touchUpInside)
        
        self.title = "로그인"
    }
    
    // MARK: Layout Initializing

    func backgroundImageLayout() {
        backgroundImage.backgroundColor = .lightGray
        backgroundImage.snp.makeConstraints{ make in
            make.leading.equalTo(0)
            make.trailing.equalTo(0)
            make.top.equalTo(0)
        }
    }
    
    func profileImageLayout() {
        profileImage.layer.cornerRadius = (UIScreen.main.bounds.width - 220)/2
        profileImage.backgroundColor = .yellow
        profileImage.layer.masksToBounds = true // 영역 밖으로 나가면 잘라줌
        profileImage.snp.makeConstraints{ make in
            make.centerX.equalTo(self.view)
            make.leading.equalTo(110)
            make.trailing.equalTo(-110)
            make.top.equalTo(100)
        }
    }
    
    func mainLableLayout() {
        mainLabel.text = "효원 캠퍼스"
        mainLabel.textColor = .black
        mainLabel.sizeToFit()
        mainLabel.snp.makeConstraints{ make in
            make.centerX.equalTo(self.view)
            make.top.equalTo(200)
            make.top.equalTo(backgroundImage.snp.bottom).offset(20)
            make.top.equalTo(profileImage.snp.bottom).offset(0)
        }
    }
    
    func emailFieldLayout() {
        emailField.placeholder = "아이디를 입력하세요."
        emailField.keyboardType = .emailAddress
        emailField.autocapitalizationType = .none
        emailField.snp.makeConstraints { make in
            make.centerX.equalTo(self.view)
            make.leading.equalTo(50)
            make.trailing.equalTo(-50)
            make.top.equalTo(mainLabel.snp.bottom).offset(50)
        }
        
    }
    
    func passwordFieldLayout() {
        passwordField.placeholder = "비밀번호를 입력하세요."
        passwordField.isSecureTextEntry = true
        passwordField.autocapitalizationType = .none
        passwordField.snp.makeConstraints{ make in
            make.centerX.equalTo(self.view)
            make.leading.equalTo(50)
            make.trailing.equalTo(-50)
            make.top.equalTo(emailField.snp.bottom).offset(50)
        }
        underLineViewLayout(textField: self.passwordField)
    }
    
    func loginButtonLayout() {
        loginButton.setTitle("Login", for: .normal)
        loginButton.setTitleColor(.white, for: .normal)
        loginButton.backgroundColor = .black
        loginButton.layer.cornerRadius = 10
        loginButton.snp.makeConstraints{ make in
            make.centerX.equalTo(self.view)
            make.leading.equalTo(30)
            make.trailing.equalTo(-30)
            make.top.equalTo(passwordField.snp.bottom).offset(100)
        }
        underLineViewLayout(textField: self.emailField)
    }
    
    func underLineViewLayout(textField: UITextField) {
        let underLineView = UIView()
        self.view.addSubview(underLineView)
        underLineView.backgroundColor = .lightGray
        underLineView.snp.makeConstraints{ make in
            make.height.equalTo(0.5)
            make.leading.equalTo(48)
            make.trailing.equalTo(-70)
            make.top.equalTo(textField.snp.bottom).offset(12)
        }
    }
    
    func tapImage(imageView: UIImageView) {
        var tap = UITapGestureRecognizer()
        if imageView == backgroundImage {
            tap = UITapGestureRecognizer(target: self, action: #selector(changeBackgroundImage))
        } else {
            tap = UITapGestureRecognizer(target: self, action: #selector(changeProfileImage))
        }
        imageView.addGestureRecognizer(tap)
        imageView.isUserInteractionEnabled = true
    }
    
    @objc func changeBackgroundImage() {
        backgroundImagePicker.sourceType = .photoLibrary
        backgroundImagePicker.allowsEditing = false
        backgroundImagePicker.delegate = self
        present(backgroundImagePicker, animated: true)
    }
    
    @objc func changeProfileImage() {
        profileImagePicker.sourceType = .photoLibrary
        profileImagePicker.allowsEditing = false
        profileImagePicker.delegate = self
        present(profileImagePicker, animated: true)
    }
    
    @objc func login() {
        guard let emailText = emailField.text else {
            return
        }
        
        guard let passwordText = passwordField.text else {
            return
        }
        
        if isValidEmailAddress(email: emailText) && isVailedPassword(password: passwordText) {
            if emailText == "hyowon@test.com" && passwordText == "1hyowon2" {
                successAlert()
            } else {
                joinAlert()
            }
        }
        
        if !isValidEmailAddress(email: emailText) {
            failedAlert(message: "이메일을")
        }
        
        if !isVailedPassword(password: passwordText) {
            failedAlert(message: "패스워드를")
        }
        
    }
    
    func failedAlert(message: String) {
        let alertController = UIAlertController(title: nil, message: "\(message) 다시 입력해 주세요.", preferredStyle: .alert)
        alertController.addAction(UIAlertAction(title: "확인", style: .default, handler: nil))
        self.present(alertController, animated: true, completion: nil)
    }
    
    func successAlert() {
//        let alertController = UIAlertController(title: nil, message: "로그인에 성공하였습니다!", preferredStyle: .alert)
//        alertController.addAction(UIAlertAction(title: "확인", style: .default, handler: nil))
//        self.present(alertController, animated: true, completion: nil)
        
        let settingViewController = SettingViewController()
        self.navigationController?.pushViewController(settingViewController, animated: true)
    }
    
    func joinAlert() {
        let alertController = UIAlertController(title: nil, message: "입력하신 내용으로 회원가입을 해주세요!", preferredStyle: .alert)
        alertController.addAction(UIAlertAction(title: "확인", style: .default, handler: nil))
        self.present(alertController, animated: true, completion: nil)
    }
    
    func isValidEmailAddress(email: String) -> Bool {
        let emailRegEx = "[A-Z0-9a-z._%+-]+@[A-Za-z0-9.-]+\\.[A-Za-z]{2,}"
        let emailTest = NSPredicate(format:"SELF MATCHES %@", emailRegEx)
        return emailTest.evaluate(with: email)
    }
    
    func isVailedPassword(password: String) -> Bool {
        let passwordRegEx = "^[a-zA-Z0-9]{8,}$"
        let passwordTest = NSPredicate(format:"SELF MATCHES %@", passwordRegEx)
        return passwordTest.evaluate(with: password)
    }
}

extension ViewController: UIImagePickerControllerDelegate, UINavigationControllerDelegate {
    func imagePickerController(_ picker: UIImagePickerController, didFinishPickingMediaWithInfo info: [String : Any]) {
        var newImage: UIImage? = nil
        
        if let possibleImage = info["UIImagePickerControllerEditedImage"] as? UIImage {
            newImage = possibleImage
        } else if let possibleImage = info["UIImagePickerControllerOriginalImage"] as? UIImage {
            newImage = possibleImage
        }
        
        if picker == profileImagePicker {
            profileImage.image = newImage
            picker.dismiss(animated: true)
        } else if picker == backgroundImagePicker {
            backgroundImage.image = newImage
            picker.dismiss(animated: true)
        }
    }
}
```
## SettingViewController.swift
```swift
class SettingViewController: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()
        
        self.title = "Setting"
        // 뒤로 가기 버튼을 없애준다.
//        self.navigationItem.hidesBackButton = true
    }
}
```
## 결과
![Image](/images/loginPage_without_storyboard_default_page.png)
첫 화면

![Image](/images/loginPage_without_storyboard_default_page_change_images.png)
탭으로 프로필 이미지와 배경 이미지를 바꾼 화면

![Image](/images/loginPage_without_storyboard_login_success.png)
