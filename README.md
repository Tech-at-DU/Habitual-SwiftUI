# Habitual SwiftUI 

Create a new Xcode project and set the Language to Swift and SwiftUI as the Interface. 

Create the PersistenceLayer, Habit Class, and Date Extensions. These files provide infrastructure for the app. If you completed the Habitual Storyboard app you can copy those files from that project into your project. 

You also need to get the assets used by the project. Copy or import the images for the habit icons into your SwiftUI project. 

## Views to Build

You will build each of the views of the project first with mock-up data. Then you will connect these and make the app function. 

The views you need to create are: 

- HabitList - HabitTableViewController
- HabitDetails - HabitDetailsViewController
- AddHabit - AddHabitViewController
- ConfirmHabit - ConfriemHabitViewController

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

## HabitDetails

The details view shows a single habit. It has a button to mark a habit completed for the day. You will mock up the view with a placeholder object for testing. 

Create a new SwiftUI file, and choose File > New. Then choose SwiftUI View from iOS User Interface. 

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

Next, there will be three `HStack` each with a `Text`, `Spacer`, and `Text`. 

Last is a `Button`. 

```Swift
struct HabitDetails: View {
  var habit: Habit
  var body: some View {
  VStack(spacing: 20) {
    Image(uiImage: habit.selectedImage.image)
      .resizable()
      .frame(width: 200, height: 200)
    Text(habit.title)
      .font(.system(.title))
    Text("Current Streak")
    Text("\(habit.currentStreak)")
        .font(.system(.largeTitle))
    
    HStack {
      Text("Total Completions")
        Spacer()
        Text("\(habit.numberOfCompletions)")
      }
      HStack {
        Text("Best Streak")
        Spacer()
        Text("\(habit.bestStreak)")
      }
      HStack {
        Text("Start Date")
        Spacer()
        Text(habit.dateCreated.stringValue)
      }
      Spacer()
      Button("Complete for Today") {
      
      }
    }.padding(30)
  }
}
```

That works well but the Habit has all of the default values which will be 0 for most of the values. It's hard to tell if things are working. It's not possible to set these values easily with the mocked-up habit. 

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

To test the button is displaying the correct label you'll need to create a Habit with a date other than today. To do this you'll need to add an alternative initializer to the Date Extension. 

Add the following to `DateExtension.swift`:

```Swift
init(dateString:String) {
  let dateStringFormatter = DateFormatter()
  dateStringFormatter.dateFormat = "yyyy-MM-dd"
  dateStringFormatter.locale = NSLocale(localeIdentifier: "en_US_POSIX") as Locale
  let d = dateStringFormatter.date(from: dateString)!
  self.init(timeInterval:0, since:d)
}
```

This will allow you to initialize a date instance with a string in the form: "yyyy-mm-dd" For example: "2023-06-03" would initialize a date as June 5, 2023. 


## Add Habit

Create the add habit view. This view displays a grid of icons, we choose an icon and tap the button to move on to the confirm habit view. 

Create a new SiwftUI View: File > New. Choose SwiftUI View. 

Name this file `AddHabit`

This view will replace the collection view from the original project. It will display a group of icons in a grid. 

The icons will come from `Habit.images` enum. You need to enumerate these images in the view. 

Start here:

```Swift
struct AddHabit: View {
  var body: some View {
  VStack {
    ForEach(Habit.Images.allCases, id: \.rawValue) { item in
      Image(uiImage: item.image)
    }
    }
  }
}
```

I used a VStack to arrange the list of images in a vertical column. 

ForEach is looping over all cases in `Habit.Images`. You must set an id for each item! We use the rawValue for the id. I know the syntax is a little weird here.

This is working but it's not right. VStack will not allow us to create a grid unless we combine it with HStack but that would be too complicated. 

SwiftUI provides a special view for grids called LazyGrid. You need to combine this with another class called GirdItem. 

Start by adding an array of GridItems at the top. These describe each item in a row. In this case, those items will have a min-width of 80. 

```Swift
struct AddHabit: View {
  let gridConfig = [
    GridItem(.adaptive(minimum: 80)),
    GridItem(.adaptive(minimum: 80)),
    GridItem(.adaptive(minimum: 80))
  ]
  
  var body: some View {
  VStack {
    ForEach(Habit.Images.allCases, id: \.rawValue) { item in
        Image(uiImage: item.image)
      }
    }
  }
}
```

Next, define a lazy grid: 

```Swift
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
  }
  }
}
```

Here LazyVGrid is used to create a vertical grid. Columns are set by the `gridConfig`. There will be three columns and each column uses the same `GridItem`.

Last, add a Button and a Spacer. I also added some padding on the VStack to push everything inward. 

```Swift
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
      Button("Select Icon") {
      
      }
    }.padding(30)
  }
}
```

## ConfirmHabit View

Next, create the confirm habit view. Here you enter the name of the habit and tap the Create button to create that habit. This view shows the icon from the previous view. 

Create a new SwiftUI View. Name it ConfirmHabit. 

Start with a VStack and an Image. I made the image resizable and gave it a frame with a width and height of 200. I also added padding 30 on the VStack to put space around everything. 

```Swift
struct ConfirmHabit: View {
  var body: some View {
  VStack(spacing: 30) {
    Image("book")
      .resizable()
      .frame(width: 200, height: 200)
    }.padding(30)
  }
}
```

We need a TextField under the Image. The TextField holds the value entered by a user. For this to work you need to define a state var. 

The TextField takes two parameters. The first is a string value for placeholder text that appears when the field is empty and the second is the state variable that will hold the value entered into the field. 

```Swift
struct ConfirmHabit: View {
  @State private var title: String = ""
  var body: some View {
  VStack(spacing: 30) {
    Image("book")
      .resizable()
      .frame(width: 200, height: 200)
    TextField("Enter Habit", text: $title)
      .padding(10)
      .border(.black)
    }.padding(30)
  }
}
```

Here the state var is named `title`. When used in the view we prefix the name with `$`. 

Last, add a Spacer and a Button.

```Swift
struct ConfirmHabit: View {
 @State private var title: String = ""
 var body: some View {
 VStack(spacing: 30) {
  Image("book")
    .resizable()
    .frame(width: 200, height: 200)
  TextField("Enter Habit", text: $title)
    .padding(10)
    .border(.black)
  Spacer()
  Button("Create Habit") {
    //
  }
  }.padding(30)
  }
}
```

This should create a button and push it to the bottom and everything else to the top. 

## Views Complete

That completes the 4 views that are needed for this app. You created four views. Mocked up values that would come from outside those views and defined internal state that would be used by a view. 

## Navigation 

The next steps are to get the navigation working. In this app list items in the `HabitList` view take us to the `HabitDetails` view. Tapping the + in the upper right (we need to add this) takes us to the `AddHabit` view, there tapping the button at the bottom takes us to the `ConfirmHabit` view, here we create a new habit and return to the habit list. 

Start in `HabitList`, wrap the `List` in a `NavigationView`. 

```Swift
NavigationView {
  List(habits) { habit in
    HabitListCell(habit: habit)
  }
}
```

The effect of this should be invisible. You've wrapped the current view in a navigation controller, the view should display the same as before. 

Navigation controllers have a navigation bar at the top. This is where the title of the view and possibly buttons can be displayed. 

To add the title add a new `Text` element inside the `NavigationView` and declare it as the `navigationTitle` like this: 

```Swift
NavigationView {
  Text("") // Navigation title Text
    .navigationTitle("Habits") // navigation title
  List(habits) { habit in
    HabitListCell(habit: habit)
  }
}
```

This should show the label "Habits" at the top in the "navbar". You can style the `Text` element like you would anywhere else in SwiftUI. 

I'm not sure why a string in `Text` shows up as sort of a subheading. Try this: 

```Swift
NavigationView {
  Text("Developing Good Habits!") // Navigation title Text
    .navigationTitle("Habits") // navigation title
  List(habits) { habit in
    HabitListCell(habit: habit)
  }
}
```

You should see the text: "Developing Good Habits!" below the title. There is probably a reason. I'm not sure why as I write this. 

Next, add a button at the top. Do this by adding a "toolbar" to the `VStack`. It feels like you should be added to the `NavigationView` but realize the `NavigationView` is outside of the current view. Think of the `NavigationView` as a container. When you navigate to another view you may not want a toolbar. So the toolbar goes inside the `NavigationView` so it is part of the current view. 

```Swift
NavigationView {
  VStack {
    Text("Developing Good Habits!") // Navigation title Text
      .navigationTitle("Habits") // navigation title
    List(habits) { habit in
      HabitListCell(habit: habit)
    }
    }
    .toolbar {
    Button("Add") {
      print("Add tapped!")
    }
  }
}
```

This should display "Add" in the upper right. 

Now add some navigation. To add navigation swap the Button for a `NavigationLink`. The `NavigationLink` takes the view to navigate to and the view that will be the link, in our case it can be a `Text` but it could also be an Image. 

Try this: 

```Swift
NavigationView {
  VStack {
    Text("Developing Good Habits!") // Navigation title Text
      .navigationTitle("Habits") // navigation title
    List(habits) { habit in
      HabitListCell(habit: habit)
    }
    }
    .toolbar {
    NavigationLink(destination: AddHabit()) {
      Text("Add!")
    }
  }
}
```

Notice the `NavigationLink` it looks like this: 

```Swift
NavigationLink(destination: AddHabit()) {
  Text("Add")
}
```

The `Text("Add")` is displayed and `destination: AddHabit()` provides the view to navigate to! 

Try it out. This should display the `AddHabit` SwiftUI view. It also provides a back button! 

Try this: 

```Swift
NavigationLink(destination: AddHabit()) {
  Image(systemName: "globe")
}
```

This should swap the text for an image of a globe. The globe is one of the built-in icons. The SF Font has all of the icons built-in. If you plan to do a lot of iOS development you should download the Symbols app, it will show you all of the icons! 

https://developer.apple.com/sf-symbols/

Try this: 

```Swift
NavigationLink(destination: AddHabit()) {
  Image(systemName: "plus")
}
```

Or:

```Swift
NavigationLink(destination: AddHabit()) {
  Image(systemName: "plus.circle")
}
```

or:

```Swift
NavigationLink(destination: AddHabit()) {
  Image(systemName: "plus.circle.fill")
}
```

## Navigation List

The list will work the same. You can Wrap the `HabitListCell` view in a `NavigationLink`. The one added detail is that the `HabitListCell` requires that you provide a habit object for it to display. 

Try this: 

```Swift
List(habits) { habit in
  NavigationLink(destination: HabitDetails(habit: habit)) {
    HabitListCell(habit: habit)
  }
}
```

Notice that `NavigationLink` takes two parameters: `destination` which is the view to navigate to, and the second parameter is the view to display that will be the link, this is the habit list cell.

Since the destination view requires a habit we provide the habit from the list iteration. 

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

## Connect to the PsersitenceLayer

Connect PersistenceLayer to our SwiftUI app will require a few changes. Let's make it a Singleton. A singleton is a class configured so only one instance is ever created. 

Singleton is a programming pattern. In some cases, you will only want to create a single instance of a class. This app, the PersistanceLayer is an example of this. We only ever want one instance of PersistenceLayer since the persistence layer has an array of habits that are shared across all views. 

Open PersistenceLayer and make a couple of changes. 

Change PersistenceLayer from a struct to a class. This is needed since Structs are immutable. 

```Swift
class PersistenceLayer {
  ...
}
```

Next, make PersistenceLayer an ObservableObject. This will allow our SwiftUI views to update when "Published" of this object change. 

```Swift
class PersistenceLayer: ObservableObject {
  ...
}
```

Add a new static property `sharedInstance`. This property is static, meaning it is accessed from the class not from an instance of the class! Assign this property an instance of the class. 

```Swift
class PersistenceLayer: ObservableObject {
  static let sharedInstance = PersistenceLayer()
  ...
```

From here on you will always PersistenceLayer like this: 

```Swift
PersistenceLayer.sharedIntance
```
In this way any time the sharedInstance is accessed it will always be the single instance of this class that is stored on the static property. 

Change the definition of the habits property to: 

```Swift
@Published var habits: [Habit] = []
```

A published var is a variable that lets other objects know when it's updated. In this way when a new item is added to the list our views can update. 

Remove a few things. Find `loadHabits` and remove "private mutating"

```Swift
func loadHabits() {
  ...
}
```

Find `createNewHabit` and remove `@discardableResult mutating`

```Swift
func createNewHabit(name: String, image: Habit.Images) -> Habit {
 ...
}
```

Also, remove the return type and the return value. We aren't using these. 

```Swift 
func createNewHabit(...) {
  ...
  // return newHabit <- Remove this
}
```

Find `delete` and remove `mutating`

```Swift
func delete(_ habitIndex: Int) {
  ...
}
```

Find `markHabitAsCompleted` and remove `mutating`

```Swift
func markHabitAsCompleted(_ habitIndex: Int) -> Habit {
  ...
}
```

Find `swapHabits` and remove `mutating`

```Swift
func swapHabits(habitIndex: Int, destinationIndex: Int) {
  ...
}
```

Find `setNeedsToReloadHabits` and remove `mutating`. 

```Swift
func setNeedsToReloadHabits() {
  ...
}
```

The original PersistenceLayer was created as a struct. Structs are immutable. So mutating was used to allow some of the properties to be mutated. By changing PersistenceLayer to a class its properties can be mutated so the mutating keyword is not needed. 

### Updating the HabitList

Open HabitList make a couple of changes. These changes will use the single instance of PersistanceLayer and mark that as an Observable Object. This will allow this view to update when the Observable object is updated. 

Find: 

```Swift
@State var habits: [Habit] = PersistenceLayer.sharedInstance.habits
```

Change this to: 

```Swift
@ObservedObject var pl = PersistenceLayer.sharedInstance
```

I removed the `habits: [Habit]` array. You'll now get the habits array from PersistenceLayer like this: `pl.habits. 

Find: 

```Swift
List(habits) {
  ...
}
```

Change this to: 

```Swift
List(pl.habits) {
  ...
}
```

### Update ConfirmHabit

Confirm Habit creates a new habit when the button is tapped. You'll create the new habit by calling: `PersistenceLayer.createHabit()`. 

In ConfirmHabit find the button. 

```Swift
Button("Create Habit") {
  PersistenceLayer
    .sharedInstance
    .createNewHabit(name: title, image: image ?? Habit.Images.bulb)
}
```

Here you call `createHabit` on the single instance of `PersistanceLayer` and set the title and image. Since the image is optional we check with the nil coalescing operator and use the bulb if we find nil. 

Take a look at PersistenceLayer and find the `markHabitAsCompleted` function. This function takes the index of a habit and returns a habit. 

Since what we have here doesn't supply the index this will have to change. Rewrite this function so that it takes a `Habit` as a parameter. 

It will use the id of the habit to locate the correct habit. 

```Swift
func markHabitAsCompleted(_ updateHabit: Habit) -> Habit? {
  for i in habits.indices {
    if habits[i].id == updateHabit.id {
      habits[i].numberOfCompletions += 1
      if let lastCompletionDate = habits[i].lastCompletionDate, lastCompletionDate.isYesterday {
      habits[i].currentStreak += 1
    } else {
      habits[i].currentStreak = 1
    }
    if habits[i].currentStreak > habits[i].bestStreak {
      habits[i].bestStreak = habits[i].currentStreak
    }
    let now = Date()
    habits[i].lastCompletionDate = now
    self.saveHabits()
    return habits[i]
    }
  }
  return nil
}
```

This function now loops over all of the habits. Matches the supplied habit to a habit in the list, then updates that habit and returns it. 

If the habit is not found it returns nil. For this reason, the return type has been changed to `Habit?` (Optional Habit.)

### HabitDetails and completing a habit

The last stage is to allow marking a habit complete in the Details View. The original tutorial uses the index of a habit to mark it as complete. 

Open HabitDetail.swift. This view is passed a Habit object and it displays that. If you complete a habit we will update the habit and we want the view to update also. For this reason mark `habit` as `@State`. 

```Swift
struct HabitDetails: View {
  @State var habit: Habit 
  ...
```

As a State variable changing habits will cause this view to update. 

Next, find the button. Make these changes: 

```Swift
Button(habit.completedToday ? "Completed for Today!" : "Mark as Completed") {
  habit = PersistenceLayer.sharedInstance.markHabitAsCompleted(habit)!
}
```

Here if `habit.completedToday` is true the button displays "Completed for Today!" otherwise: "Mark as Completed". 

In the code block for the button, you call the persistence layers `markHabitAsCompleted` method and pass the habit. 

This method returns a habit so you assign that to the @State var `habit`. Assigning a new value causes the view to be updated. 

Last, disable the button when the habit is marked completed. 

```Swift
Button(habit.completedToday ? "Completed for Today!" : "Mark as Completed") {
  habit = PersistenceLayer.sharedInstance.markHabitAsCompleted(habit)!
}.disabled(habit.completedToday)
```

Add `.disabled()` after the button. Notice that disabled takes a bool you supply `habit.isCompletedToday` which is true if the habit was completed today! 

## Conclusion

This tutorial is complete! You covered: 

- SwiftUI includes many of the major view elements. 
- @State which updates a view when a variable changes
- @Observable which allows your views to observe the properties of an object

