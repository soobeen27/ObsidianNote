
## FileManager

```swift
    private func copyVideoToDocumentsDirectory(videoURL: URL, completion: @escaping (URL) -> Void) {
        let fileManager = FileManager.default
        let documentsDirectory = fileManager.urls(for: .documentDirectory, in: .userDomainMask)[0]
        let destinationURL = documentsDirectory.appendingPathComponent(videoURL.lastPathComponent)
        
        do {
            if fileManager.fileExists(atPath: destinationURL.path) {
                try fileManager.removeItem(at: destinationURL)
            }
            try fileManager.copyItem(at: videoURL, to: destinationURL)
            completion(destinationURL)
        } catch {
            print("Failed to copy video file: \(error)")
        }
    }
```


## PHPickerViewController
```swift
    func phpickerVCPresent() {
        var configuration = PHPickerConfiguration()
        configuration.selectionLimit = 10
        configuration.filter = .any(of: [.images, .videos])
        configuration.preferredAssetRepresentationMode = .current
        let picker = PHPickerViewController(configuration: configuration)
        picker.delegate = self
        self.present(picker, animated: true, completion: nil)
    }
```

```swift
extension UploadVC : PHPickerViewControllerDelegate {
    func picker(_ picker: PHPickerViewController, didFinishPicking results: [PHPickerResult]) {
        viewModel.mediaItems.accept(results)
        picker.dismiss(animated: true)
    }
}
```

```swift
    func configure(mediaItem: PHPickerResult) {
        if mediaItem.itemProvider.hasItemConformingToTypeIdentifier(UTType.movie.identifier) {
            mediaItem.itemProvider.loadFileRepresentation(forTypeIdentifier: UTType.movie.identifier) { (url, error)  in
                guard let url else { return }

                self.copyVideoToDocumentsDirectory(videoURL: url) { url in
                    DispatchQueue.main.async {
                        self.playVideo(url: url)
                    }
                }
            }
        } else if mediaItem.itemProvider.hasItemConformingToTypeIdentifier(UTType.image.identifier) {
            mediaItem.itemProvider.loadObject(ofClass: UIImage.self) { image, error in
                DispatchQueue.main.async {
                    self.imageView.image = image as? UIImage
                }
            }
        }
    }

    private func copyVideoToDocumentsDirectory(videoURL: URL, completion: @escaping (URL) -> Void) {
        let fileManager = FileManager.default
        let documentsDirectory = fileManager.urls(for: .documentDirectory, in: .userDomainMask)[0]
        let destinationURL = documentsDirectory.appendingPathComponent(videoURL.lastPathComponent)
        
        do {
            if fileManager.fileExists(atPath: destinationURL.path) {
                try fileManager.removeItem(at: destinationURL)
            }
            try fileManager.copyItem(at: videoURL, to: destinationURL)
            completion(destinationURL)
        } catch {
            print("Failed to copy video file: \(error)")
        }
    }

```

swift에서 firestore에 document를 올리려는데 document 명을 현재 시간으로 할 수 있을까?

