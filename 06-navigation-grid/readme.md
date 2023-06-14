
## Navigation Grid

Add Habit is where we choose an image to represent the habit we are about to create. We need to be able to select an image from the grid before advancing to the next view by tapping the button at the bottom. 

To select an image you'll need to use `@State`. This state variable will hold the selected image. 

That brings up the question: what is the selected image? Specifically, I mean what type? The images are defined in the enum `Habit.Images`. This is just the enum, each case returns a `UIImage`. 

Here is what you currently have: 

```Swift
import SwiftUI

struct AddHabit: View {
  let gridConfig = [
    GridItem(.adaptive(minimum: 80)),
    GridItem(.adaptive(minimum: 80)),
    GridItem(.adaptive(minimum: 80))
  ]
  
  var body: some View {
    VStack {
      LazyVGrid(columns: gridConfig) {
        ForEach(Habit.Images.allCases, id: \.rawValue) { item in
          Image(uiImage: item.image)
        }
      }
      Spacer()
      Button("Select Image") {
      // 
      }
    }.padding(30)
  }
}

struct AddHabit_Previews: PreviewProvider {
  static var previews: some View {
    AddHabit()
  }
}
```

Add a `@State` variable to the top:

```Swift
struct AddHabit: View {
  @State private var selectedImage: Habit.Images?
  ...
}
```

This is variable `selectedImage` is type optional Habit.Images. It's optional so the value can be nil, or the value can be one of the `Habit.Images` cases. 

It's marked `@State` so if this variable is updated the View will update its display. 

Next, find where the images are displayed. This happens in the `ForEach`. It should look something like this: 

```Swift
ForEach(Habit.Images.allCases, id: \.rawValue) { item in
  Image(uiImage: item.image)
}
```

This displays one image for each case in `Habit.Images`. 

You want the selected image to display differently from the others. The selected image is stored in `selectedImage`. That means if `item` is equal to `selectedImage` then it is the selected one. 

To differentiate the display you'll use `.colorMultiply`. This takes a color as an argument and will make the colors darker depending on the color. 

```Swift
Image(uiImage: item.image)
  .colorMultiply(item == selectedImage ? .white : .gray)
```

Here `.colorMultiply` uses `.white` when the selected image matches the item and `.gray` when they do not match. White will make no change, but gray will make the colors of the image darker. 

Select an image by adding a `.onTapGesture`. 

```Swift
Image(uiImage: item.image)
  .colorMultiply(item == selectedImage ? .white : .gray)
  .onTapGesture {
  selectedImage = item
}
```

At this stage, all of the images should initially appear "gray". When the image is tapped it should show in full color.

Last, this view needs to navigate to the `ConfirmHabit` view. Replace the button at the bottom with a `NavigationLink`.

```Swift
NavigationLink(destination: ConfirmHabit()) {
  Text("Select Image")
}
```