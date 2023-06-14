

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
