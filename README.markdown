# VRGSoft Swift Style Guide.
### Updated for Swift 4.2

This style guide is based on Apple’s Swift standard library style but it has distinctions due to our vision of best practice.

## Table of Contents

* [Code formatting](#code-formatting)
* [Naming](#naming)
  * [Use type inferred context](#use-type-inferred-context)
  * [Language](#language)
* [Code organization](#code-organization)
  * [Protocol conformance](#protocol-conformance)
  * [Unused code](#unused-code)
* [Classes and structures](#classes-and-structures)
  * [Use of `self`](#use-of-self)
  * [Computed properties](#computed-properties)
* [Function declarations](#function-declarations)
* [Closure expressions](#closure-expressions)
* [Types](#types)
  * [Constants](#constants)
  * [Optionals](#optionals)
* [Access control](#access-control)
* [Control flow](#control-flow)
* [Using `guard` statement](#using-guard-statement)
* [Semicolons](#semicolons)
* [Parentheses](#parentheses)
* [Multi-line string literals](#multi-line-string-literals)


## Code Formatting

Avoid too long lines.
Use 4 spaces for tabs.
Do not add a space before the colon when writing a type. Add a space after a comma. Also there should be a space before and after a binary operator.
Do not place opening braces on new lines. Move one line after class declaration.

**Preferred**:
```swift
class SomeClass: BaseClass {

	if condition {
    
		//do something
	}
}
```

**Not Preferred**:
```swift
class SomeClass : BaseClass
{
	if condition 
	{
		//do something
	}
}
```
We follow Xcode's recommended indentation style. When calling the function that has many parameters, put each argument on a separate line with a single extra indentation.
When dealing with an array or dictionary large enough split it into multiple lines, treat the [ and ] as if they were braces in a method, if statement, etc. 

**Preferred**:
```swift

let params: [String: String] = [
	"key1": "value1",
	"key2": "value2",
	"key3": "value3",
]

func functionWithManyParams(param1: Int,
							  param2: Int,
							  param3: Int) {
	//code
}
```


## Type declaration

Always indicate the type when declaring a variable. This speeds the project build process.

**Preferred**:
```swift
class SomeClass {
	
	let someText: String = "Text"
}
```

**Not Preferred**:
```swift
class SomeClass {

	let someText = "Text"
}
```


## Naming

Descriptive and consistent naming makes software easier to read and understand. Use Swift naming conventions described in [API Design Guidelines](https://swift.org/documentation/api-design-guidelines/). Some key takeaways include:

- prioritizing clarity over brevity
- using camelCase (initial lowercase letter) for function, method, property, constant, variable, argument names, enum cases, etc.
- using PascalCase for type names (e.g. struct, enum, class, typedef, associatedtype, etc.).
- including all needed words while omitting needless words
- using names based on roles, not types
- sometimes compensating for weak type information
- striving for fluent usage
- naming methods for their side effects
  - verb methods follow the -ed, -ing rule for the non-mutating version
  - noun methods follow the formX rule for the mutating version
  - boolean types should read like assertions
  - protocols that describe _what something is_ should read as nouns
  - protocols that describe _a capability_ should end in _-able_ or _-ible_
- giving the same base name to methods that share the same meaning
- avoiding overloads on return type
- choosing good parameter names that serve as documentation

### Use type inferred context

Use compiler inferred context to write shorter, clear code.

**Preferred**:
```swift
let selector: Selector = #selector(viewWillAppear)
let view: UIView = UIView(frame: .zero)
view.backgroundColor = .white
```

**Not Preferred**:
```swift
let selector: Selector = #selector(ViewController.viewWillAppear)
let view: UIView = UIView(frame: CGRect.zero)
view.backgroundColor = UIColor.white
```

### Language

Use US English spelling to match Apple's API.

**Preferred**:
```swift
let letterSpacing: Int = 14
```

**Not Preferred**:
```swift
let latterSpacing: Int = 14
```


## Code organization

Use marks to organize your code into logical blocks of functionality. Class should have next structure:

```swift
class ViewController: UIViewController {

    // MARK: deinit
    
	// MARK: init
	
	// MARK: View controller lifecycle methods
	
	// MARK: Base overrides
	
	// MARK: Logic
	
	// MARK: Actions
	
	/* delegate and protocols methods */
	// MARK: UITableViewControllerDelegate
}
```

Simplified syntax should be used.

**Preferred**:
```swift
var array: [String] = []
```

**Not Preferred**:
```swift
var array: [String] = [String]()

//or

var array: Array<Int> = []
```
Create typealias only if the closure occurs two or more times.

In methods with a return parameter, should be created a variable `result` which will be returned.

**Preferred**:
```swift
func createTableViewFooter() -> UIView {

	let result: MyTableViewFooter = MyTableViewFooter.loadFromNib()

    return result
}
```

**Not Preferred**:
```swift
func createTableViewFooter() -> UIView {

	return MyTableViewFooter.loadFromNib()
}
```

### Protocol conformance

Do not add a separate extension for the protocol methods. Do it only for `final` classes.

**Preferred**:
```swift
class ViewController: UIViewController, UITableViewDataSource {

	// class stuff here


	// MARK: - UITableViewDataSource
    
	// table view data source methods
}
```
**or**:

```swift
final class ViewController: UIViewController {

	// class stuff here
}

extension ViewController: UITableViewDataSource {

	// table view data source methods
}
```

**Not Preferred**:
```swift
class ViewController: UIViewController {

	// class stuff here
}

extension ViewController: UITableViewDataSource {

	// table view data source methods
}
```

### Unused code

Unused (dead) code, including Xcode template code and placeholder comments should be removed. 

**Preferred**:
```swift
override func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
    
    return count
}
```

**Not Preferred**:
```swift
override func didReceiveMemoryWarning() {

    super.didReceiveMemoryWarning()
    
    // Dispose of any resources that can be recreated.
}

override func numberOfSections(in tableView: UITableView) -> Int {
    
    // #warning Incomplete implementation, return the number of sections
    return 1
}

override func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
    
    // #warning Incomplete implementation, return the number of rows
    return count
}
```


## Classes and Structures

### Use of `self`

Avoid using `self` since Swift does not require it to access an object's properties or invoke its methods.

Use self only when required by the compiler (in closures, or in initializers to disambiguate properties from arguments). In other words, if it compiles without `self` then omit it.

### Computed properties

For read-only property omit the get clause. The get clause is required only when a set clause is provided.

**Preferred**:
```swift
var diameter: Double {

    return radius * 2
}
```

**Not Preferred**:
```swift
var diameter: Double {
    get {
        return radius * 2
    }
}
```


## Function declarations

Keep short function declarations on one line including the opening brace.

Don't use `(Void)` to represent the lack of input and function outputs simply use `()`. Use `Void` instead of `()` for closure .

**Preferred**:

```swift
func updateConstraints() {

  // some code
}

typealias CompletionHandler = (result) -> Void
```

**Not Preferred**:

```swift
func updateConstraints() -> Void {

  // some code
}

typealias CompletionHandler = (result) -> ()
```


## Closure expressions

Use trailing closure syntax only if there's a single closure expression parameter at the end of the argument list. Give the closure parameters descriptive names.

**Preferred**:
```swift
UIView.animate(withDuration: 1.0) {

    self.myView.alpha = 0
}

UIView.animate(withDuration: 1.0, animations: {

    self.myView.alpha = 0
}, completion: { finished in

    self.myView.removeFromSuperview()
})
```

**Not Preferred**:
```swift
UIView.animate(withDuration: 1.0, animations: {

    self.myView.alpha = 0
})

UIView.animate(withDuration: 1.0, animations: {

    self.myView.alpha = 0
}) { f in
    self.myView.removeFromSuperview()
}
```


## Types

Always use Swift's native types and expressions when available. Swift offers bridging to Objective-C so you can still use the full set of methods as needed.

**Preferred**:
```swift
let width: Double = 120.0                                    // Double
let widthString: String = "\(width)"                         // String
```

**Less Preferred**:
```swift
let width: Double = 120.0                                    // Double
let widthString: String = (width as NSNumber).stringValue    // String
```

**Not Preferred**:
```swift
let width: NSNumber = 120.0                          // NSNumber
let widthString: NSString = width.stringValue        // NSString
```

In drawing code, use `CGFloat` if it makes the code more succinct by avoiding too many conversions.

### Constants

Constants are defined using the `let` keyword and variables with the `var` keyword. Always use `let` instead of `var` if the value of the variable will not change.

**Tip:** A good technique is to define everything using `let` and only change it to `var` if the compiler complains!

### Optionals

Declare variables and function return types as optional with `?` where a `nil` value is acceptable.

Prefer optional binding to implicitly unwrapped optionals in most other cases.

When naming optional variables and properties, avoid naming them like `optionalString` or `maybeView` since their optional-ness is already in the type declaration.

For optional binding, shadow the original name whenever possible rather than using names like `unwrappedView` or `actualLabel`.

**Preferred**:
```swift
var subview: UIView?
var volume: Double?

// later on...
if let subview = subview, let volume = volume {

    // do something with unwrapped subview and volume
}

// another example
UIView.animate(withDuration: 2.0) { [weak self] in

    guard let self = self else { 
        return
    }
    
    self.alpha = 1.0
}
```

**Not Preferred**:
```swift
var optionalSubview: UIView?
var volume: Double?

if let unwrappedSubview = optionalSubview {

    if let realVolume = volume {
  
        // do something with unwrappedSubview and realVolume
    }
}

// another example
UIView.animate(withDuration: 2.0) { [weak self] in
    
    guard let strongSelf = self else { return }
    strongSelf.alpha = 1.0
}
```
In conditions, the optional bool variable should be checked as follows:

**Preferred**:
```swift
if validatableObject?.validatableText?.isEmpty == true {

    //text is empty
} 
```

**Not Preferred**:
```swift
if validatableObject?.validatableText?.isEmpty ?? true {

    //text is empty
}
```


## Access control

Prefer `private` to `fileprivate`; use `fileprivate` only when the compiler insists.

Only explicitly use `open`, `public`, and `internal` when you require full access control specification.

Use access control as the leading property specifier. The only things that should come before access control are the `static` specifier or attributes such as `@IBAction`, `@IBOutlet` and `@discardableResult`.

Static `String` variables that are used for `NSNotification.Name` or
`UserDefaults` keys should not be `private` or `fileprivate`, as in the case of limited access control there is a possibility of duplication in another places


## Control flow

Prefer the `for-in` style of `for` loop over the `while-condition-increment` style.

The Ternary operator, `?:` , should only be used when it increases clarity or code neatness.

**Preferred**:

```swift
let value: Int = 5
result = value != 0 ? x : y

let isHorizontal: Bool = true
result = isHorizontal ? x : y
```

**Not Preferred**:

```swift
result = a > b ? x = c > d ? c : d : y
```


## Using `guard` statement

Use `guard` when really necessary. Avoid writing it in a nested code (loop, condition), this will worsen the readability and understanding of the written code.

Always start `guard` body on new line.

**Preferred**:
```swift
guard let value = value else {

	return
}
```

**Not Preferred**:
```swift
guard let value = value else { return }
```

Compound version when possible. In the compound version, place the `guard` on its own line, then indent each condition on its own line. The `else` clause is indented to match the conditions and the code is indented one additional level, as shown below. Example:

**Preferred**:
```swift
guard let value1 = value1, let value2 = value2, let value3 = value3 else {
    
    return
}

// do something
```

**Less Preferred**:
```swift
if let value1 = value1, let value2 = value2, let value3 = value3 {

    // do something
}
```

**Not Preferred**:
```swift
if let value1 = value1 {
    if let value2 = value2 {
        if let value3 = value3 {
            // do something
        } else {
            return
        }
    } else {
        return
    }
} else {
    return
}
```
Guard statements are required to exit in some way. Generally, this should be simple one line statement such as `return`, `throw`, `break`, `continue`, and `fatalError()`. Large code blocks should be avoided.


## Semicolons

Swift does not require a semicolon after each statement in your code. They are only required if you wish to combine multiple statements on a single line.

Do not write multiple statements on a single line separated with semicolons.

**Preferred**:
```swift
let swift: String = "not a scripting language"
```

**Not Preferred**:
```swift
let swift: String = "not a scripting language";
```


## Parentheses

Parentheses around conditionals are not required and should be omitted.

**Preferred**:
```swift
if name == "Hello" {

    print("World")
}
```

**Not Preferred**:
```swift
if (name == "Hello") {

    print("World")
}
```

Sometimes in larger expressions parentheses can make code read more clearly.


## Multi-line string literals

When building a long string literal, you're encouraged to use the multi-line string literal syntax. Open the literal on the same line as the assignment but do not include text on that line. Indent the text block one additional level.

**Preferred**:

```swift
let message = """
  You cannot charge the flux \
  capacitor with a 9V battery.
  You must use a super-charger \
  which costs 10 credits. You currently \
  have \(credits) credits available.
  """
```

**Not Preferred**:

```swift
let message = """You cannot charge the flux \
  capacitor with a 9V battery.
  You must use a super-charger \
  which costs 10 credits. You currently \
  have \(credits) credits available.
  """
```

**Not Preferred**:

```swift
let message = "You cannot charge the flux " +
  "capacitor with a 9V battery.\n" +
  "You must use a super-charger " +
  "which costs 10 credits. You currently " +
  "have \(credits) credits available."
```
