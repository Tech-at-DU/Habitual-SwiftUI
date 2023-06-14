

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
