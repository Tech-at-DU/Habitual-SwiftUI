
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
