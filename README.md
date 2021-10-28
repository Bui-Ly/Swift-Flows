
# Swift-Flows
> This is a tips and trick with Swift

## Documentation

* [Split an array into chunks](#split-an-array-into-chunks)
* [Move view when keyboard is shown](#move-view-when-keyboard-is-shown)
* [Type files](#type-files)
* [TextField](#textfield)
    - [Hide Keyboard When Tapped Around](#hide-keyboard-when-tapped-around)
    - 


## Split an array into chunks
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

## Move view when keyboard is shown
Flow [blog](https://fluffy.es/move-view-when-keyboard-is-shown/)

<img width="292" alt="Screen Shot 2021-07-29 at 22 45 09" src="https://user-images.githubusercontent.com/45566801/127523263-90fe1161-8c7c-448d-b920-8d810c6ec647.png">

## Type files
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




