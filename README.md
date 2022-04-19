# ACS 2420 Todo List

This tutorial creates a todo list for iOS using Xcode and Swift. 

## Learning Objectives 

- Use Swift 
- Use TableView 
- Use UserDefaults 

## Make the Todo List

This tutorial will use both code and storyboard. 
Some views will be initialized in the storyboard and 
others will be created using code. 

### Getting started 

Create a new Xcode project. Choose iOS, set the language
to Swift and the Choose storyboard. 

### Setup storyboard 

In main.storyboard add a UITableView object to the 
Viewcontroller. 

Pin all four edges to the edges of the view. 

Create an IBOutlet named: tableView in your 
ViewController.swift. This should be connected to the 
tableView in the storyboard. 

### Create a default table view cell 

Drag a UITableViewCell into the tableview in the storyboard. 

In the attributes inspector set the identifier to 
"cell".

### Setup table view data source

In ViewController.swift add an array. This will hold 
the list of todo items. This will be a placeholder 
for now. 

```Swift 
var todos = ["Hello", "World", "foo", "bar"]
```

Use an extension for the tableview data source methods. 
Add the following: 

```Swift
extension ViewController: UITableViewDataSource {
	func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
		...
	}

	func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
		...
	}
}
```

Use the `todos` array as the data source: 

```Swift 
func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
	return todos.count
}
```

Dequeue the reusable cell with it's idenitifer name: 

```Swift
func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
	let cell = tableView.dequeueReusableCell(withIdentifier: "cell", for: indexPath)
	cell.textLabel?.text = todos[indexPath.row]
	return cell
}
```

Be sure to use the same name you used as the 
identifier name for the cell you created in the storyboard!

Here you also set the text displayed by the cell from 
the data in your array. 

In `viewDidLoad` assign your view controller as the datasource: 

```Swift 
tableView.dataSource = self
```

**Sanity Check:** At this point running the app should
display the items in the list. 

### Setup table view delegate 

Testing the app now you should see the list of todos. 
Tapping a todo should select it and it should show 
a gray background. 

This is the default behavior of the default UITableViewCell. 

Let's change it so the cells don't stay keep the gray color. 

Add the UITableViewDelegate methods in an extension: 

```Swift 
extension ViewController: UITableViewDelegate {
	...
}
```

Add the viewController as the delegate for your tableView
in `viewDidLoad`: 

```Swift 
tableView.delegate = self
```

Back in the UITableViewDelegate Extension add the
`didSelectRow:at:IndexPath` method: 

```Swift
extension ViewController: UITableViewDelegate {
	func tableView(_ tableView: UITableView, didSelectRowAt indexPath: IndexPath) {
		...
	}
}
```

Add `tableView.deselectRow(at:indexPath)`:

```Swift
tableView.deselectRow(at: indexPath, animated: true)
```

This delegate method is called when a row is selected. 
The deselect method allows you to set the state of a 
cell in the tableview. 

### View details 

In this step, you will add a new view controller that will 
be displayed when you tap a row. 

Add a new view controller to the storyboard. Drag it from the 
library palette and place it next to the original View 
controller. 

In the identity inspector set the storyboard id to: "details".
The identity inspector is the icon to the left of the 
attributes inspector. 

This new ViewController needs a Viewcontroller file to 
back it up. Add a new Swift file. Name it: "DetailsViewController.swift"

Add this code to get started with a new UIViewController subclass: 

```Swift 
import UIKit

class DetailsViewController: UIViewController {
	...
}
```

Connect the new `DetailsViewController` to the new view 
controller in the storyboard. In the storyboard, choose the new 
view "details" view controller. In the Identity inspector
add DetailsViewController in the class field. You should
be able to choose this from the menu also. 

Add a Label to the details view controller in the storyboard. 
Set some options on the label. Make the text large and 
in charge. This will help people complete those tasks! 

Use some constraints to position the label in the center. 

Add an IBOUtlet for the new label in `DetailsViewController`.

It should look something like this: 

```Swift
@IBOutlet var todoLabel: UILabel!
```

### Add a navigation controller 

Add a navigation controller in the storyboard and embed your original 
view controller inside it. 

Do this by selecting the original View 
controller, it has the table view inside, click the tab at the top
in storyboard. 

Then choose: Editor > Embed In > Navigation Controller. 

This should create a new navigation controller to the left of 
the original view controller and connect the two with an arrow. 

### Navigate to the details view

In ViewController.swift find the UITableViewDelegate extension. 

In the didSelectRow method add the following code: 

```Swift 
let vc = storyboard?.instantiateViewController(withIdentifier: "details") as! DetailsViewController
navigationController?.pushViewController(vc, animated: true)
```

Notice you used the storyboard identifier for the view controller 
you want to create. 

You pushed the new view controller onto the stack of the navigation 
controller. 

This should display the new view but we'd also like to display the 
todo name in the label. 

In `DetailsViewController` add a variable to hold the todo:

```Swift
var todo: String = ""
```

Then add the `viewDidLoad` method: 

```Swift
override func viewDidLoad() {
	super.viewDidLoad()

	todoLabel.text = todo
}
```

Here you are setting the text of the label to the value stored 
in `todo`. 

You need to set `todo` when you create the detail view controller. 

In ViewController didSelectRow method, add the following, after 
you create the new view controller: 

```Swift 
vc.todo = todos[indexPath.row]
```

Here you are setting `vc.todo` to a value from the array of tasks. 

### Add Task ViewController

You need a new view controller that will be dedicated to adding new 
tasks. 

In the storyboard add a new view controller. Drag one out from the 
library and place it to the right of the others. 

The new view controller needs a class file to back it up. Make a 
new Swift file, name it `AddTodoViewController.swift`, add the following: 

```Swift 
import UIKit

class AddTodoViewController: UIViewController {
	...
}
```

Set `AddTodoViewController` as the class for the new view controller 
in the identity inspector in the storyboard. 

Also, set the storyboard id to "addtodo"

### Add Todo View controller

To add a new todo new need an interface. Set up a text input and 
the IBOutlets in your new view controller. 

In the storyboard, add a new TextField to the AddTodoView controller. 
Drag it from the library and style it. 

Add some constraints to the new Textfield. 

Add an IBOutlet for the new text field: 

```Swift
@IBOutlet var textField: UITextField!
```

Be sure it is connected to the storyboard! 

### Displaying AddTodoViewController

Let's display the AddTodoViewController. 

In `ViewController.swift` add a new method to display the 
new view controller: 

```Swift 
@objc func showAddTodo() {
	let vc = storyboard?.instantiateViewController(withIdentifier: "addtodo") as! AddTodoViewController
	navigationController?.pushViewController(vc, animated: true)
}
```

Notice! You are creating a new view controller with its storyboard id! Then 
you pushed this view controller onto the navigation stack. 

This method is marked: @objc because you will call it from an action!

Add a bar button item. In ViewController's viewDidLoad create a new bar 
button item and configure it. Add the following: 

```Swift
navigationItem.rightBarButtonItem = UIBarButtonItem(title: "Add Todo", style: .done, target: self, action: #selector(showAddTodo))
```

Notice! You're calling the `showAddTodo` method above from the action! 

Test your work. 

"Add Todo" should appear at the top right as a bar button item. 
Tapping this should display the Add Todo View controller. There 
should be a back button in the upper left to return to the todo 
list. 

### Adding a new todo

Since the todos array lives in `ViewController` and the name of 
the new todo item will be added in `AddTodoViewController` you 
need to facilitate some communication between the two. 

Start by adding a save button to the Add Todo view controller. 
Open `AddTodoViewController.swift`. 

Add a new method to handle tapping the save button. You'll 
make this button the next step. 

```Swift 
@objc func saveButtonTapped() {
	...
}
```

In the `viewDidLoad` method create a right bar button item: 

```Swift 
navigationItem.rightBarButtonItem = UIBarButtonItem(title: "Save", style: .done, target: self, action: #selector(saveButtonTapped))
```

Notice this button's action calls the method above. 

Since the array of todos is owned by `ViewController` that's where you
have to add a new one. Do this by passing from `ViewController` to 
`AddTodoViewController`. Still `AddTodoViewController` add a variable 
to hold the function: 

```Swift
var addTodo: ((String) -> Void)?
```

This is optional whose type is a function that takes 
a string and returns nothing (void.)

In `saveButtonTapped` add the following: 

```Swift
if let addTodo = addTodo {
	addTodo(textField.text!)
	navigationController?.popViewController(animated: true)
}
```

Here you check if `addTodo` is not nil. If not you call it 
with the text string entered in `textField`. Then pop this 
view controller off the stack returning us to the previous 
view controller! 

Open `ViewController` and find the `showAddTodo` method. Add a 
new method that will add new todo items to the array: 

```Swift
func addTodo(name: String) {
	todos.append(name)
	tableView.reloadData()
}
```

This method receives a string and appends it to the `todos` array. 
Then it reloads the data in the `tableView`. You need to reload 
the table view data or the new data will not be displayed in the
table view even though you appended something new to the array!

Now find `showAddTodo` and add the following: 

```Swift 
vc.addTodo = self.addTodo(name:)
```

This sets `addTodo` in `AddTodoViewController` to `addTodo` in this 
view controller when the we show `AddTodoViewController`.

Test your work. At this stage, you should be able to show the Add todo 
view controller, enter some text in the text field, and tapping save 
should return the previous view controller and show the entered text 
in the list. 

### Persisting Todos

Currently, the todos array is stored in memory and is lost when the 
app is closed. To make this into something more useful you need 
to save the todos somewhere and load them again when the app is 
restarted. 

There are many ways to do persist data in your iOS applications. 
This tutorial will work with the easiest method: UserDefaults. 
The UserDefaults method is easy to work with but limits what and 
how much data you can store.

UserDefaults stores simple values as key-value pairs. You can 
save things like strings and numbers with each value assigned 
to a key. 

Since this application needs to store a list we will convert the 
data in the list into a data string and store that string. 

Data Strings are not type-safe. To get around this you will use 
the codable interface to encode and decode the data string in a
type-safe way. 

### Make the TodoItem

Make a new Swift file, name it `TodoItem.swift`. 

```Swift 
import Foundation

struct TodoItem: Codable {
	var name: String
}
```

This is a struct, that implements Codable, with a single property. 

### Store TodoItems

In `ViewController` change the definition for `todos` from a list of 
Strings to a list of `TodoItem`. 

```Swift
var todos = [
	TodoItem(name: "Hello"),
	TodoItem(name: "World"),
	TodoItem(name: "Foo"),
	TodoItem(name: "Bar")
]
```

This should cause a couple problems. Check `addTodo`. This 
needs to be updated: 

```Swift
func addTodo(name: String) {
	todos.append(TodoItem(name: name))
	tableView.reloadData()
}
```

Look for `cellAtRow` there should be an error there. This 
needs to be updated also: 

```Swift 
cell.textLabel?.text = todos[indexPath.row].name
```

Look for `didSelectRowAt` there should be a problem here 
also. Update this: 

```Swift 
vc.todo = todos[indexPath.row].name
```

With these changes, you are now using an array of Structs 
to hold your todo items and everything should work like 
it did before. 

Note! With this arrangement, you would be able to add more 
properties to each todo item beyond just the name. 

### Persisting Todos

To persist Todoitems you'll add two new methods. One to load 
todos from userdefaults and the other to save todos to 
user defaults. 

Add this new method to ViewController: 

```Swift
func loadTodos() -> [TodoItem] {
	let defaults = UserDefaults.standard
	if let data = defaults.data(forKey: "todos") {
		return try! PropertyListDecoder().decode([TodoItem].self, from: data)
	}
	return []
}
```

This method gets the user defaults object as `defaults`. Then 
tries to get the value for the key "todos" from the User defaults 
storage. This might not be there so the user defaults returns an 
optional value. 

This is a data object that needs to be decoded. Use the 
`PropertyListDecoder` to turn the data found into an array of TodoItems. 

If the "todos" key doesn't exist on UserDefaults we return an empty
array. 

At the top of the ViewController class change the defintion of todos to: 

```Swift
var todos = [TodoItem]()
```

This will start the app with an empty array of todos. 

In `viewDidLoad` load your todos from user defaults by calling 
the `loadTodos` method: 

```Swift
todos = loadTodos()
```

From here everything should work as before with 
two exceptions. The app will start with an empty list
and todos will not be persisted after relaunching the app. 

Add a method to save todos. 

```Swift
func saveTodos(todos: [TodoItem]) {
	if let data = try? PropertyListEncoder().encode(todos) {
		let defaults = UserDefaults.standard
		defaults.set(data, forKey: "todos")
	}
}
```

This method takes an array of TodoItems and tries to encode them 
as data. This should work because TodoItem is codable. next, we get
the user defaults object. Then save the data to user defaults 
under the key "todos". 

Save the todos when you create a new todo item. Find the 
`addTodo` method and add the following: 

```Swift
saveTodos(todos: todos)
```

Here you are calling the `saveTodos` method with the todos array. 

### Add priority to a todo item

Imagine you want to mark todo items as completed when they are done. 
To do this we need to store a Bool along with the name of the todo. 

Go to TodoItem.swift and modify the struct. Add this:

```Swift 
var completed: Bool
```

This will generate an error in `ViewController`'s `addTodo` method. 
Here you are making a new TodoItem but initializing it without
the new completed property! 

Modify the `addTodo` method: 

```Swift
todos.append(TodoItem(name: name, completed: false))
```

This should solve the error in the code editor. 

Trying to run the app now will generate another error. This time 
an error occurs because the data stored in Userdefaults doesn't match 
the struct we are trying to decode to. 

Remember the old data was just the name property. The new struct 
has a name and completed property. 

An easy fix is to delete these "todos" from Userdefaults. 

Add the following to `viewDidLoad` in `ViewController`. 

```Swift
UserDefaults.standard.set(nil, forKey: "todos")
```

Be sure this line happens before you load the saved todos! 

Running your app now should start the app fresh with an empty list. 
Adding new items should save them as before. 

NOTE! The line above is deleting all of the old todos when the view controller 
loads! After you stop your app be sure to comment on this line out
before running it again! 

```Swift 
// UserDefaults.standard.set(nil, forKey: "todos")
```

Note! If you make changes to `TodoItem` you'll need to 
uncomment this line and run your app once to reset your 
data stored in user defaults. 

### Displaying completed

Now that we have a property to say when a todo item is completed
we should show that.

Modify the `DetailsVeiwController` to show the completed state. 
In main.storyboard find the DetailViewController. It has the 
UILabel. 

Remove the constraints from the UIlabel by selecting it and 
choosing "Clear Constraints" from the "Resolve Autolayout issues"
button in the lower right. 

Drag a UISwitch from the library palette into the view controller. 

Add some constraints to arrange the label and Switch in the
center. I used a Stackview but you can do this your own way. 

Add an IBOutlet for the switch and connect the switch in 
storyboard to the IBOutlet in the view controller. I named
mine: 

```Swift
@IBOutlet var completedSwitch: UISwitch!
```

Let's show the completed property when we show the details
view controller. 

Currently `DetailsViewController` defines var `todo` as a string. 
This holds just the name of the todo item. Let's make that type
`TodoItem`. We will also make it type optional since it won't 
have a value unless it's been set. 

```Swift
var todo: TodoItem?
```

This will cause a couple problems that need to be addressed. 

In `viewDidLoad` of `DetailsViewController`:

```Swift
if let todo = todo {
	todoLabel.text = todo.name
	completedSwitch.isOn = todo.completed
}
```
Here you unwrapped the optional `todo` safely and then 
used its properties to display the name and completed
values. 

Find `didSelectRowAt` in `ViewController`. Here you need to set 
`vc.todo` to the `TodoItem` instance instead of just the name String. 

```Swift
vc.todo = todos[indexPath.row]
```

TableView cells can be customized but the default UITableViewCell
is packed with features. You can swipe to show options, it has 
the text label, and it can also show an icon on the right side. 

Show the check mark for completed todos. In `ViewController` find 
`cellForRowAt` and refactor the code there: 

```Swift
let todo = todos[indexPath.row]
cell.textLabel?.text = todo.name
cell.accessoryType = todo.completed ? .checkmark : .none
```

Might as well put the todo item in a variable for convenience! 

The `accessoryType` sets the built-in icon. 

Note! Currently, you won't see the checkmark because we haven't 
completed any todos and there is no way to mark them complete! 

### Marking todos complete

Marking todos completed in the details view will require that 
that view communicates with the main view controller where 
the array of todo items lives. 

We also need to know the index of the todo item in the array 
of todos that need to be updated. 

Add a couple properties to `DetailsViewController`:

```Switch
var todoIndex: Int?
var setCompletedForTodoAt: ((Int, Bool) -> Void)?
```

The first is an Int and will be the index of the todo. 

The second is a function we can call to update the 
status of the todo. This is a function that takes 
an Int and a Bool and returns nothing. 

Notice! bot of these are optionals since they get their 
values from outside this class!

Add an IBAction for the UISwitch in `DetailsViewController`:

```Swift
@IBAction func switchToggled() {
	if let setCompletedForTodoAt = setCompletedForTodoAt, let todoIndex = todoIndex {
		setCompletedForTodoAt(todoIndex, completedSwitch.isOn)
	}
}
```

Here we unwrap both of the new optionals and then 
call `setCompletedForTodoAt` with the index of the todo 
and the new value for its status. 

In `ViewController` add a new method to update the completed 
status of a todo at an index: 

```Swift
func setCompletedForTodoAt(index: Int, completed: Bool) {
	todos[index].completed = completed
	saveTodos(todos: todos)
	tableView.reloadData()
}
```

Here you are setting the status of the todo at the index in todos. 
Saving all of your todos to user defaults. 
Last, reloading the tableview data. 

To make this work you need to set the `todoIndex` and the 
`setCompletedforTodoAt` in the detail view controller. 

Find `didSelectRowAt` in the UITableVIewDelegate extension. Add the 
following: 

```Swift
vc.todoIndex = indexPath.row
vc.setCompletedForTodoAt = setCompletedForTodoAt
```

Test your work. With these new options in place, you should be able
to create new todos, and set their completed status. The todos
]along with the completed status should persist between launches 
of the app. 

### Stretch Challenges 

If you're done early with this project you should try the stretch chellenges here. See how far you can go with these to measure your knowledge and ability. 

Strech challenges: 

###  Work on the design and style of the app. 

- Improve the type styles. Adjust the font size, weight, and color. 
- Style the view controllers. Set the background color, and titles. 
- Add some labels or emojis to the view controllers to clarify what is going on in each.  

### Add a sort button to sort the list of todos and move completed todos to the bottom. 

- You'll to add some user interface to sort. This could be a bar button on the navigation bar or a button on the screen. If you add the button to the screen you'll need to leave some room at the top or bottom of the tableview. 
- Use Array.sort() to sort the todos list on the completed property. You should save the the todos and reload the tableview data after sorting! 

### Add more features to each TodoItem. This could be a notes field, or category. 

- Add your new fields to the TodoItem class. Be sure to delete the todos from user defaults or you will get an error when loading data that doesn't match the new model. See: [Add priority to a todo item](#Add-priority-to-a-todo-item)
- Add a category field. This can be a string with any name like: Home, School, Work, etc. 
- Add a priority field. This can be an integer value like 1, 2, 3. You can display these in the UI with a segmented controller labeled: Low, Normal, High. 
- Add a notes or description field. This can be a longer string with some notes for the todo item. You can add another textfield or label to display these in the detail screen. 
- You can also display notes in the tableview cell. There is a default tableviewcell that has a subtitle style. If you set the Cell Style in the storyboard you can put text in the `cell.detailTextLabel?.text` which will appear below the text in: `cell.textLabel?.text`
- You can also expand todo items with a date. Get the date when a todo is created. Save this to the TodoItem in a property. Follow the guide here: https://www.hackingwithswift.com/books/ios-swiftui/working-with-dates
- Add a due date for a todo item. You can do this by a adding a UIDatePicker to the AddTodoViewController. 

#### Sort by Priority

- If you did the priority field try sorting by priority. 

#### Sorting by Dates

- If you added dates in the stretch challenge above try sorting them by date. Where the oldest are listed first. 

### Delete todos. Try deleting a todo from the detail screen. 

- Try deleting a todo from the list by swiping. Swipe to reveal options on a table view cell is a built-in feature: https://www.hackingwithswift.com/example-code/uikit/how-to-swipe-to-delete-uitableviewcells
- Use the editing style to mark todos complete
- Remmeber to save your todos to user defaults and reload your tableview data after removing a todo item! 
