---
title: iOS에서 LocalNotification으로 Alarm 띄우기
author: Choi Hyowon
---
* UITextField에 사용자가 정해진 형식대로 시간을 입력하면, 해당 시간에 Notification이 뜨도록 하였다.
* UITextField 위에는 현재 시간을 실시간으로 보여주는 Label를 두었다. Timer()와 DateFormatter()를 사용하였다.
* 현재 시간을 가져오는 것은 Date()와 Calender를 이용하였다.

## ViewController.swift
```swift
class ViewController: UIViewController {

    @IBOutlet var timeTextField: UITextField!
    @IBOutlet var realTimeLabel: UILabel!
    let dateFormatter = DateFormatter()
    var currentDateTime = Date()
    var timer = Timer()
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        // 1초 간격으로 실시간으로 realTimeLabel을 업데이트 시키는 tick 함수를 실행
        self.timer = Timer.scheduledTimer(timeInterval: 1.0, target: self, selector:#selector(self.tick) , userInfo: nil, repeats: true)
        
        // 권한 체크
        UNUserNotificationCenter.current().requestAuthorization(options: [.alert, .sound, .badge], completionHandler: {didAllow, error in
        })
        
        // UITextfield를 입력하다 다른 아무 곳을 선택하면 기기의 키보드 입력창이 내려가도록 설정
        self.dismissKeyboard()
    }
    
    @objc func tick() {
        realTimeLabel.text = DateFormatter.localizedString(from: Date(), dateStyle: .none, timeStyle: .medium)
    }
    
    @IBAction func sendNotification() {

        // 사용자의 시간 입력 받아오기
        guard let timeText = timeTextField.text else {
            return
        }
        
        let date = Date()
        let calendar = Calendar.current
        let realHour = calendar.component(.hour, from: date)
        let realMinute = calendar.component(.minute, from: date)
        let realSecond = calendar.component(.second, from: date)
        
        let time = timeText.split(separator: ":").map { val -> Int in
            return Int(val)!
        }
        
        // 현재 시간과 사용자에게 입력받은 시간 차를 계산
        let timeDiffer = Double((time[0] - realHour) * 3600 + (time[1] - realMinute) * 60 + (time[2] - realSecond))
        print("timeDiffer : \(timeDiffer)")
        let content = UNMutableNotificationContent() // 노티피케이션 메세지 객체
        content.title = NSString.localizedUserNotificationString(forKey: "알림!", arguments: nil)
        content.body = NSString.localizedUserNotificationString(forKey: "\(time[0]):\(time[1]) 입니다!", arguments: nil)

        let trigger = UNTimeIntervalNotificationTrigger(timeInterval: timeDiffer, repeats: false) // 얼마 후 실행?

        let request = UNNotificationRequest(
            identifier: "LocalNotification",
            content: content,
            trigger: trigger
        ) // 노티피케이션 전송 객체

        let center = UNUserNotificationCenter.current() // 노티피케이션 센터
        center.add(request) { (error : Error?) in // 노티피케이션 객체 추가 -> 전송
            if let theError = error {
                print(theError.localizedDescription)
            }
        }
        print("yes!!")
    }
    
    @IBAction func dismissKeyboard() {
        view.endEditing(true)
    }
    
}
```
### dismissKeyboard()에 대해..
* UITextField에 값을 입력하고 나서도 키보드 입력창이 내려가지 않는 문제가 발생하였다.
* `view.endEditing(true)`라는 코드로 해결하였다.
* 코드와 함께 다음의 storyboard 설정이 필요하다.

![Image](/images/dismissKeyboard_UIControl.png)
상위 View의 identity inspector의 Custom Class의 이름을 UIControl로 설정해준다.
![Image](/images/dismissKeyboard_sent_events.png)
상위 View의 connections inspector의 Sent Events에서 dismissKeyboard()함수를 연결해준다.

## 결과
![Image](/images/LocalNotification_require_author.png)
실시간 시간을 보여주는 첫 화면
![Image](/images/LocalNotification_input.png)
시간 입력
![Image](/images/LocalNotification_output.png)
입력한 시간에 알람이 뜸