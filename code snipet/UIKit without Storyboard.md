```swift

func scene(_ scene: UIScene, willConnectTo session: UISceneSession, options connectionOptions: UIScene.ConnectionOptions) {
    guard let windowScene = (scene as? UIWindowScene) else { return }
    let window = UIWindow(windowScene: windowScene)
    window.rootViewController = ViewController() // <- Root VC setting
    window.makeKeyAndVisible()
    self.window = window
}

```

`Main` -> `Move to trash`

Delete `info` -> `Storyboard Name` 

Delete  `project file` -> `Build Settings` -> `UIKit Main Storyboard File Base Name` 