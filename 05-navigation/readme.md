
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

### Navigation List

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
