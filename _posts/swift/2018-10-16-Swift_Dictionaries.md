---
layout: post
title: Swift Dictionaries
summary: 
author: jon_maloney
---



Swift Dictionaries
-----------------------------

Dictionaries are like arrays however their index is not a zero based integer that is incremented for each new value. Instead their index is some other object refered to as a key. A dictionary has keys and values. These are typically referred to as key value pairs. 

A key in a dictionarty must be unique. As this object is  used to reference the value. Both the key and the value must be objects. They don't need to be the same type. However all keys must be of the same type and all values must be of the same type. 

To create a dictionary we can use a dictionary literal format. This uses square brackets to initialise and define a dictionary in a single line of code. 

```swift
	var airlines = ["SWA": "Southwest Airlines", 
					"BAW": "British Airlines",
					"BHA": "Buddah Air", 
					"CPA": "Cathay Pacific"]
```

Alternativeyl a dictionary can be defined before any values are added. 

```swift
	// Dictionary of String keys and String values
	var keyValuePairs: [String: String]

	// Dictionary of Int keys and String values
	var employees: [Int, String]
```


To get a value from our dictionary we can use the key to look up the value. When looking up a value in a dictionary using the key. The returned type is optional. This means we need to unbox it to access the raw value. 

```swift
	if let result = airlines["SWA"] {
		print(result)
	}else{
		print("No match found")
	}
```

Further the key can be used to update values in the dictionary. 

```swift
	airlines["DVA"] = "Discovery Airlines"
```

If the value DVA already exists the below will change it. However if the key doesn't exist the value will be added to the dictionary. 

To remove a value from the dictionary you can set the value to nil. 

```swift
	airlines["BHA"] = nil
```

To itterate over the dictionary we can use a simple for loop.

```swift
	for entry in airlines{
		print(entry)
	}
```

Tupils
-----------------------------

To itterate over the dictionary each key and value can be access via a mechanism called a tupil. Tupils allow us to group values together. A variable can be created as a single compound value. 

```swift
	var basicTuple = (apeture, iso, cameraType)
```


Tuples allow groupings of differnt types which could be literals, constants, and variables. 

```swift
var newTuple = ("Sting literal!", photoMode, 2312, cameraType)
```

The most handy way to use the Turple is to return multiple types from a function. When returning a tuple from a function the type and order must be defined. 

```swift
	func randomAlbum() -> (String, Int) {
		// Code goes here...
		let title = "Some randomAlbum"
		let duration = 3260

		return (title, duration)
	}
```

There are slight variations to the above funciton call. Swift allows you to name the return types.

```swift
	func randomAlbum() -> (title: String, lenght: Int) {
		// Code goes here...
		let title = "Some randomAlbum"
		let duration = 3260

		return (title, duration)
	}

	...


	let result = randomAlbum()

	print("The album title is \(result.title)")

	print("The album length is \(result.lenght)")
```

Also the caller can assign a name to the returned variables. 

```swift
	let (albumName, duration) = randomAlbum()

	print("The album title is \(albumName)")

	print("The album length is \(duration)")
```

This brings us back to looking at dictionaries and looping through them. Swift allows us to decompose a dictionary element in a for loop using a tupil. 

```swift
	for (airlineKey, airlineName) in airlines{
		print("The key is \(airlineKey) and the value is: \(airlineName)")
	}
```