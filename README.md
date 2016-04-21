## Background

Here are some of the documents from Apple that informed the style guide. If something isn't mentioned here, it's probably covered in great detail in one of these:

* [The Objective-C Programming Language](http://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/ObjectiveC/Introduction/introObjectiveC.html)
* [Cocoa Fundamentals Guide](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CocoaFundamentals/Introduction/Introduction.html)
* [Coding Guidelines for Cocoa](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/CodingGuidelines.html)
* [iOS App Programming Guide](http://developer.apple.com/library/ios/#documentation/iphone/conceptual/iphoneosprogrammingguide/Introduction/Introduction.html)

## Table of Contents

* [Code Organization](#code-organization)
* [Spacing](#spacing)
* [Comments](#comments)
* [Naming](#naming)
  * [Underscores](#underscores)
  * [ViewControllers](#viewcontrollers)
* [Methods](#methods)
* [Variables](#variables)
* [Property Attributes](#property-attributes)
* [Dot-Notation Syntax](#dot-notation-syntax)
* [Literals](#literals)
* [Constants](#constants)
* [Enumerated Types](#enumerated-types)
* [Case Statements](#case-statements)
* [Private Properties](#private-properties)
* [IBActions](#ibactions)
* [IBOutlets](#iboutlets)
* [Generics](#generics)
* [Nullable identificators](#nullable-identificators)
* [Protocols](#protocols)
* [Booleans](#booleans)
* [Conditionals](#conditionals)
  * [Ternary Operator](#ternary-operator)
* [Init Methods](#init-methods)
* [Class Constructor Methods](#class-constructor-methods)
* [CGRect Functions](#cgrect-functions)
* [Golden Path](#golden-path)
* [Error handling](#error-handling)
* [Singletons](#singletons)
* [Line Breaks](#line-breaks)
* [Xcode Project](#xcode-project)


## Code Organization

Use `#pragma mark -` to categorize methods in functional groupings and protocol/delegate implementations following this general structure. Pragma marks names should be terms that are widely known; for example: Lifecycle, IBActions, Custom setters, Custom getters etc. If we delimit want to delimit a protocol implementation, it should be named exactly as the protocol; for example: UITableViewDataSource, NSCopying etc. If methods are overriden, they should be marked with the class name that contains the overriden method; for example: NSObject for "description" method.

Lifecycle methods should be in the calling order. Private methods can also be separated by functionality pragma marks (eg. `#pragma mark - Requests`).

```objc
#pragma mark - Lifecycle

+ (void)initialize;
- (instancetype)init {}
- (void)viewDidLoad {}
- (void)viewWillAppear:(BOOL)animated {}
- (void)didReceiveMemoryWarning {}
- (void)dealloc {}

#pragma mark - Custom Setters

- (void)setCustomProperty:(id)value {}

#pragma mark - Custom Getters

- (id)customProperty {}

#pragma mark - Public

- (void)publicMethod {}

#pragma mark - Private

- (void)privateMethod {}

#pragma mark Requests

- (void)requestMethod {}

#pragma mark - UITextFieldDelegate
#pragma mark - UITableViewDataSource
#pragma mark - UITableViewDelegate

#pragma mark - NSCopying

- (id)copyWithZone:(NSZone *)zone {}

#pragma mark - NSObject

- (NSString *)description {}

#pragma mark - IBActions

- (IBAction)submitData:(id)sender {}

```

## Spacing

* Indent using XCode default (4 spaces).
* Method braces and other braces (`if`/`else`/`switch`/`while` etc.) always open on the same line as the statement but close on a new line.
* Set page guide at column 120 (see below how to deal with exceeding this limit.

**Preferred:**
```objc
if (user.isHappy) {
  //Do something
} else {
  //Do something else
}
```

**Not Preferred:**
```objc
if (user.isHappy)
{
    //Do something
}
else {
    //Do something else
}
```

* There should be exactly one blank line between methods to aid in visual clarity and organization. Whitespace within methods should separate functionality, but often there should probably be new methods.
* Prefer using auto-synthesis. But if necessary, `@synthesize` and `@dynamic` should each be declared on new lines in the implementation.
* Colon-aligning method invocation should be used if a method signature has >= 3 arguments or the length exceeds the 120 limit. Please do **NOT** however colon align methods containing blocks because Xcode's indenting makes it illegible.

**Preferred:**

```objc
[self foo:bar bar:foo];

// has >= three arguments
[self foo:bar
      bar:foo
      ham:spam
      hem:spam];
```

```objc
// blocks are easily readable
[UIView animateWithDuration:1.0 animations:^{
  // something
} completion:^(BOOL finished) {
  // something
}];
```

**Not Preferred:**

```objc
// colon-aligning makes the block indentation hard to read
[UIView animateWithDuration:1.0
                 animations:^{
                     // something
                 }
                 completion:^(BOOL finished) {
                     // something
                 }];
```

## Comments

When they are needed, comments should be used to explain **why** a particular piece of code does something. Any comments that are used must be kept up-to-date or deleted.

Block comments should generally be avoided, as code should be as self-documenting as possible, with only the need for intermittent, few-line explanations. *Exception: This does not apply to those comments used to generate documentation.*

Rule of thumb: if you feel the need to write a comment, you probably need to create another method or name the variables better.

## Naming

Apple naming conventions should be adhered to wherever possible, especially those related to [memory management rules](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/MemoryMgmt/Articles/MemoryMgmt.html) ([NARC](http://stackoverflow.com/a/2865194/340508)).

Long, descriptive method and variable names are good. Shortcuts are not ecouraged.

**Preferred:**

```objc
UIButton *settingsButton;
```

**Not Preferred:**

```objc
UIButton *setBut;
```

Constants should be camel-case with all words capitalized.

**Preferred:**

```objc
static NSTimeInterval const RWTTutorialViewControllerNavigationFadeAnimationDuration = 0.3;
```

**Not Preferred:**

```objc
static NSTimeInterval const fadetime = 1.7;
```

Properties should be camel-case with the leading word being lowercase. Use auto-synthesis for properties rather than manual @synthesize statements unless you have good reason.

**Preferred:**

```objc
@property (strong, nonatomic, nonnull) NSString *descriptiveVariableName;
```

**Not Preferred:**

```objc
id varnm;
```

### Underscores

When using properties, instance variables should always be accessed and mutated using `self.`. This means that all properties will be visually distinct, as they will all be prefaced with `self.`.

An exception to this: inside initializers, the backing instance variable should be used directly to avoid any potential side effects of the getters/setters.

Local variables should not contain underscores.

### ViewControllers

ViewControllers will have the `VC` suffix and other counterparts of it will have a similar one.

**Preferred:**

```objc
LoginVC
OffersTVC // for UITableViewController
ProductsCVC // for UICollectionViewController
```

**Not Preffered:**
```objc
LoginViewC
OffersTableVC
ProductsCollectionViewController
```

## Methods

In method signatures, there should be a space after the method type (-/+ symbol). There should be a space between the method segments (matching Apple's style). Always include a keyword and be descriptive with the word before the argument which describes the argument.

When creating a new file, methods created by the file template that are not used should be deleted. No empty methods.

The usage of the word "and" is reserved.  It should not be used for multiple parameters as illustrated in the `initWithWidth:height:` example below.

**Preferred:**
```objc
- (void)setExampleText:(NSString *)text image:(UIImage *)image;
- (void)sendAction:(SEL)aSelector to:(id)anObject forAllCells:(BOOL)flag;
- (id)viewWithTag:(NSInteger)tag;
- (instancetype)initWithWidth:(CGFloat)width height:(CGFloat)height;
```

**Not Preferred:**

```objc
-(void)setT:(NSString *)text i:(UIImage *)image;
- (void)sendAction:(SEL)aSelector :(id)anObject :(BOOL)flag;
- (id)taggedView:(NSInteger)tag;
- (instancetype)initWithWidth:(CGFloat)width andHeight:(CGFloat)height;
- (instancetype)initWith:(int)width and:(int)height;  // Never do this.
```

## Variables

Variables should be named as descriptively as possible. Single letter variable names should be avoided except in `for()` loops.

Asterisks indicating pointers belong with the variable, e.g., `NSString *text` not `NSString* text` or `NSString * text`, except in the case of constants.

[Private properties](#private-properties) should be used in place of instance variables whenever possible. Although using instance variables is a valid way of doing things, by agreeing to prefer properties our code will be more consistent. Also, starting from Objective-C 3.0, you can put nullable identificators for properties, thus making the code more readable an easier to understand.

Apple (https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/ProgrammingWithObjectiveC/EncapsulatingData/EncapsulatingData.html) :
"You Can Define Instance Variables without Properties

It’s best practice to use a property on an object any time you need to keep track of a value or another object.

If you do need to define your own instance variables without declaring a property, you can add them inside braces at the top of the class interface or implementation."

Direct access to instance variables that 'back' properties should be avoided except in initializer methods (`init`, `initWithCoder:`, etc…), `dealloc` methods and within custom setters and getters. For more information on using Accessor Methods in Initializer Methods and dealloc, see [here](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/MemoryMgmt/Articles/mmPractical.html#//apple_ref/doc/uid/TP40004447-SW6).

**Preferred:**

```objc
@interface RWTTutorial : NSObject

@property (strong, nonatomic, nullable) NSString *tutorialName;

@end
```

**Not Preferred:**

```objc
@interface RWTTutorial : NSObject {
  NSString *tutorialName;
}
```


## Property Attributes

Property attributes should be explicitly listed, and will help new programmers when reading the code.  The order of properties should be storage then atomicity, which is consistent with automatically generated code when connecting UI elements from Interface Builder. The nullable identificator should be the last one listed.

**Preferred:**

```objc
@property (strong, nonatomic, nonnull) NSString *tutorialName;
```

**Not Preferred:**

```objc
@property (nonatomic) NSString *tutorialName;
```

Properties with mutable counterparts (e.g. NSString) should prefer `copy` instead of `strong`.
Why? Even if you declared a property as `NSString` somebody might pass in an instance of an `NSMutableString` and then change it without you noticing that.  

**Preferred:**

```objc
@property (copy, nonatomic) NSString *tutorialName;
```

**Not Preferred:**

```objc
@property (strong, nonatomic) NSString *tutorialName;
```

## Dot-Notation Syntax

Dot syntax is purely a convenient wrapper around accessor method calls. When you use dot syntax, the property is still accessed or changed using getter and setter methods.  Read more [here](https://developer.apple.com/library/ios/documentation/cocoa/conceptual/ProgrammingWithObjectiveC/EncapsulatingData/EncapsulatingData.html)

Dot-notation should **always** be used for accessing and mutating properties, as it makes code more concise. Bracket notation is preferred in all other instances.

**Preferred:**
```objc
NSInteger arrayCount = self.array.count;
view.backgroundColor = [UIColor orangeColor];
[UIApplication sharedApplication].delegate;
```

**Not Preferred:**
```objc
[view setBackgroundColor:[UIColor orangeColor]];
UIApplication.sharedApplication.delegate;
```

## Literals

`NSString`, `NSDictionary`, `NSArray`, and `NSNumber` literals should be used whenever creating immutable instances of those objects. Pay special care that `nil` values can not be passed into `NSArray` and `NSDictionary` literals, as this will cause a crash.

**Preferred:**

```objc
NSArray *names = @[@"Brian", @"Matt", @"Chris", @"Alex", @"Steve", @"Paul"];
NSDictionary *productManagers = @{@"iPhone": @"Kate", @"iPad": @"Kamal", @"Mobile Web": @"Bill"};
NSNumber *shouldUseLiterals = @YES;
NSNumber *buildingStreetNumber = @10018;
```

**Not Preferred:**

```objc
NSArray *names = [NSArray arrayWithObjects:@"Brian", @"Matt", @"Chris", @"Alex", @"Steve", @"Paul", nil];
NSDictionary *productManagers = [NSDictionary dictionaryWithObjectsAndKeys: @"Kate", @"iPhone", @"Kamal", @"iPad", @"Bill", @"Mobile Web", nil];
NSNumber *shouldUseLiterals = [NSNumber numberWithBool:YES];
NSNumber *buildingStreetNumber = [NSNumber numberWithInteger:10018];
```

## Constants

Constants are preferred over in-line string literals or numbers, as they allow for easy reproduction of commonly used variables and can be quickly changed without the need for find and replace. Constants should be declared as `static` constants and not `#define`s unless explicitly being used as a macro.
For objects that can't be defined as constants because the `initializer element is not a compile time constant`, we should define the object only as static and initialize it in the `+ (void)initialize` method using a `dispatch_once` block.

**Preferred:**

```objc
static NSString * const RWTAboutViewControllerCompanyName = @"RayWenderlich.com";

static CGFloat const RWTImageThumbnailHeight = 50.0;
```

```objc
static NSDictionary<NSNumber *, UIColor *> *kColorForStatesDictionary;

+ (void)initialize
{
    static dispatch_once_t onceToken;
    dispatch_once(&onceToken, ^{
        kColorForStatesDictionary = @{
                                      @(RCAStepStateCurrent) : kColorCurrent,
                                      @(RCAStepStateActive) : kColorActive,
                                      @(RCAStepStateInactive) : kColorInactive
                                      };
    });
}
```

**Not Preferred:**

```objc
#define CompanyName @"RayWenderlich.com"

#define thumbnailHeight 2
```

## Enumerated Types

When using `enum`s, it is recommended to use the new fixed underlying type specification because it has stronger type checking and code completion. The SDK now includes a macro to facilitate and encourage use of fixed underlying types: `NS_ENUM()`

**For Example:**

```objc
typedef NS_ENUM(NSInteger, RWTLeftMenuTopItemType) {
  RWTLeftMenuTopItemMain,
  RWTLeftMenuTopItemShows,
  RWTLeftMenuTopItemSchedule
};
```

You can also make explicit value assignments (showing older k-style constant definition):

```objc
typedef NS_ENUM(NSInteger, RWTGlobalConstants) {
  RWTPinSizeMin = 1,
  RWTPinSizeMax = 5,
  RWTPinCountMin = 100,
  RWTPinCountMax = 500,
};
```

Older k-style constant definitions should be **avoided** unless writing CoreFoundation C code (unlikely).

**Not Preferred:**

```objc
enum GlobalConstants {
  kMaxPinSize = 5,
  kMaxPinCount = 500,
};
```


## Case Statements

Braces are not required for case statements, unless enforced by the complier.  
When a case contains more than one line, braces should be added.

```objc
switch (condition) {
  case 1:
    // ...
    break;
  case 2: {
    // ...
    // Multi-line example using braces
    break;
  }
  case 3:
    // ...
    break;
  default:
    // ...
    break;
}

```

There are times when the same code can be used for multiple cases, and a fall-through should be used.  A fall-through is the removal of the 'break' statement for a case thus allowing the flow of execution to pass to the next case value.  A fall-through should be commented for coding clarity.

```objc
switch (condition) {
  case 1:
    // ** fall-through! **
  case 2:
    // code executed for values 1 and 2
    break;
  default:
    // ...
    break;
}

```

When using an enumerated type for a switch, 'default' is not needed.   For example:

```objc
RWTLeftMenuTopItemType menuType = RWTLeftMenuTopItemMain;

switch (menuType) {
  case RWTLeftMenuTopItemMain:
    // ...
    break;
  case RWTLeftMenuTopItemShows:
    // ...
    break;
  case RWTLeftMenuTopItemSchedule:
    // ...
    break;
}
```


## Private Properties

Private properties should be declared in class extensions (anonymous categories) in the implementation file of a class. Named categories (such as `RWTPrivate` or `private`) should never be used unless extending another class.   The Anonymous category can be shared/exposed for testing using the <headerfile>+Private.h file naming convention.

**For Example:**

```objc
@interface RWTDetailViewController ()

@property (strong, nonatomic) GADBannerView *googleAdView;
@property (strong, nonatomic) ADBannerView *iAdView;
@property (strong, nonatomic) UIWebView *adXWebView;

@end
```

## IBOutlets

IBOutlets will always be in the .m file as strong properties. Should not be mixed, IBOutlets first.

## IBActions

IBActions will always be only in the .m file. It will not be called programatically. If the same functionality is needed, it will pe extracted into a method that will be called from the IBAction.

IBActions for `UIButton`s `Touch Up Inside` event will be named `*buttonName*ButtonPressed`

**For Example:**

```objc
- (IBActions)purchaseButtonPressed:(UIButton *)sender {}
```

## Generics

Generics should always be used for all the classes that support them (eg. NSArray, NSDictionary, NSSet etc.), both when defined as properties or as public method arguments and for private methods only if needed. As for identation, there should be a space between the class type and the pointer. There shouldn't be any spaces between the class and the generics identificator.

**Preferred:**

```objc
@property (strong, nonatomic, nonnull) NSArray<NSString *> *stringsArray;

- (void)foo:(nonnull NSArray<NSString *> *)stringsArray;

```
**Not Preferred:**

```objc
@property (strong, nonatomic, nonnull) NSArray *stringsArray;
@property (strong, nonatomic, nonnull) NSArray<NSString*> *stringsArray;
@property (strong, nonatomic, nonnull) NSArray <NSString *> *stringsArray;

```

## Nullable identificators

Nullable identificators (`nonnull`, `nullable`) should be used in the same places as Generics. For methods, it should be written before the argument type. For properties, it should be written as the last identificator.

Delegates, which are `weak`, will always be `nullable`.

**Preferred:**

```objc
- (void)foo:(nonnull NSString *)bar;
@property (nonatomic, copy, nonnull) NSString *nonNullString;
```

**Not Preferred:**

```objc
- (void)foo:(NSString * nonnull)bar;
- (void)foo:(NSString nonnull *)bar;
@property (nonatomic, nonnull, copy) NSString *nonnullString;
```

## Protocols

Protocols can be defined either in a separate file or in an existing class header file.
If defined in an existing header class, the protocol definition will be after the class interface only if the class is used as an argument in one of the methods.
Methods in the protocol will be prefixed with the class name, including the class as a parameter or not.

**Preferred:**

```objc
@protocol fooProtocol;

@interface foo {
 IBOutlet UILabel *label;
}

- (void)setupWithBar:(nullable id)bar;

@end;

@protocol fooProtocol<NSObject>

- (void)fooDidDoThis:(nullable id)this;

@end
```

## Booleans

Objective-C uses `YES` and `NO`.  Therefore `true` and `false` should only be used for CoreFoundation, C or C++ code.  Since `nil` resolves to `NO` it is unnecessary to compare it in conditions. Never compare something directly to `YES`, because `YES` is defined to 1 and a `BOOL` can be up to 8 bits.

This allows for more consistency across files and greater visual clarity. *folosirea enforce-urilor*

**Preferred:**

```objc
if (someObject) {}
if (![anotherObject boolValue]) {}
```

**Not Preferred:**

```objc
if (someObject == nil) {}
if ([anotherObject boolValue] == NO) {}
if (isAwesome == YES) {} // Never do this.
if (isAwesome == true) {} // Never do this.
```

If the name of a `BOOL` property is expressed as an adjective, the property can omit the “is” prefix but specifies the conventional name for the get accessor, for example:

```objc
@property (assign, getter=isEditable) BOOL editable;
```
Text and example taken from the [Cocoa Naming Guidelines](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/Articles/NamingIvarsAndTypes.html#//apple_ref/doc/uid/20001284-BAJGIIJE).

## Conditionals

Conditional bodies should always use braces even when a conditional body could be written without braces (e.g., it is one line only) to prevent errors. These errors include adding a second line and expecting it to be part of the if-statement. Another, [even more dangerous defect](http://programmers.stackexchange.com/a/16530) may happen where the line "inside" the if-statement is commented out, and the next line unwittingly becomes part of the if-statement. In addition, this style is more consistent with all other conditionals, and therefore more easily scannable. If there is a conditional used for returning from a method, it can be written on the same line using braces and if they contain only a return statement.
If you can, all conditionals for returning from a method should be written at the beginning.

**Preferred:**
```objc
if (!error) {
  self.foo = bar;
  return success;
}

if (!error) { return NO; }
```

**Not Preferred:**
```objc
if (!error)
  return success;
```

or

```objc
if (!error) return success;
```

### Ternary Operator

The Ternary operator, `?:` , should only be used when it increases clarity or code neatness. A single condition is usually all that should be evaluated. Evaluating multiple conditions is usually more understandable as an `if` statement, or refactored into instance variables. In general, the best use of the ternary operator is during assignment of a variable and deciding which value to use.

Non-boolean variables should be compared against something, and parentheses are added for improved readability.  If the variable being compared is a boolean type, then no parentheses are needed.

Also, this operator can be used as an "Elvis" operator.
"result = expression ?: value_if_expression_is_null"
In this case, "result" will have "expression"'s value, if it's not nil. Otherwise, it will get the default value.

**Preferred:**
```objc
NSInteger value = 5;
result = (value != 0) ? x : y;

BOOL isHorizontal = YES;
result = isHorizontal ? x : y;
```

**Not Preferred:**
```objc
result = a > b ? x = c > d ? c : d : y;
```

## Init Methods

Init methods should follow the convention provided by Apple's generated code template.  A return type of 'instancetype' should also be used instead of 'id'.

```objc
- (instancetype)init {
  self = [super init];
  if (self) {
    // ...
  }
  return self;
}
```

See [Class Constructor Methods](#class-constructor-methods) for link to article on instancetype.

## Class Constructor Methods

Where class constructor methods are used, these should always return type of 'instancetype' and never 'id'. This ensures the compiler correctly infers the result type.

```objc
@interface Airplane
+ (nonnull instancetype)airplaneWithType:(RWTAirplaneType)type;
@end
```

More information on instancetype can be found on [NSHipster.com](http://nshipster.com/instancetype/).

## CGRect Functions

When accessing the `x`, `y`, `width`, or `height` of a `CGRect`, always use the [`CGGeometry` functions](http://developer.apple.com/library/ios/#documentation/graphicsimaging/reference/CGGeometry/Reference/reference.html) instead of direct struct member access. From Apple's `CGGeometry` reference:

> All functions described in this reference that take CGRect data structures as inputs implicitly standardize those rectangles before calculating their results. For this reason, your applications should avoid directly reading and writing the data stored in the CGRect data structure. Instead, use the functions described here to manipulate rectangles and to retrieve their characteristics.

**Preferred:**

```objc
CGRect frame = self.view.frame;

CGFloat x = CGRectGetMinX(frame);
CGFloat y = CGRectGetMinY(frame);
CGFloat width = CGRectGetWidth(frame);
CGFloat height = CGRectGetHeight(frame);
CGRect frame = CGRectMake(0.0, 0.0, width, height);
```

**Not Preferred:**

```objc
CGRect frame = self.view.frame;

CGFloat x = frame.origin.x;
CGFloat y = frame.origin.y;
CGFloat width = frame.size.width;
CGFloat height = frame.size.height;
CGRect frame = (CGRect){ .origin = CGPointZero, .size = frame.size };
```

## Golden Path

When coding with conditionals, the left hand margin of the code should be the "golden" or "happy" path.  That is, don't nest `if` statements.  Multiple return statements are OK.

**Preferred:**

```objc
- (void)someMethod {
  if (![someOther boolValue]) { 
   return; 
  }

  //Do something important
}
```

**Not Preferred:**

```objc
- (void)someMethod {
  if ([someOther boolValue]) {
    //Do something important
  }
}
```

## Error handling

When methods return an error parameter by reference, switch on the returned value, not the error variable.

**Preferred:**
```objc
NSError *error;
if (![self trySomethingWithError:&error]) {
  // Handle Error
}
```

**Not Preferred:**
```objc
NSError *error;
[self trySomethingWithError:&error];
if (error) {
  // Handle Error
}
```

Some of Apple’s APIs write garbage values to the error parameter (if non-NULL) in successful cases, so switching on the error can cause false negatives (and subsequently crash).


## Singletons

Singleton objects should use a thread-safe pattern for creating their shared instance.
```objc
+ (instancetype)sharedInstance {
  static id sharedInstance = nil;

  static dispatch_once_t onceToken;
  dispatch_once(&onceToken, ^{
    sharedInstance = [[self alloc] init];
  });

  return sharedInstance;
}
```
This will prevent [possible and sometimes prolific crashes](http://cocoasamurai.blogspot.com/2011/04/singletons-your-doing-them-wrong.html).


## Line Breaks

Line breaks are an important topic since this style guide is focused for print and online readability.

For example:
```objc
self.productsRequest = [[SKProductsRequest alloc] initWithProductIdentifiers:productIdentifiers];
```
A long line of code like this should be carried on to the second line adhering to this style guide's Spacing section (two spaces).
```objc
self.productsRequest = [[SKProductsRequest alloc]
  initWithProductIdentifiers:productIdentifiers];
```


## Xcode project

The physical files should be kept in sync with the Xcode project files in order to avoid file sprawl. Any Xcode groups created should be reflected by folders in the filesystem. Code should be grouped not only by type, but also by feature for greater clarity.

When possible, always turn on "Treat Warnings as Errors" in the target's Build Settings and enable as many [additional warnings](http://boredzo.org/blog/archives/2009-11-07/warnings) as possible. If you need to ignore a specific warning, use [Clang's pragma feature](http://clang.llvm.org/docs/UsersManual.html#controlling-diagnostics-via-pragmas).
