

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