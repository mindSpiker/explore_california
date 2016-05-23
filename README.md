iOS Style Guide
===============


## Contents
* [Commenting Functions](#commenting_functions)
*  [Commenting Examples](#commenting-examples)
* [Conditionals](#conditionals)




<a name="commenting_functions"></a>
## Commenting Functions
- To assist in commenting functions and to help standardize you can use the [VVDocumenter-Xcode](https://github.com/onevcat/VVDocumenter-Xcode) plug-in.
- When commenting functions for Objective C use the following format:

### Examples
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

<a name="#conditionals"></a>
## Conditionals

### Ternary Operator
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

### If Else

 - Testing object is/isn't nil
```
if (someObject) // do not use 'if (someObject != nil)'
if (!someObject) // do not use 'if (someObject == nil)'
```
 - Testing string is nil and has characters
```
if (someString.length) // do not check nil and length separately
```
 - Testing array is not nil and not empty
```
if (someArray.count) // do not check nil and length separately
```

 - Testing numeric is not zero
```
if (someNumber) // do not use 'if (someNumber == 0)'
```

 - Always use brackets with if and else statements
```
if (varToCheck) {
    // Even if you only have one line 
    varToCheck = false;
} else {
    varToCheck = true;
}
```
