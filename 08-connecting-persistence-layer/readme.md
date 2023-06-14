
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
