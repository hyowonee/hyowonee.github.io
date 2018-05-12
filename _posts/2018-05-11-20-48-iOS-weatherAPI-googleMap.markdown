---
title: iOS에서 날씨 API와 GoogleMap을 이용한 어플 만들기
author: Choi Hyowon
---
* iOS에서 GoogleMap을 띄우고 탭으로 위치를 이동하면 해당 위치의 날씨가 나오는 어플을 만들었다.
* Alamofire와 AlamofireObjectMapper, ObjectMapper를 사용하였다.

## storyboard 연결
![Image](/images/iOS_weather_api_storyboard.png)

## AppDelegate.swift
AppDelegate파일에 등록한 API키를 넣어준다.
```swift
 func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool {
        GMSServices.provideAPIKey("AIzaSyCj4IsZft9QugV3RbnKlB1l9_FfMwyFvWY") 
        return true
    }
```

## Model
### Weather.swift
```swift
class Weather: Mappable {
    var name: String = ""
    var main: Main? = nil
    
    required init?(map: Map) {}
    
    func mapping(map: Map) {
        self.name <- map["name"]
        self.main <- map["main"]
    }
}
```

### Main.swift
```swift
class Main: Mappable {
    var temp: Double = 0.0
    
    required init?(map: Map) {}
    
    func mapping(map: Map) {
        self.temp <- map["temp"]
    }
}
```

## Controller
### ViewController.swift
```swift
class ViewController: UIViewController {
    
    @IBOutlet var mapView: GMSMapView!
    @IBOutlet var tempLabel: UILabel!
    @IBOutlet var placeLabel: UILabel!
    
    var apiKey = "af90d6e8611a0eb157ef6264f53a575e"
    // 초기 위치 설정
    var lat = 37.566535
    var lon = 126.977969
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        let camera = GMSCameraPosition.camera(withLatitude: lat, longitude: lon, zoom: 14.0)
        self.mapView.isMyLocationEnabled = true
        self.mapView.settings.zoomGestures = true
        self.mapView.camera = camera
        self.mapView.delegate = self
        
        let marker = GMSMarker()
        marker.position = CLLocationCoordinate2D(latitude: lat, longitude: lon)
        marker.map = mapView
        
        self.getWeather(lat: lat, lon: lon)
    }
    
    // 날씨 API로 지도의 지역이름, 온도 정보를 가져온다.
    func getWeather(lat: Double, lon: Double) {
        Alamofire
            .request("https://api.openweathermap.org/data/2.5/weather?lat=\(lat)&lon=\(lon)&apiKey=\(apiKey)")
            // Weather형태로 DataResponse의 데이터를 받아와서 response 변수에 넣어줌
            .responseObject { (response: DataResponse<Weather>) in
                let weatherResponse = response.result.value
                
                self.placeLabel.text = weatherResponse?.name ?? ""
                if let temp = weatherResponse?.main?.temp {
                    // 절대온도 -> 섭씨온도로 바꿔준다.
                    self.tempLabel.text = "\(temp - 273.15)℃"
                }
            }
    }

}

extension ViewController: GMSMapViewDelegate {
    func mapView(_ mapView: GMSMapView, idleAt position: GMSCameraPosition) {
        self.getWeather(lat: position.target.latitude, lon: position.target.longitude)
    }
}
```
## 결과
![Image](/images/iOS_weather_api_result.png)
