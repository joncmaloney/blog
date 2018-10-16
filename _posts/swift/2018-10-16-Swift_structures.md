---
layout: post
title: Swift Structures
summary: 
author: jon_maloney
---


Swift Structures
-----------------------------

All base types in swift are structures. These include int's strings bools etc. 

Variables inside a struct are reffered to as properties. 

```swift
	struct Movie{
		//properties
		var title: String
		var director: String
		var releaseYear: Int
		var genre: String
	}

```

Initialising Structs
-----------------------------

When using structs you can create some default values for the structure. However if there aren't any default values then the struct needs to be initialised when it's defined. 

```swift
	var starWars = Movie.init(
	    title: "Star Wars - A new Hope",
	    director: "George Lucas",
	    releaseYear: 1977,
	    genre: "Science Fiction")
```

Struct methods
-----------------------------

We can add methods to structs just as we would add to classes. Re-using the above example we can add a summary method that returns a String description of the movie. 

```swift
	struct Movie{
		//properties
		var title: String
		var director: String
		var releaseYear: Int
		var genre: String

		// methods
		func summary() -> String{
			return "\(title) is a \(genre) film released in 
			\(releaseYear) and directed by \(director)" 
		}
	}

```

We can then print the movie summary. 

```swift
	print(starWars.summary())
```


Differences between classes
-----------------------------

Structs in swift differ from classes in that classes allow inheritance. Structs cannot be inherited. The other difference is that structures are value types and classes are reference types. 



