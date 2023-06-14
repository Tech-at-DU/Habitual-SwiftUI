

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
