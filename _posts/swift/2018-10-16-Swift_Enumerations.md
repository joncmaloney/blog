---
layout: post
title: Swift Enumerations
summary: 
author: jon_maloney
---

Swift Enumerations
-----------------------------

Enums allow us to create new data types. It's swift convention for data types to be upper case. It's also convention for all the enum values to be lower case. 

There are two ways to define an enum verbose case or inline case. 

```swift
	enum MediaType{
		case book
		case movie 
		case music 
		case game
	}
```

```swift
	enum MediaType{
		case book, movie, music, game
	}
	
```

Notice when we are using verbose case statements each case statement doesn't requrire a comma delimiter. However in the inline case a comma is required. 


Defining an enum variable
-----------------------------

To define the variable we will declare the variable as the enum type. 

```swift
	var itemType: MediaType

	itemType = MediaType.book
```


Swift lets us use an assignment shortcut to we dont need to write out the enum type for every assignment. So if we wanted to change the above variable to movie we don't need to write 'MediaType' we just need the 'dot'.

```swift
	itemType = .movie
```

Switch statement
-----------------------------

```swift
switch itemType {
	case .movie
		print("I've been watching \(itemTitle)")
	case .music
		print("I've been listenting to \(itemTitle)")
	case .book
		print("I've been reading \(itemTitle)")
	case .game
		print("I've been playing \(itemTitle)")
}
```

In the above example we don't need to use a defualt case as swift recognises that the all case statement have been exausted.

Raw values
-----------------------------

Swift allows us to define raw values for each enum value. Take a new enum of bottle sizes:

```swift
	enum BottleSize: String {
		case half = "37.5 cl"
		case standard = "75 cl"
		case magnum = "1.5 litres"
		case jeroboam = "3 litres"
		case rehoboam = "4.5 litres"
		case methuselah = "6 litres"
		case balthazar = "12 litres"
	}
```
We are then able to acces the raw values for each of the enums. 

```swift
	var myBottle: BottleSize = .jeroboam
	print("The bottle \(myBottle)" is \(myBottle.rawValue))
```
