## HabitDetails and completing a habit

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

