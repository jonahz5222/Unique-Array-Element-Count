# Unique Array Element Count Explanation
A description of an algorithm to determine the number of unique elements in an array, implemented using tuples and closures in Swift 4 as function in an extension of the built in Array type.

Source code can be found [here](https://content.techinnovator.info/mu/sp19/INFOTC4445/challenges/Array%20Unique%20Element%20Count%20Extension/ArrayUniqueElementCount1.playground.zip). It would be very helpful to follow along with this as you read, and I highly suggest it.

This function is contained within an extension. An extension is a method of adding your own properties and functions onto an already defined type. You can implement this on your own, custom types, or on included types. In this case we are doing the latter, extending the built in Array type. The first thing we do is apply the `where` keyword to our extension. 

```swift extension Array where Element: Comparable```

This limits the extension to arrays in which the elements contained in it comply to the `Comparable` protocol. This means that all of the elements contained in the array must be able to be 'Compared' using the comparision operators: 
`<, <=, >=, and >`

This is necessary for our implementation. 


We then create our function, `countUniqueElements() -> Int` which returns an integer and takes no parameters. It is able to take no paramters because it is an extension of the array, and thus will have access to the array that it is called on, via the `self` keyword. Thus, we will then sort our array: 

```swift 
let sorted = self.sorted(by: <)
```

This is necessary so that all non-unique elements will be adjacent to eachother in the array. For example:

This array: 
`let values = [-1, -4, 0, 4, 34, 5, 0, -1, -1, -4, 0, 5]`

will become: 
`[-4, -4, -1, -1, -1, 0, 0, 0, 4, 5, 5, 34]`

Next, we will construct our initial tuple (A tuple is a data structure that holds a pair of data, with the first element being accesed using `tupleName.0` and the second being accessed using `tupleName.1`)

```swift 
let start: (Element?, Int) = (.none, 0)
```

In this case, our tuple, `start` has an `Element?` as the first type and an `Int` as the second type. This first item will hold the previous element as we iterate through the array, and the second item will hold the current number of unique elements.

Now on to the complicated part:

```swift
let result = sorted.reduce(start) {
    (previousResult, currentElement) in
    
    var uniqueCount = previousResult.1
    if (previousResult.0 != currentElement) {
        uniqueCount += 1
    }
 
    return((currentElement, uniqueCount))
}
```

Here we call the `reduce` function on `sorted`. This takes 2 parameters, the first of which is the initial value for the parameters passed to the 2nd argument, the closure. 

You can think of a closure as an inline function, one that is passed to our sorted function so that it is able to use it, but it isn't necessary to write it elsewhere, as it is only needed right here. As such, you could right a function, and then pass it to closure, as demonstrated below: 

```swift
func backward(_ s1: String, _ s2: String) -> Bool {
    return s1 > s2
}

var reversedNames = names.sorted(by: backward)
```

Here, the sorted function, which takes a closure, in this case a function that explains to sorted how to sort the array, is passed an alredady created function, `backward` which fits the needs of the closure.

In our case, we have no such function written, so we write it inline. 

The sorted function returns the result of comibing the elements of the array using the closure that we pass to it.

Our closure takes 2 parameters, `previousResult` and `currentElement`. As mentioned above, these receive their initial values from `start`. `previousResult` is equal to `nil` and `currentElement` is equal to 0.

We then store the unique element count of the previous result: 

```swift 
var uniqueCount = previousResult.1
```

and check to see if we have encountered a new element yet:

```swift
if (previousResult.0 != currentElement) {
    uniqueCount += 1
}
```

if we do, we increment our unique count. Then we return the currentElement and our count of unique elements: 
```swift
return((currentElement, uniqueCount))
```

The closure calls this on the entire array, iterating through it, and eventually returns 1 tuple. In the case of our example array above, `(34, 6)` where 6 is the unique element count.

And remember, you can try this out by viewing the source code [here](https://content.techinnovator.info/mu/sp19/INFOTC4445/challenges/Array%20Unique%20Element%20Count%20Extension/ArrayUniqueElementCount1.playground.zip) 
