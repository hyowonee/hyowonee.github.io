---
title: 카카효톡 어플 만들기 과정 - 1
author: Choi Hyowon
---
* 대중적인 카카오톡과 비슷한 어플을 만들어 보려고 한다.
* 우선 친구 목록을 알파벳 순서대로 보여주고, 각 알파벳 순으로 섹션을 나누었다.
* 어플 오른쪽 상위의 버튼으로 설정창으로 넘어가도록 하였다.

## storyboard 구현
![Image](/images/kakahyoTalk_main_storyboard.png)
첫 화면 연결 구성

![Image](/images/kakahyoTalk_settings_storyboard.png)
설정 화면 연결 구성

## Code 구현
### NameCell.swift
```swift
class NameCell: UITableViewCell {
    @IBOutlet var nameLabel: UILabel!
}
```

### ViewController.swift
```swift
class ViewController: UIViewController {
    
    @IBOutlet var searchBar: UISearchBar!
    @IBOutlet var tableView: UITableView!
    
    // 임의로 친구 데이터(이름)를 넣어준다.
    var items = ["Choi Hyowon", "Lee Jehyeon", "Jekon", "torso", "Picasso", "Choi Migyoung"]

    override func viewDidLoad() {
        super.viewDidLoad()
        self.reload()
    }
    
    func reload() {
        self.tableView.reloadData()
    }
    
}

extension ViewController: UITableViewDelegate, UITableViewDataSource {
    
    func numberOfSections(in tableView: UITableView) -> Int {
        // Set을 사용하면 중복되는 값이 사라지기 때문에 먼저 중복 값을 제외한 수를 리턴
        return Array(Set(self.items.map { $0.first! })).count
    }
    
    func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        // section이름이 중복되지 않게 Set으로 중복값을 제거해주고, 정렬해준다.
        // [A,B,J...] 식으로 된 배열의 [section]번째 -> 0번째 charactor: A, 1번째 charactor: B, 2번째 charactor: J 이런 식
        let charactor = Array(Set(self.items.map { $0.first! })).sorted()[section]
        
        // 검색창이 비어있을 경우 해당 section의 charactor과 이름의 첫글자가 일치하는 것만 리턴
        if self.searchBar.text?.isEmpty == true {
            return self.items.filter { $0.first! == charactor }.count
        }
        
        // 검색창에 내용이 있는 경우 그 내용을 포함하는 이름들의 개수를 리턴
        return self.items.filter { $0.first! == charactor }.filter { $0.contains(self.searchBar.text!) }.count
    }
    
    func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        // 각 섹션의 첫 문자를 charactor로 선언
        let charactor = Array(Set(self.items.map { $0.first! })).sorted()[indexPath.section]
        let cell = tableView.dequeueReusableCell(withIdentifier: "NameCell", for: indexPath) as! NameCell
        
        // 검색창이 비어있을 경우 charactor와 같은 첫글자를 가진 이름들만 골라서 리턴
        if self.searchBar.text?.isEmpty == true {
            cell.nameLabel.text = self.items.filter { $0.first! == charactor }[indexPath.row]
            
        } else { // 검색창에 내용이 있는 경우 그 내용을 포함하는 이름들만 골라서 리턴
            cell.nameLabel.text = self.items.filter { $0.first! == charactor }.filter { $0.contains(self.searchBar.text!) }[indexPath.row]
        }
        
        return cell
    }
    
    func tableView(_ tableView: UITableView, titleForHeaderInSection section: Int) -> String? {
        // 각 Section의 이름은 item의 첫 글자로 설정
        return String(Array(Set(items.map { $0.first! })).sorted()[section])
    }
    
}

extension ViewController: UISearchBarDelegate {
    
    // 검색창의 내용이 바뀔 때마다 tableView를 reload해줘서 반영
    func searchBar(_ searchBar: UISearchBar, textDidChange searchText: String) {
        self.reload()
    }
    
}
```
## 결과
![Image](/images/kakahyoTalk_main_result.png)
어플의 첫 화면

![Image](/images/kakahyoTalk_settings_result.png)
설정 화면 (추가 구현 예정)