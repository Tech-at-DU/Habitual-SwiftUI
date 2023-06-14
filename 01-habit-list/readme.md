## Habit List

A list in SwiftUI requires elements to be identifiable. To be identifiable an element must have a unique id property. 

The habit list shows a list of Habits. Habits are a struct with properties of title, dateCreated, selectedImage, currentStreak, etc. To work with a list in SwiftUI each element displayed in the list should have a unique id. This is missing from the original Habit. 

Make `Habit.swft` Identifiable, and add `Identifiable` to the Habit definition. 

```Swift
struct Habit: Codable, Identifiable {

}
```

Add an id to habits. 

```Swift
var id: UUID
```

In the initializer set the value for id to a UUID. 

```Swift
self.id = UUID()
```

Rename ContentView.swift to HabitList.swift.

In `HabitList.swift` change ContentView to HabitList everywhere you see it! 

Open the main Swift file. This is named "project_nameApp.swift". Change `ContentView` to `HabitList` here. 

### Making the List

For now, you will mock up the list of habits. Later you will connect this to PersistanceLayer and get the actual habits from that class. 

Add the following above the HabitList struct. 

```Swift
let habits = [
  Habit(title: "Books", image: .book),
  Habit(title: "Gym", image: .gym),
  Habit(title: "Code", image: .code),
  Habit(title: "Drop", image: .drop),
]
```

To display a list by using `List`. This takes an iterable and generates elements for each item. 

Try this: 

```Swift 
List(habits) { habit in
  Text(habit.title)
}
```

Here you are making a `Text` element for each item in the `habits` array, you get the title of each habit and display it. In the preview, you'll see it in a list. 

Here is the whole text block for the `HabitList`

```Swift
import SwiftUI

let habits = [
  Habit(title: "Books", image: .book),
  Habit(title: "Gym", image: .gym),
  Habit(title: "Code", image: .code),
  Habit(title: "Drop", image: .drop),
]

struct HabitList: View {
  var body: some View {
    List(habits) { habit in
      Text(habit.title)
    }
  }
}

struct HabitList_Previews: PreviewProvider {
  static var previews: some View {
    HabitList()
  }
}
```

### Rows 

The `Text()` element displays a single line on each row. The original App displays an image, a label, a Streak count, and an accessory view. 

We can add some of this stuff here. 

```Swift 
Image(systemName: "globe")
Text(habit.title)
Spacer()
Text("\(habit.bestStreak)")
```

This block displays the Image, title, and best streak. I'm displaying a system image for the moment. 

To display the icon image you can use: 

```Swift
Image(uiImage: habit.selectedImage.image)
```

The Habit struct stores `selectedImage` as an Images enum type. This enum will return a UImage for the image property. So `habit.selectedImage.image` can be used with `Imageuiimage:)`. 

The `Spacer()` pushes the elements above and below to the left and right. 

Challenge: Use the other SwiftUI tools to style the text and features of the "cells".

### Make a Habit Cell view

Make a Habit cell view to replace the elements above and clean up your code. 

Choose File > New. Then choose `SwiftUI View` from the ios tab under User Interface. 

Name it: `HabitListCell`

Make the view look like this:

```Swift
struct HabitListCell: View {
  var habit: Habit 
  var body: some View {
    HStack {
      Image(uiImage: habit.selectedImage.image)
      Text(habit.title)
      Spacer()
      Text("\(habit.bestStreak)")
    }
  }
}
```

Notice there is a var `habit` at the top. This is a parameter that will come from outside this view. 

At the bottom of the preview make it look like this: 

```Swift
let habit = Habit(title: "Text", image: .grow)

struct HabitListCell_Previews: PreviewProvider {
  static var previews: some View {
    HabitListCell(habit: habit)
  }
}
```

Here you defined a `habit` and then passed that habit to the view. 

This preview section is used only for testing. 

Use this view in `HabitList`. Open `HabitList` and make the following changes. 

```Swift
List(habits) { habit in
  HabitListCell(habit: habit)
}
```

Here you used the `HabitCell` view in place of the individual elements you used earlier. You passed the habit instance to the view in the same way you passed the mocked-up habit to the preview! 

This view does not have the navigation controller and the button in the upper right corner. You'll add these in the future. You will make the view first then add the navigation. 