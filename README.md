# Objective-C Style Guide

## Table of Contents

* [Dot-Notation Syntax](#dot-notation-syntax)
* [Spacing](#spacing)
* [Conditionals](#conditionals)
  * [Ternary Operator](#ternary-operator)
* [Error handling](#error-handling)
* [Methods](#methods)
* [Variables](#variables)
* [Naming](#naming)
* [Comments](#comments)
* [Init & Dealloc](#init-and-dealloc)
* [Literals](#literals)
* [CGRect Functions](#cgrect-functions)
* [Constants](#constants)
* [Enumerated Types](#enumerated-types)
* [Bitmasks](#bitmasks)
* [Private Properties](#private-properties)
* [Pragma Marks](#pragma-marks)
* [Arithmetic and Comparison Operators](#arithmetic-and-comparison-operators)
* [Other Spacing](#other-spacing)
* [Property Declarations](#property-declarations)
* [Image Naming](#image-naming)
* [Booleans](#booleans)
* [Singletons](#singletons)
* [Xcode Project](#xcode-project)

## Dot-Notation Syntax

Dot-notation should **always** be used for accessing and mutating properties. Bracket notation is preferred in all other instances.

**For example:**
```objc
view.backgroundColor = [UIColor orangeColor];
[UIApplication sharedApplication].delegate;

count = myArray.count;
length = myName.length;
```

**Not:**
```objc
[view setBackgroundColor:[UIColor orangeColor]];
UIApplication.sharedApplication.delegate;

count = [myArray count];
length = [myName length];
```

## Spacing

* Indent using 4 spaces. Never indent with tabs. Be sure to set this preference in Xcode.
* Method braces and other braces (`if`/`else`/`switch`/`while` etc.) always open on the same line as the statement.

**For example:**
```objc
if (user.isHappy) {
//Do something
} else {
//Do something else
}
```
* There should be exactly one blank line between methods to aid in visual clarity and organization. Whitespace within methods should separate functionality, but often there should probably be new methods.
* `@synthesize` and `@dynamic` should be grouped logically, each group to be declared on new lines in the implementation.

## Conditionals

Conditional bodies should always use braces even when a conditional body could be written without braces (e.g., it is one line only) to prevent errors.

**For example:**
```objc
if (!error) {
    return success;
}
```

**Not:**
```objc
if (!error)
    return success;
```

or

```
if (!error) return success;
```

There should be a space either side of the outer most brackets.

**For example:**
```objc
if (!error) {
    return success;
}
```

**Not:**
```objc
if(!error){
    return success;
}
```

**or:**
```objc
if (!error){
    return success;
}
```

### Ternary Operator

The Ternary operator, ? , should never be used.

**For example:**
```objc
NSUInteger result;
if (a > b) {
    result = x;
} else {
    result = y;
}
```

**Not:**
```objc
NSUInteger result = a > b ? x : y;
```

## Error handling

When methods return an error parameter by reference, switch on the returned value, not the error variable.

**For example:**
```objc
NSError *error;
if (![self trySomethingWithError:&error]) {
    // Handle Error
}
```

**Not:**
```objc
NSError *error;
[self trySomethingWithError:&error];
if (error) {
    // Handle Error
}
```

Some of Apple’s APIs write garbage values to the error parameter (if non-NULL) in successful cases, so switching on the error can cause false negatives (and subsequently crash).

## Methods

*In method signatures, there should be a space after the scope (-/+ symbol). There should be a space between the method segments.
*There should be no space after the method's return type declaration.
*There should be no space after colons in the method signature.
*There should be no space after the parameter type in the method signature.
*The open brace should be on a new line.
*When calling methods there should be no space after semicolons.

**For example:**:
```objc
- (void)setExampleText:(NSString *)text image:(UIImage *)image;
```

**Not:**:
```objc
- (void)setExampleText:(NSString *)text image:(UIImage *)image;
```

## Variables

Variables should be named as descriptively as possible. Single letter variable names should be avoided. Even in `for()` loops, you can use something like `beanCounter` rather than just `i`.

Asterisks indicating pointers belong with the variable.

**For example:**
```objc
NSString *text;
```

**Not:**
```objc
NSString* text;
```

**or:**
```objc
NSString * text;

Property definitions should be used in place of naked instance variables whenever possible. Direct instance variable access should be avoided except in initializer methods (`init`, `initWithCoder:`, etc…), `dealloc` methods and within custom setters and getters. For more information on using Accessor Methods in Initializer Methods and dealloc, see [here](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/MemoryMgmt/Articles/mmPractical.html#//apple_ref/doc/uid/TP40004447-SW6).

**For example:**
```objc
@interface NYTSection: NSObject

@property (nonatomic) NSString *headline;

@end
```

**Not:**
```objc
@interface NYTSection : NSObject {
    NSString *headline;
}
```

#### Variable Qualifiers

When it comes to the variable qualifiers [introduced with ARC](https://developer.apple.com/library/ios/releasenotes/objectivec/rn-transitioningtoarc/Introduction/Introduction.html#//apple_ref/doc/uid/TP40011226-CH1-SW4), the qualifier (`__strong`, `__weak`, `__unsafe_unretained`, `__autoreleasing` `__block`) should be placed on the left of the class name.

**For example:**
```objc
__block NSString *text
```

## Naming

Apple naming conventions should be adhered to wherever possible, especially those related to [memory management rules](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/MemoryMgmt/Articles/MemoryMgmt.html) ([NARC](http://stackoverflow.com/a/2865194/340508)).

Long, descriptive method and variable names are good.

**For example:**
```objc
UIButton *settingsButton;
```

**Not:**
```objc
UIButton *setBut;
```

Constants should be camel-case with all words capitalized and prefixed by the related class name for clarity. Constants and variables should never be abbreviated.

**For example:**
```objc
static const NSTimeInterval ArticleViewControllerNavigationFadeAnimationDuration = 0.3;
```

**Not:**
```objc
static const NSTimeInterval AVCFadeTime = 1.7;
```

**or:**
```objc
static const NSTimeInterval fadetime = 1.7;
```

Properties and local variables should be camel-case with the leading word being lowercase.

Instance variables should be camel-case with the leading word being lowercase, and should be prefixed with an underscore. This is consistent with instance variables synthesized automatically by LLVM. **If LLVM can synthesize the variable automatically, then let it.**

**For example:**
```objc
@synthesize descriptiveVariableName = _descriptiveVariableName;
```

**Not:**
```objc
id varnm;
```

## Comments

When they are needed, comments should be used to explain **why** a particular piece of code does something. Any comments that are used must be kept up-to-date or deleted.

Block comments should generally be avoided, as code should be as self-documenting as possible, with only the need for intermittent, few-line explanations. This does not apply to those comments used to generate documentation.

## init and dealloc

`dealloc` methods should be placed at the top of the implementation, directly after the `@synthesize` and `@dynamic` statements. `init` should be placed directly below the `dealloc` methods of any class.

`init` methods should be structured like this:

```
- (instancetype)init
{
    self = [super init]; // or call the designated initalizer

    if (self) {
        // Custom initialization
    }

    return self;
}
```

## Literals

`NSString`, `NSDictionary`, `NSArray`, and `NSNumber` literals should be used whenever creating immutable instances of those objects. Pay special care that `nil` values not be passed into `NSArray` and `NSDictionary` literals, as this will cause a crash.

NSNumber literals should use brackets to encode numeric values, this is not necessary for boolean (`YES`/`NO) values.

There should always be space after each comma, either side of each semicolon and on the inside of the encasing brace or squared bracket.

**For example:**
```objc
NSArray *names = @[ @"Brian", @"Matt", @"Chris", @"Alex", @"Steve", @"Paul" ];
NSDictionary *productManagers = @{ @"iPhone" : @"Kate", @"iPad" : @"Kamal", @"Mobile Web" : @"Bill" };
NSNumber *shouldUseLiterals = @YES;
NSNumber *buildingZIPCode = @(10018.22);
```

**Not:**
```objc
NSArray *names = [NSArray arrayWithObjects:@"Brian", @"Matt", @"Chris", @"Alex", @"Steve", @"Paul", nil];
NSDictionary *productManagers = [NSDictionary dictionaryWithObjectsAndKeys: @"Kate", @"iPhone", @"Kamal", @"iPad", @"Bill", @"Mobile Web", nil];
NSNumber *shouldUseLiterals = [NSNumber numberWithBool:YES];
NSNumber *buildingZIPCode = [NSNumber numberWithInteger:10018];
```

## CGRect Functions

When accessing the `x`, `y`, `width`, or `height` of a `CGRect`, always use the [`CGGeometry` functions](http://developer.apple.com/library/ios/#documentation/graphicsimaging/reference/CGGeometry/Reference/reference.html) instead of direct struct member access. From Apple's `CGGeometry` reference:

> All functions described in this reference that take CGRect data structures as inputs implicitly standardize those rectangles before calculating their results. For this reason, your applications should avoid directly reading and writing the data stored in the CGRect data structure. Instead, use the functions described here to manipulate rectangles and to retrieve their characteristics.

**For example:**
```objc
CGRect frame = self.view.frame;

CGFloat x = CGRectGetMinX(frame);
CGFloat y = CGRectGetMinY(frame);
CGFloat width = CGRectGetWidth(frame);
CGFloat height = CGRectGetHeight(frame);
```

**Not:**
```objc
CGRect frame = self.view.frame;

CGFloat x = frame.origin.x;
CGFloat y = frame.origin.y;
CGFloat width = frame.size.width;
CGFloat height = frame.size.height;
```

## Constants

Constants are preferred over in-line string literals or numbers, as they allow for easy reproduction of commonly used variables and can be quickly changed without the need for find and replace. Constants should be declared as `static` constants and not `#define`s unless explicitly being used as a macro.

**For example:**
```objc
static NSString * const AboutViewControllerCompanyName = @"The New York Times Company";

static const CGFloat ImageThumbnailHeight = 50.0;
```

**Not:**
```objc
#define CompanyName @"The New York Times Company"

#define thumbnailHeight 2
```

## Enumerated Types

When using `enum`s, it is recommended to use the new fixed underlying type specification because it has stronger type checking and code completion. The SDK now includes a macro to facilitate and encourage use of fixed underlying types — `NS_ENUM()`

**Example:**
```objc
typedef NS_ENUM(NSInteger, AdRequestState) {
    AdRequestStateInactive,
    RequestStateLoading
};
```

## Bitmasks

When working with bitmasks, use the `NS_OPTIONS` macro.

**Example:**
```objc
typedef NS_OPTIONS(NSUInteger, NYTAdCategory) {
  NYTAdCategoryAutos      = 1 << 0,
  NYTAdCategoryJobs       = 1 << 1,
  NYTAdCategoryRealState  = 1 << 2,
  NYTAdCategoryTechnology = 1 << 3
};
```

## Private Properties

Private properties should be declared in class extensions (anonymous categories) in the implementation file of a class. Named categories (such as `private`) should never be used unless extending another class.

**For example:**
```objc
@interface Advertisement ()

@property (nonatomic, strong) GADBannerView *googleAdView;
@property (nonatomic, strong) ADBannerView *iAdView;
@property (nonatomic, strong) UIWebView *adXWebView;

@end
```

## Pragma Marks
There should be two newlines above a `#pragma mark` and one newline below. There should be no space after the `hash`. They should be used to categorise similar methods and use a title to describe what the category of methods does. The dash with a space either side is used to display a separator in Xcode's method selector.

**For example:**
```objc
#pragma mark - View Lifecycle Methods
```

## Arithmetic and Comparison Operators
Each arithmetic and comparison operator should have a space either side of it, brackets should be used for clarity even when unnecessary.

**For example:**
```objc
NSUInteger random = smallest + arc4random() % (largest + 1 - smallest);
```

**Not:**
```objc
NSUInteger random = smallest +arc4random() %(largest +1-smallest);
```

## Other Spacing
* There should be a space before and after the equals operator when assigning values to properties.
* There should only be one newline before @end.
* There should be a newline before the return when the method isn't a single line.
* There should be one newline between methods.
* There should be a space before the protocol declaration list. ``AppDelegate : UIResponder <UIApplicationDelegate>``

* There should be no space before the colon in the case declaration of a switch statement.
* There should always be a space before the asterisk in method signature and variable declarations.

## @property
* The order should be strong, nonatomic.
* There should be a space on either side of the brackets.
* There should be a space after each comma.

## Image Naming

Image names should be named consistently to preserve organization and developer sanity. They should be named as one camel case string with a description of their purpose, followed by the un-prefixed name of the class or property they are customizing (if there is one), followed by a further description of color and/or placement, and finally their state.

**For example:**
* `RefreshBarButtonItem` / `RefreshBarButtonItem@2x` and `RefreshBarButtonItemSelected` / `RefreshBarButtonItemSelected@2x`
* `ArticleNavigationBarWhite` / `ArticleNavigationBarWhite@2x` and `ArticleNavigationBarBlackSelected` / `ArticleNavigationBarBlackSelected@2x`.

Images that are used for a similar purpose should be grouped in respective groups in an Images folder.

## Booleans

Since `nil` resolves to `NO` it is unnecessary to compare it in conditions. Never compare something directly to `YES`, because `YES` is defined to 1 and a `BOOL` can be up to 8 bits.

This allows for more consistency across files and greater visual clarity.

**For example:**
```objc
if (!someObject) {
}
```

**Not:**
```objc
if (someObject == nil) {
}
```

-----

**For a `BOOL`, here are two examples:**
```objc
if (isAwesome)
if (![someObject boolValue])
```

**Not:**
```objc
if ([someObject boolValue] == NO)
if (isAwesome == YES) // Never do this.
```

-----

If the name of a `BOOL` property is expressed as an adjective, the property can omit the “is” prefix but specifies the conventional name for the get accessor.

**For example:**
```objc
@property (assign, getter=isEditable) BOOL editable;
```
Text and example taken from the [Cocoa Naming Guidelines](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/Articles/NamingIvarsAndTypes.html#//apple_ref/doc/uid/20001284-BAJGIIJE).

## Singletons

Singleton objects should use a thread-safe pattern for creating their shared instance.

**For example:**
```objc
+ (instancetype)sharedInstance
{
   static id sharedInstance = nil;

   static dispatch_once_t onceToken;
   dispatch_once(&onceToken, ^{
      sharedInstance = [[self alloc] init];
   });

   return sharedInstance;
}
```

This will prevent [possible and sometimes prolific crashes](http://cocoasamurai.blogspot.com/2011/04/singletons-your-doing-them-wrong.html).

## Xcode project

The physical files should be kept in sync with the Xcode project files in order to avoid file sprawl. Any Xcode groups created should be reflected by folders in the filesystem. Code should be grouped not only by type, but also by feature for greater clarity.

When possible, always turn on "Treat Warnings as Errors" in the target's Build Settings and enable as many [additional warnings](http://boredzo.org/blog/archives/2009-11-07/warnings) as possible. If you need to ignore a specific warning, use [Clang's pragma feature](http://clang.llvm.org/docs/UsersManual.html#controlling-diagnostics-via-pragmas).
