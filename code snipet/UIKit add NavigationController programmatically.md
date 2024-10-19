

```swift
    func scene(_ scene: UIScene, willConnectTo session: UISceneSession, options connectionOptions: UIScene.ConnectionOptions) {
        guard let windowScene = (scene as? UIWindowScene) else { return }
        window = UIWindow(windowScene: windowScene)
        let navigationContoller = UINavigationController(rootViewController: ViewController())
            
        window?.rootViewController = navigationContoller
        window?.makeKeyAndVisible()
    }
```
