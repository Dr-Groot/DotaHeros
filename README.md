# DotaHeros

![appstore](https://user-images.githubusercontent.com/63160825/219557863-220ac759-0273-471f-9e28-283fb3793134.png)

Dota hero characters and their stats. In this project i have use Moya for api work with ReactiveRx, Kingfisher for loading Images, SkeletonView for shimmer and Reusable for simple working.

MyPods:

+ pod 'Alamofire'
+ pod 'Moya/RxSwift'
+ pod 'RxSwift'
+ pod 'RxCocoa'
+ pod 'Kingfisher'
+ pod 'SkeletonView'
+ pod 'Reusable'
+ pod 'NVActivityIndicatorView', '~> 4.8.0'

![Simulator Screen Shot - iPhone 11 - 2023-02-17 at 11 06 09](https://user-images.githubusercontent.com/63160825/219558302-415c6d32-9bcd-442a-9cc8-4706030959b3.png)

![Simulator Screen Shot - iPhone 11 - 2023-02-17 at 11 06 20](https://user-images.githubusercontent.com/63160825/219558331-cfad6750-68ab-41a5-a744-8e2434b5d861.png)

> TIP (How to manage data and best way to call api using Moya)

```swift
import Foundation
import Moya

class NetworkManager {

    static let shared: NetworkManager = NetworkManager()

    let provider = MoyaProvider<Service>()

    func callAPI(forEntity service: Service, completion: @escaping (Bool, String, HERO?) -> Void) {
        provider.request(service) { result in
            switch result {
            case let .success(response):
                let decoder = JSONDecoder()
                let arrData = try! decoder.decode(HERO.self, from: response.data)
                completion(true, "Success", arrData)
            case .failure:
                completion(false, "Failure", nil)
            }
        }
    }
}
```

```swift
import Foundation

class ViewModel {
    
    var mainData: HERO?
    
    func getData(completion: @escaping (Bool, String) -> Void) {
        NetworkManager.shared.callAPI(forEntity: .heroStats) { [weak self] isSuccess, message, resultData in
            guard let self = self else { return }
            if isSuccess {
                self.mainData = resultData
                print(message)
            } else {
                print(message)
            }
            completion(isSuccess, message)
        }
    }
}
```
