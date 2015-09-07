---
layout: post
title: Adopting Modern Objective-C
category: iOS
tags: [Algorithms]
---

# Adopting Modern Objective-C

## 一，preface

Yesterday, I see an article that talk about something about the instancetype. so I find the document write by apple.
It's name is Adopting Modern Objective-C

## 二，instancetype

Use the instancetype keyword as the return type of methods that return an instance of the class they are called on.

### advantage
<pre>
<code>
@interface MyObject : NSObject
  + (instancetype)factoryMethodA;
  + (id)factoryMethodB;
  @end
  @implementation MyObject
  + (instancetype)factoryMethodA { return [[[self class] alloc] init]; }
  + (id)factoryMethodB { return [[[self class] alloc] init]; }
  @end

  x = [[MyObject factoryMethodA] count]; // Return type of +factoryMethodA is taken to be "MyObject *"
  y = [[MyObject factoryMethodB] count]; // Return type of +factoryMethodB is "id"
</code>
</pre>

Because the instancetype return type of +factoryMethodA, the MyObject doesn't have a -count method, the compiler gives a warning about the method.
So it is safe to programmer.

### How to Adopt

Note that you should replace id with instancetype for return values only, not elsewhere in your code. Unlike id, the instancetype keyword can be used only as the result type in a method declaration.
 
## 三，Properties

Use Properties instead of instance variables.

### advantage
1) Auto synthesized getters and setters.
2) Better declaration of intent of a set of methods.
3) Property keywords that express additional information about behavior.

### How to Adopt

Just use property

## 四，Enumeration Macros

Apple provide NS_ENUM and NS_OPTIONS.

### advantage

NS_ENUM and NS_OPTIONS has also define the type. It is clear for users.

### How to Adopt
<pre>
<code>
typedef NS_ENUM(NSInteger, UITableViewCellStyle) {
    UITableViewCellStyleDefault,
	UITableViewCellStyleValue1,
	UITableViewCellStyleValue2,
	UITableViewCellStyleSubtitle
};

typedef NS_OPTIONS(NSUInteger, UIViewAutoresizing) {
	UIViewAutoresizingNone = 0, 
	UIViewAutoresizingFlexibleLeftMargin = 1 << 0,
	UIViewAutoresizingFlexibleWidth = 1 << 1,
	UIViewAutoresizingFlexibleRightMargin = 1 << 2,
 	UIViewAutoresizingFlexibleTopMargin =1<<3,
  	UIViewAutoresizingFlexibleHeight =1<<4, 
  	UIViewAutoresizingFlexibleBottomMargin = 1 << 5
};
</code>
</pre>

## 五，Enumeration Macros

know the designated initializer and convenience initializer

###advantage

clear for the designated initializer

### How to Adopt
<pre>
<code>

- (instancetype)init NS_DESIGNATED_INITIALIZER;

</code>
</pre>

change the MRC to ARC

## 六，Automatic Reference Counting

who use who know

### advantage

convient

### How to Adopt

change the MRC to ARC

