---
title: iOS에서 IBDesignable과 IBInspectable 사용하기
author: Choi Hyowon
---
* IBDesignable과 IBInspectable 기능은 커스텀 뷰를 만들때 인터페이스 빌더 내에서 라이브로 화면이 어떻게 구성되는지 보여줄 수 있다.
> 이는 커스텀 뷰를 만들기 위해 수 많은 빌드를 통해 실행하여 볼 필요가 없음을 의미한다.
* 가운데의 카운트값을 1씩 올리고 내리는 카운트 바를 만들어 보았다.
* 카운트의 초기값은 0이며, 플러스와 마이너스 버튼으로 값을 조정한다.
* 마이너스 버튼은 "-", 플러스 버튼은 "+" 이다.
* 버튼의 text는 모두 가운데 정렬이다.

## CounterView.swift
```swift
@IBDesignable
class CounterView: UIView {
    
    var minusButton: UIButton = UIButton()
    var plusButton: UIButton = UIButton()
    var countLabel: UILabel = UILabel()
    
    @IBInspectable var count: Int = 0

    override func draw(_ rect: CGRect) {
        super.draw(rect)
        
        // minus와 plus버튼의 너비는 30으로 고정되고 countLabel의 너비는 frame너비 - 60으로 맞춰진다. (빈 공간이 없도록 너비가 자동으로 맞춰진다)
        self.minusButton.frame = CGRect(x: 0, y: 0, width: 30, height: 30)
        self.countLabel.frame = CGRect(x: 30, y: 0, width: self.frame.size.width - 60, height: 30)
        self.plusButton.frame = CGRect(x: 30 + self.countLabel.frame.size.width, y: 0, width: 30, height: 30)
        
        self.minusButton.backgroundColor = .red
        self.countLabel.backgroundColor = .blue
        self.plusButton.backgroundColor = .green
      
        self.minusButton.setTitle("-", for: .normal)
        self.minusButton.titleLabel?.textColor = .white
        self.minusButton.titleLabel?.textAlignment = .center
        self.countLabel.text = "\(self.count)"
        self.countLabel.textColor = .white
        self.countLabel.textAlignment = .center
        self.plusButton.setTitle("+", for: .normal)
        self.plusButton.titleLabel?.textColor = .white
        self.plusButton.titleLabel?.textAlignment = .center
        
        // storyboard 설정 없이 @IBAction의 역할을 코드로 대신 구현
        self.minusButton.addTarget(self, action: #selector(self.minus), for: .touchUpInside)
        self.plusButton.addTarget(self, action: #selector(self.plus), for: .touchUpInside)
        
        self.addSubview(minusButton)
        self.addSubview(countLabel)
        self.addSubview(plusButton)
    }
    
    @objc func minus() {
        self.count -= 1
        self.countLabel.text = "\(self.count)"
    }
    
    @objc func plus() {
        self.count += 1
        self.countLabel.text = "\(self.count)"
    }   
}
```
## storyboard
![Image](/images/CounterView_class.png)
storyboard에는 UIView 하나를 만들고 class이름을 custom으로 만든 CustomView로 설정한다.

## 결과
![Image](/images/IBDesignable_IBInspectable.png)
IBDesignable로 인해 storyboard에 customView가 나타나고, ,IBInspectable로 인해 Count의 값의 조정을 storyboard로 볼 수 있다.