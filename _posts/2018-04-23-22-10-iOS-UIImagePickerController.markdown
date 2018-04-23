---
title: iOS에서 UIImagePickerController로 이미지 가져오기
author: Choi Hyowon
---
* UIImagePickerController를 이용하여 사진 라이브러리에서 이미지를 가져온다.
* 버튼을 누르면 앨범이 뜨고, 이미지를 선택하면 해당 이미지를 가져온다.

## ViewController.swift
```swift
class ViewController: UIViewController {
    
    var imageView = UIImageView()
    // storyboard에서 만들지 않았기 때문에 type을 명시해줘야 한다.
    var button = UIButton(type: .system)
    let picker = UIImagePickerController()

    override func viewDidLoad() {
        super.viewDidLoad()
        
        // 기본 imageView의 배경색과 크기 설정
        self.imageView.frame = CGRect(x: 0, y: 0, width: UIScreen.main.bounds.size.width, height: 500)
        self.imageView.backgroundColor = .gray
        
        // 버튼의 크기와 title을 설정하고 버튼을 누르면 pickImage 함수가 호출된다.
        self.button.frame = CGRect(x: 0, y: 510, width: UIScreen.main.bounds.size.width, height: 50)
        self.button.setTitle("이미지 선택", for: .normal)
        self.button.titleLabel?.textAlignment = .center
        self.button.addTarget(self, action: #selector(self.pickImage), for: .touchUpInside)
        
        self.view.addSubview(self.imageView)
        self.view.addSubview(self.button)
        
        self.picker.sourceType = .photoLibrary // 방식 선택. 앨범에서 가져오는걸로 선택.
        self.picker.allowsEditing = false // 수정가능하게 할지 선택. 하지만 false
        self.picker.delegate = self // picker delegate
    }
    
    @objc func pickImage() {
        self.present(self.picker, animated: true) // Controller이기 때문에 present 메서드를 이용해서 컨트롤러 뷰를 띄워준다!
    }
    
}

extension ViewController: UIImagePickerControllerDelegate, UINavigationControllerDelegate {
    
    func imagePickerController(_ picker: UIImagePickerController, didFinishPickingMediaWithInfo info: [String : Any]) {
        var newImage: UIImage? = nil
        
        if let possibleImage = info["UIImagePickerControllerEditedImage"] as? UIImage { // 수정된 이미지가 있을 경우
            newImage = possibleImage
        } else if let possibleImage = info["UIImagePickerControllerOriginalImage"] as? UIImage { // 오리지널 이미지가 있을 경우
            newImage = possibleImage
        }
        
        imageView.image = newImage // 받아온 이미지를 이미지 뷰에 넣어준다.
        
        picker.dismiss(animated: true) // 그리고 picker를 닫아준다.
    }
    
}
```
### UIImagePickerControllerDelegate를 선언할 때, UINavigationControllerDelegate도 선언해야 하는 이유
* UIImagePickerControllerDelegate의 delegate 속성은 UIImagePickerControllerDelegate와 UINavigationControllerDelegate 프로토콜을 모두 구현하는 객체로 정의되어있다. 
* 위 코드의 `self.picker.delegate =  self`에서 self를 picker.delegate에 할당하려면 self는 UINavigationControllerDelegate 타입이어야 한다. 
* picker의 delegate를 UINavigationControllerDelegate에 위임해준 것인데, 대리자는 사용자가 이미지나 동영상을 선택하거나 picker화면을 종료할 때 알림을 받는다. 

## 결과
![Image](/images/UIImagePickerController_first.png)
처음의 이미지 뷰 화면
![Image](/images/UIImagePickerController_second.png)
이미지 선택 버튼을 누르면 앨범으로 넘어간다
![Image](/images/UIImagePickerController_third.png)
앨범에서 원하는 사진을 선택하면
![Image](/images/UIImagePickerController_last.png)
선택한 사진이 이미지 뷰에 들어간다.