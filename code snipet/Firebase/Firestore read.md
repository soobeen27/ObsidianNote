
```swift
db.collection("users")

            .whereField("userName", isGreaterThanOrEqualTo: searchText)

            .whereField("userName", isLessThanOrEqualTo: searchText + "\u{f8ff}")

            .getDocuments { (querySnapshot, error) in

                if let error = error {

                    print("Error getting documents: \(error)")

                    completion(nil, error)

                } else if let querySnapshot = querySnapshot {

                    var users: [User] = []

                    for document in querySnapshot.documents {

                        do {

                            let user = try document.data(as: User.self)

                            users.append(user)

                        } catch {

                            print("Error decoding user: \(error)")

                        }

                    }

                    completion(users, nil)

                } else {

                    // 에러도 없고, querySnapshot도 없는 경우

                    completion(nil, nil)

                }

            }
```