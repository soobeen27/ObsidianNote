
add

```swift
        let userRef = db.collection("users").document(user.uid)
        
        userRef.updateData(["blackList" : FieldValue.arrayUnion([uid])]) 
        { error in
            if let error = error {
                print("차단하는 과정중 에러: \(error)")
                completion(false)
            }
            print("차단 성공")
            completion(true)
        }
```

delete

```swift
        let userRef = db.collection("users").document(user.uid)
        
        userRef.updateData(["blackList" : FieldValue.arrayRemove([uid])]) 
        { error in
            if let error = error {
                print("차단 해제중 오류: \(error)")
                completion(false)
            }
            print("차단 해제 성공")
            completion(true)
        }
```