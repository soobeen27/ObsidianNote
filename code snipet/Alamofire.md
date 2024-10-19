
Define
```swift
func fetchData<T: Decodable>(url: URL, completion: @escaping (Result<T, AFError>) -> Void) {
    AF.request(url).responseDecodable(of: T.self) { response in
        completion(response.result)
    }
}
```

Usage
```swift
fetchData(url: url) { (result: Result<SomeData, AFError>) in
    switch result {
        case .success(let result):
			// 데이터 값 로드 성공! 사용하는 코드 작성
        case .failure(let error):
            print("데이터 로드 실패: \(error)")
            // 데이터 값 로드 실패
    }
}
```