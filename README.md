iOS Style Guide
===============

The style guide we (sometimes) (try to) adhere to for iOS code at TripAdvisor. 

For Objective-C code, we follow many of the same principles outlined in the 
[NYTimes Style Guide](https://github.com/NYTimes/objective-c-style-guide).


For Swift code, we follow the [GitHub Swift Style Guide](https://github.com/github/swift-style-guide), except for the following:

* TripAdvisor uses 4 spaces in Swift code instead of tabs.



## Apple Documentation

Some references to the "Official" Documentation. We follow recommendations here unless otherwise noted below.

* [The Objective-C Programming Language](http://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/ObjectiveC/Introduction/introObjectiveC.html)
* [Cocoa Fundamentals Guide](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CocoaFundamentals/Introduction/Introduction.html)
* [Coding Guidelines for Cocoa](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/CodingGuidelines.html)
* [iOS App Programming Guide](http://developer.apple.com/library/ios/#documentation/iphone/conceptual/iphoneosprogrammingguide/Introduction/Introduction.html)


## Contents
* [UI Development](#ui_development)
* [Organization and Documentation](#organization_and_documentation)
* [Third Party Libraries](#third_party_libraries)
* [Spacing](#spacing)
* [Naming](#naming)
* [Properties](#properties)
* [Control Structures](#control_structures)
    * [Conditionals](#conditionals)
    * [If / Else](#if_else)
    * [Switch](#switch)
    * [For / While](#for_while)
* [Ternary Operator](#ternary_operator)
* [Literals](#literals)
* [Designated Initializers](#designated_initializers)
* [Constants](#constants)
* [Types](#types)
* [Enums](#enums)
* [Singletons](#singletons)
* [Memory Management](#memory_management)
* [Classes and Methods](#classes_and_methods)
* [Blocks](#blocks)
* [Exceptions and Errors](#exceptions_and_errors)
* [Categories](#categories)
* [Commenting Functions](#commenting_functions)
* [Conditionals](#conditionals)


## Misc (need a home)
- Avoid `NSLog`, prefer `RKLogDebug` instead.
- Ensure that the code you introduced works on our lowest supported SDK.
- Follow DRY rule: https://en.wikipedia.org/wiki/Don%27t_repeat_yourself

<a name="ui_development"></a>
## UI Development
- Use Interface Builder for creating new UI (Storyboards or NIBs) as opposed to programming constraints unless creating a highly dynamic complex view. Several reasons for this:
    - Helps avoid auto layout warnings at runtime (Xcode will warn you of ambiguous, missing or conflicting constraints)
    - Allows us to easily and cleanly take advantage of Size Classes when needed
    - Clearly separates view code from business logic
See this [Auto Layout Case Study](https://docs.dev.tripadvisor.com/display/MOB/Auto+Layout+Case+Study) for example use cases for using IB vs programmatic approach.

<a name="organization_and_documentation"></a>
## Organization and Documentation 
- Each feature (for example Dual Search, Results Filters) should have a folder. Within each feature folder, the various components should have subfolders eg. Views, View Models, Model, DataSources, etc. Feature folder for your project should be an actual folder, but subfolders can either be real or virtual folders/groups in Xcode. Common objects used by multiple features should go in the general folders (Code/Controllers, Code/Models).

**Example**

Xcode Project Navigator:

```
Code/
├── DualSearch
│   ├── BaseClasses
│       ├── TAAbstractListItemViewModel.h
│       ├── TAAbstractListItemViewModel.m
│   ├── Behaviors
│       ├── TADualSearchBarHighlightingBehavior.h
│       ├── TADualSearchBarHighlightingBehavior.m
│   ├── DataSources
│       ├── TADualSearchReviewableLocationDataSource.h
│       ├── TADualSearchReviewableLocationDataSource.m
```

Filesystem:

```
Code/
├── DualSearch
│   ├── TAAbstractListItemViewModel.h
│   ├── TAAbstractListItemViewModel.m
│   ├── TADualSearchBarHighlightingBehavior.h
│   ├── TADualSearchBarHighlightingBehavior.m
│   ├── TADualSearchReviewableLocationDataSource.h
│   ├── TADualSearchReviewableLocationDataSource.m
```
See [Xcode Groups and Folder References](http://www.thomashanning.com/xcode-groups-folder-references/) for instructions

- Include documentation on use, parameters, return values for all public methods. 
- Use `#pragma mark` to group like pieces of code and protocol code. `init` and `dealloc` methods belong at the top.
- Define constants at the top of the file so that they are together in a single place.
- Where possible, order methods in a reasonable expected resembling the life cycle.

**Example**

```objc
// constants that are not visible outside of a .m file prefixed with k
const NSUInteger kTANumSections = 10;
const CGFloat kTASectionHeight = 60.f;

// constants that are externally visible outside of a .m file prefixed with TA
extern NSUInteger const TANumSections;

// lifecycle
- (instancetype)init;
- (instancetype)initWithUser:(TAUser *user);
- (void)dealloc;


#pragma mark - UITableViewDelegate
- (NSUInteger)numberOfSections;

- (NSUInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSUInteger)section;

- (CGFloat)tableView:(UITableView *)tableView heightForRowAtIndexPath:(NSIndexPath)indexPath;

- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath)indexPath;
```

<a name="third_party_libraries"></a>
## Third Party Libraries
See [Third Party Libraries](https://docs.dev.tripadvisor.com/display/DEV/Third+Party+Libraries) for information on why/when/how to introduce a third party library into your project.

<a name="spacing"></a>
## Spacing
- Take advantage of spaces to improve readability, follow Apple examples where available.
- Include line breaks between methods to improve readability (2).
- **Indent using 4 spaces. Avoid Tabs** (change this preference in Xcode, otherwise you will have to deal with strange merge conflicts and misaligned code reviews)
- Start braces on the same line (for `ta-ios-flights` pod, method braces start on the next line, control structures on the same line)

**Example**

```objc

- (NSString *)methodWithString:(NSString *)string andAnotherArgument:(NSUInteger)integerArgument {
    // Include a space after the type, before the *, with no space between the * and the variable name.
    NSString *anotherString = @"here is another string";
    
    // For conditionals, include a space between if and (.
    if (!integerArgument) {
        return nil;
    }
    
    // include spaces after commas to improve readability
    return [NSString stringWithFormat:@"%@, %@, %d", string, anotherString, integerArgument];
    
}
```

> The -/+ should be the first character of the line, followed by a space. 
> There should be no spaces within the return type (void), or between the type and the method name.


Include spaces around operators, after commas, and where it improves readability

**Example**

```objc
NSUInteger number = (1 + 2) * 3;
NSArray *array = @[@"one", @"two", @"three"];
BOOL isNegative = number >= 0 ? NO : YES;

```

<a name="naming"></a>
## Naming
- Adhere to Apple conventions for naming (explicit, informative, aka "verbose")
- Return types and input types should be obvious from naming. 
- Prefix Classes with `TA`, `TAC` (commons), `TAF` (flights) (and methods and constants where needed).
- Avoid abbreviations other than common ones (num, max, min).

<a name="properties"></a>
## Properties
- Use `copy` for types with mutable options (ie NSArray, NSDictionary, NSString). Protect yourself against the wrong type being passed in.
- Expose the immutable type when possible.
- Make use of `readonly` where applicable to force immutability
- Explicitly include `strong` identifier even though it is default. Be explicit. 
- Use `instancetype` over `id` for constructors.
- include `is`, `has`, etc. for `BOOL` getters.
- Use nullable, _Nonnull for all new code. Note that you must annotate an entire file with nullable/_Nonnull to avoid introducing new warnings (new warnings will break the build).


**Example**

```objc
@property (nonatomic, assign, getter=isSaved) BOOL saved;

```

- Prefer dot-notation for getting and setting properties of objects. Use brackets for methods. One benefit of keeping style different for properties and methods is that it makes it more obvious how the messages are sent. Another is that Xcode shows a P for properties and M for methods in autocomplete popups.

<a name="control_structures"></a>
## Control Structures
- Always include a space after control structure, before (
- Braces start on same line, end on a new line.
- Return early. Prefer a negative condition to return over several layers of nested `if` when possible
- No spaces between parenthesis and their contents inside.

**Prefer**

```objc
- (void)preformSomeMethodWithString:(NSString *)string {
    if (!string.length) return;
    
    // do other code here that may be more complex
}
```

**Over** 

```objc
- (void)preformSomeMethodWithString:(NSString *)string {
    if (string.length) {
    
        if (another expression) {
            // do other code here that may be more complex
        }
    }
}
```

<a name="if_else"></a>
### If / Else
- Don't compare against nil, or 0
- for strings, arrays, check length count
- For short statements, keep simple w/ 1 line (but avoid lacking braces and putting result on a separate line)

**Example**

```objc
- (BOOL)myTestMethodForInput:(NSString *)input andList:(NSArray *)list {
    // simple case
    if (!input.length) return NO;
    
    NSUInteger listCount = [list count];
    if (!listCount) {
        
    } else if (listCount >= 10) {
        
    } else {
        
    }

}

```

<a name="switch"></a>
### Switch
- Avoid the use of `default` when dealing with `ENUMs`. By avoiding you will receive warnings when you forget to support a value in the enum, which can help to future-proof your code if someone adds a new value. Instead, group cases together and handle default case explicitly there.
- Include braces around cases for consistency


**Example**

```objc
- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath {

    UITableViewCell *cell = nil;
    switch (indexPath.section) {
        
        case TAPOIDetailsSectionHeader: 
        case TAPOIDetailsSectionSubHeader: {
            cell = [TAResultsCell headerCell];
            break;
        }
        
        case TAPOIDetailsSectionName: {
            cell = [TAResultsCell sectionNameCell];
            break;
        }
            
        case TAPOIDetailsSectionOverview: {
            cell = [TAResultsCell overviewCell];
            break;
        }
        
        // explicitly handle other cases rather than relying on default.
        case TAPOIDetailsSectionUnsupported:
        case TAPOIDetailsSectionUnsupportedAgain: {
            break;
        }
        
        // warning: Enumeration values 'TAPOIDetailsSectionNew' and 'TAPOIDetailsSectionUnsupported' not handled in switch
    }
            
    if (!cell) {
        cell = [TAResultsCell defaultCell];
    }
    
    return cell
}
```

<a name="for_while"></a>
### For / While 
- Prefer fast enumeration where possible

```objc
for (TAReview *review in location.reviews) {
    // do stuff here
}
```

<a name="ternary_operator"></a>
## Ternary Operator
- Take advantage of the simplicity of the Ternary Operator, but don't get too complicated. Be nice to future programmers who will try to quickly understand your work. If it takes more then 2 seconds to understand the logic, use an if statement instead. Clarity > lines of code.
- Wrap long lines in parenthesis to improve clarity.

**Example**

```objc
return ([self.reviews count] == 1 ? @"1 review" : [NSString stringWithFormat:@"%d reviews", [self.reviews count]]);
```

```objc
// If checking for nil and returning value if present, you can abbreviate the operator like this
return self.user.name ?: @"A TripAdvisor Member";
```

<a name="literals"></a>
## Literals
- Prefer literals for `arrays`, `dictionaries`, and `NSNumbers`. Be careful about accidental nil inserts that can cause crashes.
- For dictionaries, no space between " and : in key, include a space after : before value.

**Example**

```objc
NSArray *array = @[@1, @2, @3];
NSDictionary *dictionary = @{@"key": @"value"};
NSNumber *boolNumber = @YES;
NSNumber *integerNumber = @100;

NSUInteger number = 100;
[array addObject:@(number)];
```

<a name="designated_initializers"></a>
## Designated Initializers
- Protect yourself and your code. If you have a required parameter in an initializer and it is not present, warn the developer using an `NSInvalidArgumentException`. 
- Return `nil` as well when you want to avoid moving forward in a bad state

**Example**
```objc
- (instancetype)initWithUser:(TAUser *)user {
    if (!user) {
        [NSException raise:NSInvalidArgumentException format:@"`user` cannot be `nil`."];
        return nil;
    }
    
    if (self = [super init]) {
        _user = user
    }
    
    return self;  
}

```

<a name="constants"></a>
## Constants
- Prefer `static const`  over `#define`. 
- For public constants, prefix with TA and the name of the class where relevant. For private/internal constants, prefix with kTA.

**Example**

```objc

// Example.h
extern const NSUInteger TAExampleNumReviews;
extern NSString * const TAExampleDefaultName;

// Example.m
static const NSUInteger TAExampleNumReviews = 20;
static NSString * const TAExampleDefaultName = @"A TripAdvisor Member";

static const CGFLoat kTAExamplePrivateConstant = 10.5;
```

> Using constants gives better information about the type, and helps to avoid simple mistakes
> The use if kTA vs. TA makes the scope of the constant more obvious. In general, the more obvious the better.
>
> Note: `NSString * const` does not follow normal variable naming, which has no space between * and const. This is an exception here introduced by Apple, and we follow Apple's example in this case.

<a name="types"></a>
## Types
- Prefer `NSInteger` over `int`, and `CGFloat` over `float`. When applicable, prefer `NSUInteger` over `NSInteger`
- Prefer `typedef` when applicable, such as `NSTimeInterval` over `double`.

<a name="enums"></a>
## Enums
- Use `NS_ENUM` where possible (and `NS_OPTIONS`)

**Example**

```objc
typedef NS_ENUM(NSUInteger, TAAttractionSubType) {
    TAAttractionSubTypeAttraction,
    TAAttractionSubTypeShopping,
    TAAttractionSubTypeNightlife,
    TAAttractionSubTypeActivities,
};
```

<a name="singletons"></a>
## Singletons
- When creating singletons, be thread-safe and use `dispatch_once`

**Example**

```objc
+ (TALoginManager *)sharedInstance {
    static TALoginManager *sharedInstance = nil;
    static dispatch_once_t onceToken;
    dispatch_once(&onceToken, ^{
        sharedInstance = [[TALoginManager alloc] init];
    });
    
    return sharedInstance;
}

```

<a name="classes_and_methods"></a>
## Classes and Methods
- Prefer `@class` over `#import` when possible.
- Remove methods that simply just call `super`. They are superfluous.
- Prefer short methods with concise names over long and hard to follow methods.
- Prefer direct access to member variables (_varname) instead of referencing self in init methods.
- Variables should be declared near their first use.
- When deleting code ensure that you have removed the #imports.
- Keep classes small, divide your code into multiple classes.

<a name="menory_management"></a>
## Memory Management
- If you write an explicit dealloc for a class, ensure it is actually getting called. This will help reduce memory leaks. See [Debugging Retain Cycles in ObjC](http://blog.reigndesign.com/blog/debugging-retain-cycles-in-objective-c-four-likely-culprits/)
- Look for potential retain cycles/memory leaks with instruments. See [Debugging Retain Cycles in ObjC](http://blog.reigndesign.com/blog/debugging-retain-cycles-in-objective-c-four-likely-culprits/)

<a name="blocks"></a>
## Blocks
- Prefer Weak references in blocks, this avoids potential future retain cycles, but also it's very rare we ever want to reference/update anything if it has become nil;
- If you are passing multiple blocks into a method, consider defining a new protocol instead to use as a delegate.
- Avoid long anonymous blocks as they are difficult to find, not  as reusable/accessible by subclasses (depends) and not as debug friendly/readable. Blocks should be used for short, one-use only pieces of code. 
**Example**

```objc

TAWeakSelf weakSelf = self;    
dispatch_async(dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0), ^{
    UIImage *thumbnail = [weakSelf squareThumbnailWithSize:size];
        
    if (shouldCache) {
        [weakSelf bk_associateValue:thumbnail withKey:kTAThumbnailKey];
    }
        
    if (completion) {
        dispatch_async(dispatch_get_main_queue(), ^{
            completion(thumbnail);
        });
    }
});

```

<a name="exceptions_and_errors"></a>
## Exceptions and Errors
- Do not throw an exception unless it is intentional to crash the app (it never should be intentional in production).
- Use raiseOrLog to avoid throwing the exception in production, but only throw it on dev builds. We will still have a record of the exception on the server but it will not trigger a crash in production.
- Safer pattern is to return BOOL and pass in an NSError**

**Example**

```objc

if (![dataSourceClass isSubclassOfClass:[TACalendarDataSource class]]) {
    [NSException raiseOrLogWithName:NSInvalidArgumentException reason:@"Calendar data source class must be a subclass of TACalendarDataSource" userInfo:nil];
    return nil;
}

```

<a name="categories"></a>
## Categories
- Use categories for helper methods on models (keep models clean). Strongly preferred to use of base superclass whenever possible. OK to use objc_setAssociatedObject when necessary for state across methods in the category.
- Do not use very common names for functions in categories, especially if adding a category on common classes such as NSString, NSObject, etc. When in doubt about a potential naming collision, prefix the methods in the category with ta_

<a name="commenting_functions"></a>
## Commenting Functions
- To assist in commenting functions and to help standardize you can use the [VVDocumenter-Xcode](https://github.com/onevcat/VVDocumenter-Xcode) plug-in.
- When commenting functions for Objective C use the following format:

**Example**

```objc

/**
 Get query params for filter, sort, pagination options
 
 @param filterOptions Filter options
 @param sortOptions Sort Options
 @param paginationOptions Pagination Options
 @return NSDictionary Query parameters for all those options
 */
- (NSDictionary *)queryParamsFromFilterOptions:(nullable TAFilterOptions *)filterOptions sortOptions:(nullable TASortOptions *)sortOptions pagingOptions:(nullable TAPagingOptions *)pagingOptions {

```

- For Swift use the following format:

**Example**

```objc

/**
 Get query params for filter, sort, pagination options
     
 - parameter filterOptions: Filter options
 - parameter sortOptions: Sort Options
 - parameter paginationOptions: Pagination Options
     
 - returns: Query parameters for all those options
 */
 func queryParams(filterOptions: TAFilterOptions, sortOptions: TASortOptions, pagingOptions: TAPagingOptions) -> NSDictionary {


```

- Only use "// TODO" (space required so that it does not raise a warning) with an accompanying JIRA, and reference the "// TODO" comment from the JIRA to provide context.

