---
title: iOS에서 ScrollView와 pageControl 사용하기
author: Choi Hyowon
---
ScrollView와 pageControl을 이용하여 Paging Color을 만들어 보았다.
* 총 5개의 page index를 두었다.
* scroll하여 page를 넘어갈 때마다 배경의 색깔이 검은색에서 점차 밝은 색으로 바뀌게 하였다.

## storyboard

### scrollView
![Image](/images/pagingColor_scrollView.png)

### pageControl
![Image](/images/pagingColor_pageControl.png)

## Code

### ViewController.swift
```swift
class ViewController: UIViewController {
    
    @IBOutlet var scrollView: UIScrollView!
    @IBOutlet var pageControl: UIPageControl!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        for index in 0..<5 {
            let subView = UIView()
            subView.frame = UIScreen.main.bounds
            // RBG, 투명도 값 설정
            subView.backgroundColor = UIColor( 
                red: CGFloat(index * 50) / 255.0,
                green: CGFloat(index * 50) / 255.0,
                blue: CGFloat(index * 50) / 255.0,
                alpha: 1
            )
            // subView의 x좌표를 기기의 너비 * index만큼 주어 각 페이지의 시작 x좌표를 설정
            subView.frame.origin.x = UIScreen.main.bounds.width * CGFloat(index)
            scrollView.addSubview(subView)
        }
        // scrollView에서 페이징이 가능하도록 설정
        scrollView.isPagingEnabled = true
        // scrollView의 contentSize를 5 페이지 만큼으로 설정
        scrollView.contentSize = CGSize(
            width: UIScreen.main.bounds.width * 5,
            height: UIScreen.main.bounds.height
        )
        scrollView.alwaysBounceVertical = false // 수직 스크롤 바운스 안되게 설정
        
        pageControl.numberOfPages = 5
    }
}

extension ViewController: UIScrollViewDelegate {
    func scrollViewDidScroll(_ scrollView: UIScrollView) {
        // floor 내림, ceil 올림
        // contentOffset는 현재 스크롤된 좌표
        pageControl.currentPage = Int(floor(scrollView.contentOffset.x / UIScreen.main.bounds.width))
    }
}
```

## 결과
### 첫 번째 인덱스
![Image](/images/pagingColor_result_firstIndex.png)
### 마지막 인덱스
![Image](/images/pagingColor_result_lastIndex.png)