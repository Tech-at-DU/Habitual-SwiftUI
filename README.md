# Habitual SwiftUI 

Create a new Xcode project set the Language to Swift and SwiftUI as the Interface. 

Create the PersistenceLayer, Habit Class, and Date Extensions. These files provide infastructures for app. If you completed the Habitual Storyboard app you can copy those file from that project into your project. 

You also need to get the assets used by the project. Copy or import the images for the habit icons into your SwiftUI project. 

## Views to Build

You will build each of the views of the project first with mock up data. Then you will connect these together and make the app function. 

The views you need to create are: 

- HabitList - HabitTableViewController
- HabitDetails - HabitDetailsViewController
- AddHabit - AddHabitViewController
- ConfirmHabit - ConfriemHabitViewController

## Habit List

A list in SwiftUI requires elements to be idenitifiable. To be identifiable an element must have a unique id property. 

The habit list shows a list of Habits. Habits are a struct with properties of title, dateCreated, selectedImage, currentStreak etc. To work with a list in SwiftUI each element displayed in the list should have a unique id. This is missing from the original Habit. 

Make Habit.swft Identifiable, add `Identifiable` to the Habit definition. 

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

In HabitList.swift change ContentView to HabitList everywhere you see it! 

Open main swift file. This is named "project_nameApp.swift". Change `ContentView` to `HabitList` here. 

### Makeing the List

For now you will mock up the list of habits. Later you will connect this to PersistanceLayer and get the actual habits from that class. 

Add the following above the HabitList struct. 

```Swift
let habits = [
    Habit(title: "Books", image: .book),
    Habit(title: "Gym", image: .gym),
    Habit(title: "Code", image: .code),
    Habit(title: "Drop", image: .drop),
]
```

To display a list by using `List`. This takes an interable and generates elements for each item. 

Try this: 

```Swift 
List(habits) { habit in
    Text(habit.title)
}
```

Here you are making a `Text` element for each item in the `habits` array, you get the title of each habit and display it. In the preview you'll see it in a list. 

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

The `Text()` element displays a single line on each row. The original App displays an image, a label, Streak count, and a accessory view. 

We can add some of this stuff here. 

```Swift 
Image(systemName: "globe")
Text(habit.title)
Spacer()
Text("\(habit.bestStreak)")
```

This block displays the Image, title, and bext streak. I'm displaying a system image for the moment. 

To display the icon image you can use: 

```Swift
Image(uiImage: habit.selectedImage.image)
```

The Habit struct stores `selectedImage` as an Images enum type. This enum will return a UImage for the image property. So `habit.selectedImage.image` can be used with `Imageuiimage:)`. 

The `Spacer()` pushes the elements above and below to the left and right. 

Challenge: Use the other SwiftUI tools to style the text and features of the "cells".

### Make a Habit Cell view

Make a Habit cell view to replace the elements above and clean up your code. 

Choose File > New. Then Shoose `SwiftUI View` from ios tab under User Interface. 

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

Notice there is var `habit` at the top. This is a parameter that will come from outside this view. 

At the bottom in the preview make it look like this: 

```Swift
let habit = Habit(title: "Text", image: .grow)

struct HabitListCell_Previews: PreviewProvider {
    static var previews: some View {
        HabitListCell(habit: habit)
    }
}
```

Here you defined a `habit` and then passed that habit to the view. 

This preview section is used only for the testing. 

Use this view in `HabitList`. Open `HabitList` and make the following changes. 

```Swift
List(habits) { habit in
    HabitListCell(habit: habit)
}
```

Here you used the `HabitCell` view in place of the individual elements you used earlier. You passed the habit instance to the view in the same way you passsed the mocked up habit to the preview! 

This view does not have the navigation controller and the button in the upper right corner. You'll add these in the future. You will make the view first then add the navigation. 

## HabitDetails

The details view shows a single habit. It has a button to mark a habit completed for the day. You will mock up the view with a placeholder object for testing. 

Create a new SwiftUI file, choose File > New. Then Shoose SwiftUI View from iOS User Interface. 

Name this `HabitDetails`.

Define a Habit instance and add a habit variable to the View, then pass the variable to view in the preview. Something like this: 

```Swift
import SwiftUI

struct HabitDetails: View {
    var habit: Habit // bind a new 
    var body: some View {
        Text("\(habit.title)")
    }
}

// Define a variable
let codeHabit = Habit(title: "Code Daily", image: .code)

struct HabitDetails_Previews: PreviewProvider {
    static var previews: some View {
        // Pass the variable
        HabitDetails(habit: codeHabit)
    }
}
```

Now it's time to build the view. All content is in a `VStack`. The first three items will be `Image` followed by two `Text` views. 

Next there will be three `HStack` each with a `Text`, `Spacer`, and `Text`. 

Last is a `Button`. 

```Swift
struct HabitDetails: View {
    var habit: Habit
    var body: some View {
        VStack(spacing: 20) {
            Image(uiImage: habit.selectedImage.image)
                .resizable()
                .frame(width: 200, height: 200)
            Text("habit.title")
                .font(.system(size: 40))
            Text("Days \(habit.currentStreak)")
                .font(.system(size: 30))
            HStack {
                Text("Total:")
                Spacer()
                Text("\(habit.numberOfCompletions)")
            }
            HStack {
                Text("Best Streak:")
                Spacer()
                Text("\(habit.bestStreak)")
            }
            HStack {
                Text("Start Date:")
                Spacer()
                Text("\(habit.dateCreated.stringValue)")
            }
            Spacer()
            Button("Complete for day") {
                
            }
        }
        .padding(40)
    }
}
```

That works well but the Habit has all of the default values which will be 0 for most of the values. It's hard to tell if things are working. It's not possible to set these values easily with on the mocked up habit. 

Edit the Habit object and give it a new initializer. 

```Swift
init(title: String, image: Habit.Images, bestStreak: Int, currentStreak: Int, numberOfCompletions: Int) {
    self.title = title
    self.selectedImage = image
    self.id = UUID()
    self.bestStreak = bestStreak
    self.currentStreak = currentStreak
    self.numberOfCompletions = numberOfCompletions
}
```

This new initializer allows you to set the `bestStreak`, `currentStreak`, and `numberOfCompletions`. 

Initialize the habit in the Details view with some new values: 

```Swift
var codeHabit = Habit(title: "Code Daily", image: .code, bestStreak: 3, currentStreak: 2, numberOfCompletions: 12)
```

You should see these in the details view preview. 

Challenge: The `lastCompletedDate` could also be set. Add this to the alternate initializer. 

Then Make the button display "Completed For Today" if completed `completedToday` is true. 

<!-- ## Add Habit

Create the add habit view. This view displays a grid of icons, we choose an icon and tap the button to move on to the confirm habit view. 

Create a new SiwftUI View: File > New. Choose SwiftUI View. 

Name this file `AddHabit` -->






