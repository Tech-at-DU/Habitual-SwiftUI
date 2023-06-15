
### Update ConfirmHabit

Confirm Habit creates a new habit when the button is tapped. You'll create the new habit by calling: `PersistenceLayer.createHabit()`. 

In ConfirmHabit find the button. 

```Swift
Button("Create Habit") {
  PersistenceLayer
    .sharedInstance
    .createNewHabit(name: title, image: image ?? Habit.Images.bulb)
	NavigationUtil.popToRootView()
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