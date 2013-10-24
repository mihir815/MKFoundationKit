# LINQ for Objective-C

[![Build Platform](http://cocoapod-badges.herokuapp.com/p/LINQ4Obj-C/badge.png)](https://github.com/michalkonturek/LINQ4Obj-C)
[![Build Version](http://cocoapod-badges.herokuapp.com/v/LINQ4Obj-C/badge.png)](https://github.com/michalkonturek/LINQ4Obj-C)
[![Build Status](https://travis-ci.org/michalkonturek/LINQ4Obj-C.png?branch=master)](https://travis-ci.org/michalkonturek/LINQ4Obj-C)

Provides fluent interface of LINQ-style query to Objective-C.

## Intro

This project ports LINQ standard query operators to Objective-C. This is achieved by collection of categories for `NSArray` and `NSDictionary` classes.

This library is available through [CocoaPods][PODS].

## License

Source code of this project is available under the standard MIT license. Please see [the license file][LICENSE].

[PODS]:http://cocoapods.org/
[LICENSE]:https://github.com/michalkonturek/LINQ4Obj-C/blob/master/LICENSE


# API

* [Aggregation Operations](#aggregation-operations)
* [Converting Operations](#converting-operations)
* [Filtering Operations](#filtering-operations)
* [Generation Operations](#generation-operations)
* [Grouping Operations](#grouping-operations)
* [Partitioning Operations](#partitioning-operations)
* [Projection Operations](#projection-operations)
* [Quantifier Operations](#quantifier-operations)
* [Set Operations](#set-operations)
* [Sorting Operations](#sorting-operations)

## Aggregation Operations

#### Aggregate

 Performs a custom aggregation operation on the values of a collection.

```objc
- (id)LINQ_aggregate:(LINQAccumulatorBlock)accumulatorBlock;
```

The following example creates a coma-separated string from an array of strings.

```objc
NSArray *input = @[@"M", @"A", @"R", @"K"];
NSString *result = [input LINQ_aggregate:^id(id item, id aggregate) {
    return [NSString stringWithFormat:@"%@, %@", aggregate, item];
}];	
// Result is: @"M, A, R, K"
```

#### Average

Calculates the average value of a collection of values.

```objc
- (id)LINQ_avg;
```

Calculates the average value of the attribute specified
by the key parameter for all objects in the collection.

```objc
- (id)LINQ_avgForKey:(NSString *)key; 
```

Example: Return the average length of strings in the collection.

```objc
NSArray *words = @[@"A", @"AB", @"ABC", @"ABCD", @"ABCDE"];
NSNumber *avg_word_length = [words LINQ_avgForKey:@"length"];
// Result is 3.
```


#### Count

Counts the elements in a collection, optionally only 
those elements that satisfy a predicate function.

```objc
- (NSUInteger)LINQ_count:(LINQConditionBlock)conditionBlock;
```

Example: Return the number of elements 
in the collection that are not smaller than 8.

```objc
NSArray *numbers = [NSArray LINQ_from:1 to:10];
NSInteger *count = [numbers LINQ_count:^BOOL(id item) {
        return ([item compare:@8] != NSOrderedAscending);
    }];
// Count is 3.
```


#### Max

Determines the maximum value in a collection.

```objc
- (id)LINQ_max;
```

Calculates the max value of the attribute specified
by the key parameter for all objects in a collection.

```objc
- (id)LINQ_maxForKey:(NSString *)key;- 
```


#### Min

Determines the minimum value in a collection.

```objc
- (id)LINQ_min;
```

Calculates the min value of the attribute specified
by the key parameter for all objects in the collection.

```objc
- (id)LINQ_minForKey:(NSString *)key;
```


#### Sum

 Calculates the sum of the values in a collection.

```objc
- (id)LINQ_sum;
```

 Calculates the sum of values of the attribute specified
 by the key parameter for all objects in a collection.

```objc
- (id)LINQ_sumForKey:(NSString *)key;
```


<!--## Concatenation Operations-->


## Converting Operations

#### ToArray

Puts value elements into an NSArray.

```objc
- (NSArray *)LINQ_toArray;
```

Puts value elements into a NSArray which satisfy key condtion.
 
```objc
- (NSArray *)LINQ_toArrayWhereKey:(LINQConditionBlock)conditionBlock;
```

Puts value elements into a NSArray which satisfy value condtion.

```objc
- (NSArray *)LINQ_toArrayWhereValue:(LINQConditionBlock)conditionBlock;
```

Puts value elements into a NSArray which satisfy both key and value condtion.

```objc
- (NSArray *)LINQ_toArrayWhereKeyValue:(LINQKeyValueConditionBlock)conditionBlock;
```

#### ToDictionary

 Puts elements into an index-key-based NSDictionary.

```objc
- (NSDictionary *)LINQ_toDictionary;
```
<!--
 Example: 

```objc
[@[@"A", @"B", @"C", @"D", @"E"] LINQ_ToDictionary];

// Result is: 
// {
// 		{0 : @"A"},
// 		{1 : @"B"},
// 		{2 : @"C"},
// 		{3 : @"D"},
// 		{4 : @"E"},
// }

```
-->

Puts elements into a NSDictionary based on a key selector function.

```objc
- (NSDictionary *)LINQ_toDictionaryWithKeySelector:(LINQSelectorBlock)keySelector;
```

Puts elements into a NSDictionary based on a key and value selector functions.

```objc
- (NSDictionary *)LINQ_toDictionaryWithKeySelector:(LINQSelectorBlock)keySelector
                                     valueSelector:(LINQSelectorBlock)valueSelector;
```


## Filtering Operations

#### OfType

Selects values, depending on their ability to be cast to a specified type.

```objc
- (instancetype)LINQ_ofType:(Class)klass;
```
<!--
 Example: 

```objc
```
-->

Selects elements which keys can be cast to a specified type.

```objc
- (instancetype)LINQ_ofTypeKey:(Class)klass;
```

Selects elements which values can be cast to a specified type.

```objc
- (instancetype)LINQ_ofTypeValue:(Class)klass;
```

#### Where

 Selects values that are based on a predicate function.

```objc
- (instancetype)LINQ_where:(LINQConditionBlock)conditionBlock;
```
<!--
 Example: 

```objc
```
-->

Selects values which satisify key-value condition.

```objc
- (instancetype)LINQ_where:(LINQKeyValueConditionBlock)conditionBlock;
```

Selects values which keys satisify condition.

```objc
- (instancetype)LINQ_whereKey:(LINQConditionBlock)conditionBlock;
```

Selects values which satisify condition.

```objc
- (instancetype)LINQ_whereValue:(LINQConditionBlock)conditionBlock;
```

## Generation Operations

#### Empty

Returns empty array.

```objc
+ (instancetype)LINQ_empty;
```

#### From:To:

Creates array with integers from to.

```objc
+ (instancetype)LINQ_from:(NSInteger)from to:(NSInteger)to;
```

#### Repeat

Generates a collection that contains one repeated value.

```objc
+ (instancetype)LINQ_repeat:(id)element count:(NSInteger)count;
```


## Grouping Operations


#### GroupBy

 Returns NSDictionary of groups that share a common
 attribute defined by selector. Each group is defined
 as a dictionary entry whose key is a result of a selector
 and its value is an array of all elements that return
 the same key, i.e. selector(element) -> key.


` { key <- selector(element), value <- [element : key = selector(element)] }`


```objc
- (NSDictionary *)LINQ_groupBy:(LINQSelectorBlock)selector;
```

Example: 

```objc
NSArray *words = @[@"Adam", @"Anthony",
					@"Ben", @"Bob",
					@"Michael", @"Max", @"Matt",
					@"Simon"];
NSDictionary *results = [self.input_words LINQ_groupBy:^id(id item) {
    return [item substringToIndex:1];
}];
// Result is:
// {
// 		{"A" : @[@"Adam", @"Anthony"]},
// 		{"B" : @[@"Ben", @"Bob"]},
// 		{"M" : @[@"Michael", @"Max", @"Matt"]}
//		{"S" : @[@"Simon"]}
// }
```

#### ToLookup

Returns array of NSDictionaries by entering each element
into a NSDictionary whose key is a result of a selector
and its value is an element: ` { key <- selector(element), value <- element}`
 
```objc
- (instancetype)LINQ_toLookup:(LINQSelectorBlock)selector;
```

<!--
Example: 

```objc
```
-->

#### Lookup

Helps to filter results of toLookup: method.
Returns array of NSDictionaries with the same key.

```objc
- (instancetype)LINQ_lookup:(id)key;
```

<!--
Example: 

```objc
```
-->

<!--## Join Operations-->

<!--## Element Operations-->

<!--## Equality Operations-->


## Partitioning Operations

#### Skip

Skips elements up to a specified position in a collection.

```objc
- (NSArray *)LINQ_skip:(NSInteger)count;
- (NSDictionary *)LINQ_skip:(NSInteger)count;
```

<!--
Example: 

```objc
NSArray *result = [[NSArray LINQ_from:1 to:10] LINQ_skip:5];
// result is @[@6, @7, @8, @9, @10]
```
-->

#### Take

Takes elements up to a specified position in a collection.

```objc
- (NSArray *)LINQ_take:(NSInteger)count;
- (NSDictionary *)LINQ_take:(NSInteger)count;
```
<!--
Example: 

```objc
NSArray *result = [[NSArray LINQ_from:1 to:10] LINQ_take:5];
// result is @[@1, @2, @3, @4, @5]
```
-->

## Projection Operations

#### Select

 Projects values that are based on a transform function.

```objc
- (instancetype)LINQ_select:(LINQSelectorBlock)selectorBlock;
```

The example below adds 10 to each element in the collection.

```objc
NSArray *result = [[NSArray LINQ_from:1 to:5] LINQ_select:^id(id item) {
    return [NSNumber numberWithInteger:([item integerValue] + 10)];
}];
// result is @[@11, @12, @13, @14, @15];
```

#### Select Many

 Projects sequences of values that are based on a transform
 function and then flattens them into one sequence.

```objc
- (instancetype)LINQ_selectMany:(LINQSelectorBlock)selectorBlock;
```

This example returns words of each string of the collection.

```objc
NSArray *input = @[@"an apple a day", @"the quick brown fox"];
NSArray *result = [input LINQ_selectMany:^id(id item) { 
    return [item componentsSeparatedByString:@" "]; 
}]; 
// result is @[@"an", @"apple", @"a", @"day", 
//			@"the", @"quick", @"brown", @"fox"]
//
```


## Quantifier Operations

#### All

Determines whether all the elements in a sequence satisfy a condition.

```objc
- (BOOL)LINQ_all:(LINQConditionBlock)conditionBlock;
- (BOOL)LINQ_all:(LINQKeyValueConditionBlock)conditionBlock;
```

#### Any

Determines whether any elements in a sequence satisfy a condition.
 
```objc
- (BOOL)LINQ_any:(LINQConditionBlock)conditionBlock;
- (BOOL)LINQ_any:(LINQKeyValueConditionBlock)conditionBlock;
```

## Set Operations

#### Distinct

 Removes duplicate values from a collection.

```objc
- (instancetype)LINQ_distinct;
```

#### Except

 Returns the collection without the elements
 that appear in a second collection.

```objc
- (instancetype)LINQ_except:(NSArray *)other;
- (instancetype)LINQ_except:(NSDictionary *)other;
```

#### Intersect

 Returns the set intersection, which means elements
 that appear in each of two collections.

```objc
- (instancetype)LINQ_intersect:(NSArray *)other;
- (instancetype)LINQ_intersect:(NSDictionary *)other;
```

#### Union

 Returns the set union, which means unique elements
 that appear in either of two collections.

```objc
- (NSArray *)LINQ_union:(NSArray *)other;
```

Merges to dictionaries by returning the set union
of unique elements which keys appear in either of
two dictionaries.

```objc
- (NSDictionary *)LINQ_merge:(NSDictionary *)other;
```


## Sorting Operations

#### OrderBy

 Sorts values in ascending order.

```objc
- (instancetype)LINQ_orderByAscending;
```

 Sorts values in descending order.

```objc
- (instancetype)LINQ_orderByDescending;
```

 Sorts elements of a collection depending on an element's key.

```objc
- (instancetype)LINQ_orderByKey:(NSString *)key ascending:(BOOL)ascending;
```


#### Reverse

 Reverses the order of the elements in a collection.

```objc
- (instancetype)LINQ_reverse;
```

