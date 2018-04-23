---
title: iOS에서 Login Page 만들기(storyboard)
author: Choi Hyowon
---
* 아이디와 비밀번호를 입력하여 로그인을 하는 페이지를 만들어 보았다.
* storyboard에 UILabel, UITextField, UIButton을 연결하여 사용하였다.
* 아이디는 이메일 형식이어야 하고, 비밀번호는 숫자 + 영어 8글자 이상이 아닐 경우 검증 실패 alert를 띄웠다.
* 아이디와 비밀번호에 정규표현식을 사용하여 검증 하였다.
* 아이디: hyowon@test.com, 비밀번호: 1hyowon2일 경우에는 로그인하면 성공 alert가 뜨게 하였다.

## storyboard
![Image](/images/LoginPage_mainLabel.png)
![Image](/images/LoginPage_id_textField.png)
![Image](/images/LoginPage_password_textField.png)
mainLabel, id(email), password를 storyboard와 연결하였다.
![Image](/images/LoginPage_loginButton.png)
loginButton을 누르면 실행되는 login()함수를 storyboard와 연결하였다.

## ViewController.swift
```swift
class ViewController: UIViewController {

    @IBOutlet var mainLabel: UILabel!
    @IBOutlet var loginButton: UIButton!
    @IBOutlet var emailField: UITextField!
    @IBOutlet var passwordField: UITextField!
    
    // 상황에 따라 다른 alert를 줄 때 바꿔줄 문장을 tmpMessage로 설정
    var tmpMessage: String = ""
    
    override func viewDidLoad() {
        super.viewDidLoad()
        loginButton.layer.cornerRadius = 10
    }
    
    @IBAction func login() {
        guard let emailText = emailField.text else {
            return
        }
        
        guard let passwordText = passwordField.text else {
            return
        }
        
        // id(email)와 password의 형식이 둘다 올바른 경우
        if isValidEmailAddress(email: emailText) && isVailedPassword(password: passwordText) {
            // 먼저 정의해둔 id(email)과 password로 로그인 한 경우
            if emailText == "hyowon@test.com" && passwordText == "1hyowon2" {
                successAlert()
            } else { // 형식은 지켰으나 정의해둔 id(email), password가 아닌 경우
                joinAlert()
            }
        }
        
        // id(email) 형식이 올바르지 않은 경우
        if !isValidEmailAddress(email: emailText) {
            tmpMessage = "이메일을"
            failedAlert()
        }
        
        // password 형식이 올바르지 않은 경우
        if !isVailedPassword(password: passwordText) {
            tmpMessage = "패스워드를"
            failedAlert()
        }
        
    }
    
    // alert 설정
    func failedAlert() {
        let alertController = UIAlertController(title: nil, message: "\(tmpMessage) 다시 입력해 주세요.", preferredStyle: .alert)
        alertController.addAction(UIAlertAction(title: "확인", style: .default, handler: nil))
        self.present(alertController, animated: true, completion: nil)
    }
    
    func successAlert() {
        let alertController = UIAlertController(title: nil, message: "로그인에 성공하였습니다!", preferredStyle: .alert)
        alertController.addAction(UIAlertAction(title: "확인", style: .default, handler: nil))
        self.present(alertController, animated: true, completion: nil)
    }
    
    func joinAlert() {
        let alertController = UIAlertController(title: nil, message: "입력하신 내용으로 회원가입을 해주세요!", preferredStyle: .alert)
        alertController.addAction(UIAlertAction(title: "확인", style: .default, handler: nil))
        self.present(alertController, animated: true, completion: nil)
    }
    
    // 정규표현식을 사용하여 id(email) 형식이 맞는지 검증
    func isValidEmailAddress(email: String) -> Bool {
        let emailRegEx = "[A-Z0-9a-z._%+-]+@[A-Za-z0-9.-]+\\.[A-Za-z]{2,}"
        let emailTest = NSPredicate(format:"SELF MATCHES %@", emailRegEx)
        return emailTest.evaluate(with: email)
    }

    // 정규표현식을 사용하여 password 형식이 맞는지 검증 (영+숫자 8글자 이상)  
    func isVailedPassword(password: String) -> Bool {
        let passwordRegEx = "^[a-zA-Z0-9]{8,}$"
        let passwordTest = NSPredicate(format:"SELF MATCHES %@", passwordRegEx)
        return passwordTest.evaluate(with: password)
    }
}
```
## 결과
![Image](/images/LoginPage_login_error_id.png)
![Image](/images/LoginPage_login_error_id_alert.png)
![Image](/images/LoginPage_login_error_password.png)
![Image](/images/LoginPage_login_error_password_alert.png)
![Image](/images/LoginPage_login_join.png)
![Image](/images/LoginPage_login_join_alert.png)
![Image](/images/LoginPage_login_success.png)
![Image](/images/LoginPage_login_success_alert.png)