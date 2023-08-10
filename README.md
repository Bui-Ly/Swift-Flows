
# Swift-Flows
> This is a tips and trick with Swift

## Documentation

* [Move view when keyboard is shown](#move-view-when-keyboard-is-shown)
* [Type files](#type-files)
* [Collection Types](#collection-types)
    - [Split An Array Into Chunks](#split-an-array-into-chunks)
* [Strings and Characters](#strings-and-characters)
*   - [Generate List Aplhabet](#generate-list-aplhabet)
*   - [Format Numbers](#format-numbers)
*   - [Standard String](#standard-string)
* [TextField](#textfield)
    - [Hide Keyboard When Tapped Around](#hide-keyboard-when-tapped-around)
    - [Add Done Button to Keyboard](#add-done-button-to-keyboard)
    - [Add LeftView to TextField](#add-leftview-to-textfield)
    - [Get Text Display in TextField](#get-text-display-in-textfield)
* [Get URL from PHAsset](#get-url-from-phasset)
* [Get name file from PHAsset](#get-name-file-from-phasset)
* [Load image from URL](#load-image-from-url)
* [Scale Preserving Aspect Ratio](#scale-preserving-aspect-ratio)
* [Make app default theme](#make-app-default-theme)
* [Capitalizing First Letter](#capitalizing-first-letter)

## Move View When Keyboard Is Shown

Flow [blog](https://fluffy.es/move-view-when-keyboard-is-shown/)

## Type Files

```swift
import MobileCoreServices

 let pdfType = kUTTypePDF as String
 let audioType = kUTTypeAudio as String
    // .....
```
## Collection Types
> All tips and trick with Collection Types

#### Split An Array Into Chunks

```swift
extension Array {
    func chunked(into size: Int) -> [[Element]] {
        return stride(from: 0, to: count, by: size).map {
            Array(self[$0 ..< Swift.min($0 + size, count)])
        }
    }
}

let numbers = Array(1...10)
let result = numbers.chunked(into: 4) //Output: [[1, 2, 3, 4], [5, 6, 7, 8], [9, 10]]
```

## Strings and Characters
> All tips and trick with Strings and Characters

#### Generate List Aplhabet

```swift
let alphabet = (97...122)
    .map { Character(UnicodeScalar($0)) }
    .map { String($0) } // lowercased

// output: ["a", "b", "c", "d", "e", "f", "g", "h", "i", "j", "k", "l", "m", "n", "o", "p", "q", "r", "s", "t", "u", "v", "w", "x", "y", "z"]
```
#### Format Numbers

```swift
extension UIViewController {
    func formatPrice(price: String, locacle: Locale = .init(identifier: "vi_VN")) -> String {
        let formatter = NumberFormatter()
        formatter.usesGroupingSeparator = true
        formatter.numberStyle = .decimal
        formatter.groupingSeparator = ","
        formatter.locale = locacle
        guard let price = Double(price) as NSNumber? else { return "N/A"}
        
        return formatter.string(from: price)!
        
        // output: formatPrice(price: "10000000") - 10,000,000
    }
}
```
#### Standard String

```swift
 let wrongString = "  asfd saf   asfj   sdf s\n sdf sf    \nz\n"
 let standardString = wrongString.replacingOccurrences(of: #"\s\s+"#, with: " ", options: .regularExpression, range: nil).trimmingCharacters(in: .whitespacesAndNewlines)
 // output:asfd saf asfj sdf s sdf sf z
```

## TextField
> All tips and trick with textfield

#### Hide Keyboard When Tapped Around

```swift
extension UIViewController {
    func hideKeyboardWhenTappedAround() {
        let tap = UITapGestureRecognizer(target: self, action: #selector(UIViewController.dismissKeyboard))
        tap.cancelsTouchesInView = false
        view.addGestureRecognizer(tap)
    }
    
    @objc func dismissKeyboard() {
        view.endEditing(true)
    }
}
```
#### Add Done Button to Keyboard

```swift
extension UITextField {
    
    func addDoneButton() {
        let keyboardToolbar = UIToolbar(frame: CGRect(x: 0, y: 0, width: UIScreen.main.bounds.width, height: 40))
        keyboardToolbar.sizeToFit()
        let flexBarButton = UIBarButtonItem(barButtonSystemItem: .flexibleSpace,
                                            target: nil, action: nil)
        let doneBarButton = UIBarButtonItem(barButtonSystemItem: .done,
                                            target: self, action: #selector(UIView.endEditing(_:)))
        keyboardToolbar.items = [flexBarButton, doneBarButton]
        self.inputAccessoryView = keyboardToolbar
    }
    
    /// Adding a Done button to Keyboard with a Custom action
    /// - Parameters:
    ///   - action: custom action if user want to catch done event
    ///   - target: the target of the action
    func addDoneButton(withTarget target: Any? = nil, action: Selector) {
        let keyboardToolbar = UIToolbar(frame: CGRect(x: 0, y: 0, width: UIScreen.main.bounds.width, height: 40))
        keyboardToolbar.sizeToFit()
        let flexBarButton = UIBarButtonItem(barButtonSystemItem: .flexibleSpace,
                                            target: nil, action: nil)
        let doneBarButton = UIBarButtonItem(barButtonSystemItem: .done,
                                            target: (target != nil) ? target : self,
                                            action: action)
        keyboardToolbar.items = [flexBarButton, doneBarButton]
        self.inputAccessoryView = keyboardToolbar
    }
}
/// Used Guide
class ViewController: UIViewController {
    
    @IBOutlet weak var textField: UITableView!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        textField.addDoneButton(withTarget: self, action: #selector(doSomethingWithDoneAction))
    }
    
    @objc func doSomethingWithDoneAction() {
        print("do something!")
        textField.resignFirstResponder()
    }
}
```
#### Add LeftView to TextField

```swift
class ViewController: UIViewController {
    
    @IBOutlet weak var textField: UITableView!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        let icon = UIImage(systemName: "magnifyingglass")
        let iconImageView = UIImageView(frame: CGRect(x: 5, y: 0, width: 20, height: 20))
        iconImageView.tintColor = .placeholderText
        iconImageView.image = icon
        
        let leftView = UIView(frame: CGRect(x: 0, y: 0, width: 20, height: 20))
        leftView.addSubview(iconImageView)
      
        textTextField.leftView = leftView
        textTextField.leftViewMode = .always
        
        // Action
        let tapGestureRecognizer = UITapGestureRecognizer(target: self, action: #selector(doSomethingWithLeftViewAction))
        leftView.addGestureRecognizer(tapGestureRecognizer)
    }
    
     @objc func doSomethingWithLeftViewAction() {
        print("do something!")
    }
}
```
#### Get Text Display in TextField

```swift
extension TextFieldComponentsViewController: UITextFieldDelegate {
    
    func textField(_ textField: UITextField, shouldChangeCharactersIn range: NSRange, replacementString string: String) -> Bool {
        let currentText = textField.text ?? ""
        guard let stringRange = Range(range, in: currentText) else { return false }
        let textToDisplay = currentText.replacingCharacters(in: stringRange, with: string)
        print(textToDisplay)
        return true
    }
}
```

## Get URL from PHAsset

```swift
extension PHAsset {
    func getURLFromPHAsset(completionHandler : @escaping ((_ responseURL : URL?) -> Void)){
        if self.mediaType == .image {
            let options: PHContentEditingInputRequestOptions = PHContentEditingInputRequestOptions()
            options.canHandleAdjustmentData = {(adjustmeta: PHAdjustmentData) -> Bool in
                return true
            }
            self.requestContentEditingInput(
                with: options,
                completionHandler: {(contentEditingInput: PHContentEditingInput?, info: [AnyHashable : Any]) -> Void in
                    completionHandler(contentEditingInput!.fullSizeImageURL as URL?)
                }
            )
        } else if self.mediaType == .video {
            let options: PHVideoRequestOptions = PHVideoRequestOptions()
            options.version = .original
            PHImageManager.default().requestAVAsset(
                forVideo: self,
                options: options,
                resultHandler: {(asset: AVAsset?, audioMix: AVAudioMix?, info: [AnyHashable : Any]?) -> Void in
                    if let urlAsset = asset as? AVURLAsset {
                        let localVideoUrl: URL = urlAsset.url as URL
                        completionHandler(localVideoUrl)
                    } else {
                        completionHandler(nil)
                    }
                }
            )
        }
    }
}
```

## Get name file from PHAsset

```swift
extension PHAsset {
    
    var primaryResource: PHAssetResource? {
        let types: Set<PHAssetResourceType>
        
        switch mediaType {
        case .video:
            types = [.video, .fullSizeVideo]
        case .image:
            types = [.photo, .fullSizePhoto]
        case .audio:
            types = [.audio]
        case .unknown:
            types = []
        @unknown default:
            types = []
        }
        
        let resources = PHAssetResource.assetResources(for: self)
        let resource = resources.first { types.contains($0.type)}
        
        return resource ?? resources.first
    }
    
    var originalFilename: String {
        guard let result = primaryResource else {
            return "file"
        }
        
        return result.originalFilename
    }
}
```

## Load image from URL

```swift
extension UIImageView {
    func load(url: URL) {
        DispatchQueue.global().async { [weak self] in
            if let data = try? Data(contentsOf: url) {
                if let image = UIImage(data: data) {
                    DispatchQueue.main.async {
                        self?.image = image
                    }
                }
            }
        }
    }
    
    func load(urlString: String) {
        guard let url = URL(string: urlString) else { return }
        
        DispatchQueue.global().async { [weak self] in
            if let data = try? Data(contentsOf: url) {
                if let image = UIImage(data: data) {
                    DispatchQueue.main.async {
                        self?.image = image
                    }
                }
            }
        }
    }
}
```

## Scale Preserving Aspect Ratio

```swift
extension UIImage {
    func scalePreservingAspectRatio(targetSize: CGSize) -> UIImage {
        // Determine the scale factor that preserves aspect ratio
        let widthRatio = targetSize.width / size.width
        let heightRatio = targetSize.height / size.height
        
        let scaleFactor = min(widthRatio, heightRatio)
        
        // Compute the new image size that preserves aspect ratio
        let scaledImageSize = CGSize(
            width: size.width * scaleFactor,
            height: size.height * scaleFactor
        )

        // Draw and return the resized UIImage
        let renderer = UIGraphicsImageRenderer(
            size: scaledImageSize
        )

        let scaledImage = renderer.image { _ in
            self.draw(in: CGRect(
                origin: .zero,
                size: scaledImageSize
            ))
        }
        
        return scaledImage
    }
}
```

## Make app default theme

```swift
class SceneDelegate: UIResponder, UIWindowSceneDelegate {
    var window: UIWindow?
    func scene(_ scene: UIScene, willConnectTo session: UISceneSession, options connectionOptions: UIScene.ConnectionOptions) {
        // Use this method to optionally configure and attach the UIWindow `window` to the provided UIWindowScene `scene`.
        // If using a storyboard, the `window` property will automatically be initialized and attached to the scene.
        // This delegate does not imply the connecting scene or session are new (see `application:configurationForConnectingSceneSession` instead).
        guard let _ = (scene as? UIWindowScene) else { return }
        window?.overrideUserInterfaceStyle = .dark
    }
 }
```

## Capitalizing First Letter

```swift
extension String {
    func capitalizingFirstLetter() -> String {
        return prefix(1).capitalized + dropFirst()
    }

    mutating func capitalizeFirstLetter() {
        self = self.capitalizingFirstLetter()
    }
}
```

## Tapped Specil text in UILabel

```swift
extension UITapGestureRecognizer {
    
    func didTapAttributedTextInLabel(label: UILabel, inRange targetRange: NSRange) -> Bool {
        // Create instances of NSLayoutManager, NSTextContainer and NSTextStorage
        let layoutManager = NSLayoutManager()
        let textContainer = NSTextContainer(size: CGSize.zero)
        let textStorage = NSTextStorage(attributedString: label.attributedText!)
        
        // Configure layoutManager and textStorage
        layoutManager.addTextContainer(textContainer)
        textStorage.addLayoutManager(layoutManager)
        
        // Configure textContainer
        textContainer.lineFragmentPadding = 0.0
        textContainer.lineBreakMode = label.lineBreakMode
        textContainer.maximumNumberOfLines = label.numberOfLines
        let labelSize = label.bounds.size
        textContainer.size = labelSize
        
        // Find the tapped character location and compare it to the specified range
        let locationOfTouchInLabel = self.location(in: label)
        let textBoundingBox = layoutManager.usedRect(for: textContainer)
        let textContainerOffset = CGPoint(x: (labelSize.width - textBoundingBox.size.width) * 0.5 - textBoundingBox.origin.x,
                                          y: (labelSize.height - textBoundingBox.size.height) * 0.5 - textBoundingBox.origin.y);
        let locationOfTouchInTextContainer = CGPoint(x: locationOfTouchInLabel.x - textContainerOffset.x,
                                                     y: locationOfTouchInLabel.y - textContainerOffset.y);
        var indexOfCharacter = layoutManager.characterIndex(for: locationOfTouchInTextContainer, in: textContainer, fractionOfDistanceBetweenInsertionPoints: nil)
        indexOfCharacter = indexOfCharacter + 4
        return NSLocationInRange(indexOfCharacter, targetRange)
    }
    
}
```

```swift
        guard let text = label.text else { return }
        let textRange = (text as NSString).range(of: "Phương thức giao dịch điện tử")
        if gesture.didTapAttributedTextInLabel(label: label, inRange: textRange) {
            
        }
```





