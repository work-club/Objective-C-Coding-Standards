Objective-C-Standards
====================

The Development teams Objective-C standards document.

## Credits

We would like to thank the creators of the [Ray Wenderlich](https://github.com/raywenderlich/objective-c-style-guide), [New York Times](https://github.com/NYTimes/objective-c-style-guide) and [Robots & Pencils'](https://github.com/RobotsAndPencils/objective-c-style-guide) Objective-C Style Guides.  These three style guides provided a solid starting point for this guide to be created and based upon.

## Table of Contents

* [Overview](#overview)
    * [What this guide is for](#what-this-guide-is-for)
    * [What this isn't for](#what-this-isnt-for)
    * [Assumptions](#assumptions)
* [Conventions](#conventions)
    * [Whitespace](#whitespace)
    * [Comments](#comments)
    * [Organization](#organization)
    * [Naming](#naming)
    * [Literals](#literals)
    * [Dot-Notation Syntax](#dot-notation-syntax)
    * [Booleans](#booleans)
    * [Methods](#methods)
    * [Variables](#variables)
    * [Conditionals](#conditionals)
    * [CGRect Functions](cgrect-functions)
    * [Golden Path](#golden-path)
* [Xcode project](#xcode-project)
* [Testing](#testing)
* [Resources](#resources) 

## Overview

This document has been put together by the front-end development team at Work Club, a London based digital agency. This document is a reference for all developers in the team to follow. This document is subject to change and improvements.

### What this guide is for

This document is to be followed by all developers writing Objective-C on a project, regardless of size or complexity. It's intended to be followed to provide consistency across all projects and developers. It will point out good patterns and anti-patterns and explain why these should be used or ignored.

### What this isn't for

This guide will not dictate what libraries or frameworks you should use in a project as these will vary on the project and size. Nor will this guide teach your Objective-C.

### Assumptions

This guide will assume you are proffient in Objective-C.

## Convention
The following is a list of conventions for formatting, structure and overal code approach.
    
### Whitespace

* Always indent using 4 spaces. Never indent with tabs. Xcode defaults to 4 spaces, but be sure this prefernce is set in Xcode.
* Method braces and other braces (`if`/`else`/`switch`/`while` etc.) should always open on the same line as the statement but close on a new line.

**Example:**:
```objc
if (user.isHappy) {
//Do something
}
else {
//Do something else
}
```

* There should be exactly one blank line between methods to aid in visual clarity and organization.

### Comments

When they are needed, comments should be used to explain **why** a particular piece of code does something. Any comments that are used must be kept up-to-date or deleted.

Block comments should generally be avoided, as code should be as self-documenting as possible, with only the need for intermittent, few-line explanations. This does not apply to those comments used to generate documentation.

### Organization

Use `#pragma mark -`s to categorize methods in functional groupings and protocol/delegate implementations.

### Naming

Long, descriptive method and variable names are good, Be clear, expressive and non-ambigous.

**Good:**

```objc
UIButton *settingsButton;
```

**Bad**

```objc
UIButton *setBut;
```

A three letter prefix (e.g. `NYT`) should always be used for class names and constants, however may be omitted for Core Data entity names. Constants should be camel-case with all words capitalized and prefixed by the related class name for clarity.

**Good:**

```objc
static const NSTimeInterval NYTArticleViewControllerNavigationFadeAnimationDuration = 0.3;
```

**Bad:**

```objc
static const NSTimeInterval fadetime = 1.7;
```

Properties and local variables should be camel-case with the leading word being lowercase. 

Instance variables should be camel-case with the leading word being lowercase, and should be prefixed with an underscore. This is consistent with instance variables synthesized automatically by LLVM. **If LLVM can synthesize the variable automatically, then let it.**

**Good:**

```objc
@synthesize descriptiveVariableName = _descriptiveVariableName;
```

**Bad:**

```objc
id varnm;
```

### Literals

`NSString`, `NSDictionary`, `NSArray`, and `NSNumber` literals should be used whenever creating immutable instances of those objects. Pay special care that `nil` values can not be passed into `NSArray` and `NSDictionary` literals, as this will cause a crash.

**Good:**

```objc
NSArray *names = @[@"Lloyd", @"Gary", @"Jordi", @"Peter", @"Chaz", @"Andrew"];
NSDictionary *productManagers = @{@"iPhone": @"Kate", @"iPad": @"Kamal", @"Mobile Web": @"Bill"};
NSNumber *shouldUseLiterals = @YES;
NSNumber *buildingStreetNumber = @10018;
```

**Bad:**

```objc
NSArray *names = [NSArray arrayWithObjects:@"Brian", @"Matt", @"Chris", @"Alex", @"Steve", @"Paul", nil];
NSDictionary *productManagers = [NSDictionary dictionaryWithObjectsAndKeys: @"Kate", @"iPhone", @"Kamal", @"iPad", @"Bill", @"Mobile Web", nil];
NSNumber *shouldUseLiterals = [NSNumber numberWithBool:YES];
NSNumber *buildingStreetNumber = [NSNumber numberWithInteger:10018];
```

### Dot-Notation Syntax

Dot-notation should **always** be used for accessing and mutating properties. Bracket notation is preferred in all other instances.

**Good:**
```objc
view.backgroundColor = [UIColor orangeColor];
[UIApplication sharedApplication].delegate;
```

**Bad:**
```objc
[view setBackgroundColor:[UIColor orangeColor]];
UIApplication.sharedApplication.delegate;
```

### Booleans

Objective-C uses `YES` and `NO`.  Therefore `true` and `false` should only be used for CoreFoundation, C or C++ code.  Since `nil` resolves to `NO` it is unnecessary to compare it in conditions.

Good:

```objc
if (someObject) {}
if (![anotherObject boolValue]) {}
```

Bad:

```objc
if (someObject == nil) {}
if ([anotherObject boolValue] == NO) {}
```

### Methods
In method signatures, there should be a space after the scope (-/+ symbol). There should be a space between the method segments.

**Example**:
```objc
- (void)setExampleText:(NSString *)text image:(UIImage *)image;
```

### Variables

Variables should be named as descriptively as possible. Single letter variable names should be avoided except in `for()` loops.

Asterisks indicating pointers belong with the variable, e.g., `NSString *text` not `NSString* text` or `NSString * text`, except in the case of constants.

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

### Conditionals

Conditional bodies should always use braces even when a conditional body could be written without braces (e.g., it is one line only) to prevent errors. These errors include adding a second line and expecting it to be part of the if-statement. Another, [even more dangerous defect](http://programmers.stackexchange.com/a/16530) may happen where the line "inside" the if-statement is commented out, and the next line unwittingly becomes part of the if-statement. In addition, this style is more consistent with all other conditionals, and therefore more easily scannable.

**Preferred:**
```objc
if (!error) {
  return success;
}
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

### Enumerated Types
When using `enum`s, it is recommended to use the new fixed underlying type specification because it has stronger type checking and code completion. The SDK now includes a macro to facilitate and encourage use of fixed underlying types — `NS_ENUM()`

**Example:**
```objc
typedef NS_ENUM(NSInteger, NYTAdRequestState) {
    NYTAdRequestStateInactive,
    NYTAdRequestStateLoading
};
```

### CGRect Functions
When accessing the `x`, `y`, `width`, or `height` of a `CGRect`, always use the [`CGGeometry` functions](http://developer.apple.com/library/ios/#documentation/graphicsimaging/reference/CGGeometry/Reference/reference.html) instead of direct struct member access. From Apple's `CGGeometry` reference.

**Good:**
```objc
CGRect frame = self.view.frame;
CGFloat x = CGRectGetMinX(frame);
CGFloat y = CGRectGetMinY(frame);
CGFloat width = CGRectGetWidth(frame);
CGFloat height = CGRectGetHeight(frame);
```

**Bad:**
```objc
CGRect frame = self.view.frame;
CGFloat x = frame.origin.x;
CGFloat y = frame.origin.y;
CGFloat width = frame.size.width;
CGFloat height = frame.size.height;
```

### Golden Path
When coding with conditionals, the left hand margin of the code should be the "golden" or "happy" path.  That is, don't nest `if` statements.  Multiple return statements are ok.

**Good:**
```objc
- (void)someMethod {
    if (![someOther boolValue]) return;
    //Do something important
}
```

**Bad:**
```objc
- (void)someMethod {
    if ([someOther boolValue]) {;
        //Do something important
    }
}
```

## Xcode project
The physical files should be kept in sync with the Xcode project files in order to avoid file sprawl. Any Xcode groups created should be reflected by folders in the filesystem. Code should be grouped not only by type, but also by feature for greater clarity.


## Testing
* You are responsible for thoroughly testing your own code before passing off to quality assurance.
* Your code must always be tested by a minimum of one other person that is not you.


## Resources
The following is a list of existing style guides that have incluenced our approach to our work.

* [Robots & Pencils](https://github.com/RobotsAndPencils/objective-c-style-guide)
* [New York Times](https://github.com/NYTimes/objective-c-style-guide)
* [Google](http://google-styleguide.googlecode.com/svn/trunk/objcguide.xml)
* [GitHub](https://github.com/github/objective-c-conventions)
* [Adium](https://trac.adium.im/wiki/CodingStyle)
* [Sam Soffes](https://gist.github.com/soffes/812796)
* [CocoaDevCentral](http://cocoadevcentral.com/articles/000082.php)
* [Luke Redpath](http://lukeredpath.co.uk/blog/my-objective-c-style-guide.html)
* [Marcus Zarra](http://www.cimgf.com/zds-code-style-guide/)

