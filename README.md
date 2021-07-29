
# Swift-Flows

## Documentation

* [Move view when keyboard is shown (guide)](#move-view-when-keyboard-is-shown-(guide))
* [Add done button on keyboard](#add-done-button-on-keyboard)


## Move view when keyboard is shown (guide)
Flow [blog](https://fluffy.es/move-view-when-keyboard-is-shown/)

## Add done button on keyboard

```swift
func addDoneButtonOnKeyboard(){
        let doneToolbar: UIToolbar = UIToolbar(frame: CGRect.init(x: 0, y: 0, width: UIScreen.main.bounds.width, height: 50))
        doneToolbar.barStyle = .default
        
        let flexSpace = UIBarButtonItem(barButtonSystemItem: .flexibleSpace, target: nil, action: nil)
        let done: UIBarButtonItem = UIBarButtonItem(barButtonSystemItem: .done, target: self, action: #selector(doneButtonAction))
        
        let items = [flexSpace, done]
        doneToolbar.items = items
        doneToolbar.sizeToFit()
        
        textfield.inputAccessoryView = doneToolbar
    }
```
<img width="292" alt="Screen Shot 2021-07-29 at 22 45 09" src="https://user-images.githubusercontent.com/45566801/127523263-90fe1161-8c7c-448d-b920-8d810c6ec647.png">


