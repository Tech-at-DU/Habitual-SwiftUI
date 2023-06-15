

## Passing the Habit Image to ConfirmHabit

The next trick will be to pass the selected habit image to the `ConfirmHabit` view. 

To do this start in the `ConfirmHabit` view and add a variable to hold the image. 

```Swift
struct ConfirmHabit: View {
  var image: Habit.Images? // <-- Add this
  ...
}
```

Here you added the `image` var. This is type optional `Habit.Images`

Next, find the `Image()` view where the image is displayed. Change it to look like this: 

```Swift
...
Image(uiImage: image?.image ?? Habit.Images.bulb.image)
...
```

Here `Image` displays the image or the Bulb image if `image` is nil. 

By adding the `image` var you are now required to pass this to the view. 

Go to the bottom and edit the preview. Define a variable for testing: 

```Swift
var confirmHabitImage: Habit.Images? = Habit.Images.grow
```

Next, pass that variable to the `ConfirHabit` view like this: 

```Swift
ConfirmHabit(image: confirmHabitImage)
```

When you test this view you should see the `grow` image. When the view is displayed another way you'll see the image that is passed to this view. Let's handle that. 

Open the `AddHabit` view. Find the `NavigationLink` and updated it to look like this: 

```Swift
NavigationLink(destination: ConfirmHabit(image: selectedImage)) {
  Text("Select Image")
}
```

Notice you are passing the selected image as the argument to the image. 

## Getting back to the list 

To get back to the root view, the HabitList, should be easy but it turns out SwiftUI doesn't provide an easy way to do this. 

I found this extension on Stack Overflow to help us out. 

Create a new file `NavigationUtil.swft`. Add the following. 

```Swift
import Foundation
import UIKit

struct NavigationUtil {
    static func popToRootView() {
        DispatchQueue.main.asyncAfter(deadline: .now()) {
            findNavigationController(viewController:
UIApplication.shared.windows.filter { $0.isKeyWindow
}.first?.rootViewController)?
                .popToRootViewController(animated: true)
        }
    }
static func findNavigationController(viewController: UIViewController?)
-> UINavigationController? {
        guard let viewController = viewController else {
            return nil
        }
if let navigationController = viewController as? UINavigationController
{
        return navigationController
    }
for childViewController in viewController.children {
        return findNavigationController(viewController:
childViewController)
    }
return nil
    }
}
```

From here you can use: `NavigationUtil.popToRootView()` to return to the first View in the stack. 

Add this to the button: 

```Swift
Button("Create Habit") {
	NavigationUtil.popToRootView()
}
```