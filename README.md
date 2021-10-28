
# Swift-Flows
> This is a tips and trick with Swift

## Documentation

* [Split an array into chunks](#split-an-array-into-chunks)
* [Move view when keyboard is shown](#move-view-when-keyboard-is-shown)
* [Type files](#type-files)
* [TextField](#textfield)
    - [Hide Keyboard When Tapped Around](#hide-keyboard-when-tapped-around)
    - [Add Done Button to Keyboard](#add-done-button-to-keyboard)
    - [Add LeftView to TextField](#add-leftview-to-textfield)
    - [Get Text Display in TextField](#get-text-display-in-textfield)


## Split An Array Into Chunks
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

## Move View When Keyboard Is Shown
Flow [blog](https://fluffy.es/move-view-when-keyboard-is-shown/)

## Type Files
```swift
import MobileCoreServices

 let pdfType = kUTTypePDF as String
 let audioType = kUTTypeAudio as String
    // .....
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





