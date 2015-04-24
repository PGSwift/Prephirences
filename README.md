# Prephirence
[![License](https://img.shields.io/badge/License-MIT-lightgreen.svg?style=flat
            )](http://mit-license.org)
[![Platform](http://img.shields.io/badge/platform-iOS/MacOS-orange.svg?style=flat
             )](https://developer.apple.com/resources/)
[![Language](http://img.shields.io/badge/language-swift-brightgreen.svg?style=flat
             )](https://developer.apple.com/swift)
[![Issues](https://img.shields.io/github/issues/phimage/Prephirence.svg?style=flat
           )](https://github.com/phimage/Prephirence/issues)

![Logo](/logo-128x128.png)

Prephirence is a Swift library that provides useful protocols and methods to manage preferences.

Preferences could be user preferences (NSUserDefaults) or your own private application preferences - any object which implement protocol [PreferencesType](/Prephirence/PreferencesType.swift)

## Contents ##
- [Usage](#usage)
  - [Creating](#creating)
  - [Accessing](#accessing)
  - [Modifying](#modifying)
  - [NSUserDefaults](#nsuserdefaults)
  - [Proxing user defaults](#proxing-user-defaults)
  - [Composing](#composing)
- [Setup](#setup)
  - [Using xcode project](#using-xcode-project)
  - [Using cocoapods](#using-cocoapods)
- [Licence](#licence)

# Usage #

## Creating ##
The simplest implementation of [PreferencesType](/Prephirence/PreferencesType.swift) is [DictionaryPreferences](/Prephirence/DictionaryPreferences.swift)
```swift
// From Dictionary
var fromDico = DictionaryPreferences(myDictionary)
// or literal
var fromDicoLiteral: DictionaryPreferences = ["myKey", "myValue", "bool", true]

// From filepath
if let fromFile = DictionaryPreferences(filePath: "/my/file/path") {..}
// ...in main bundle ##
if let fromFile = DictionaryPreferences(filename: "prefs", ofType: "plist") {..}
```

## Accessing ##
You can access with all methods defined in PreferencesType protocol

```swift
if let myValue = fromDicoLiteral.objectForKey("myKey") {..}
if let myValue = fromDicoLiteral["bool"] as? Bool {..}

var hasKey = fromDicoLiteral.hasObjectForKey("myKey")
var myValue = fromDicoLiteral.boolForKey("myKey")
..

```

## Modifying ##

Modifiable preferences implement protocol [MutablePreferencesTypes](/Prephirence/PreferencesType.swift)

The simplest implementation is [MutableDictionaryPreferences](/Prephirence/DictionaryPreferences.swift)

```swift
// From Dictionary
var mutableFromDico: MutableDictionaryPreferences = ["myKey", "myValue"]

mutableFromDico["newKey"] = "newValue"
mutableFromDico.setObject("myValue", forKey: "newKey")
mutableFromDico.setBool(true, forKey: "newKey")

```

## NSUserDefaults ##

NSUserDefaults implement PreferencesType and can be acceded with same methods

```swift
let userDefaults = NSUserDefaults.standardUserDefaults()

if let myValue = userDefaults["mykey"] as? Bool {..}
```

NSUserDefaults implement also MutablePreferencesType and can be modified with same methods
```swift
userDefaults["mykey"] = "myvalue"
```

### Proxying user defaults ###
You can defined a subcategory of NSUserDefaults prefixed with your own string like that
```swift
let myAppPrefs = userDefaults["myAppKey"] as! MutableProxyPreferences
// We have :
userDefaults["myAppKey.myKey"] == myAppPrefs["myKey"] // is true
```
This allow prefixing all your defaults with same key

## Composing ##

Composing allow to aggregate multiples PreferencesType objects into one PreferencesType

```swift
let myPreferences = CompositePreferences([fromDico, fromFile, userDefaults])
// With array literal
let myPreferences: CompositePreferences = [fromDico, fromFile, userDefaults]

// Mutable, only first mutable will be affected
let myPreferences: MutableCompositePreferences = [fromDico, fromFile, userDefaults]
```

You can access or modify this composite preferences like any PreferencesType.

1. When accessing, first preferences that define a value for a specified key will respond
2. When modifying, first mutable preferences will be affected

The main goal is to define read-only preferences for your app (in code or files) and some mutable preferences (like NSUserDefaults). You can then access to one preference value without care about the origin

# Setup #

## Using xcode project ##

1. Drag Prephirence.xcodeproj to your project/workspace or open it to compile it
2. Add the Prephirence framework to your project

## Using [cocoapods](http://cocoapods.org/) ##

Add `pod 'Prephirence', :git => 'https://github.com/phimage/Prephirence.git'` to your `Podfile` and run `pod install`. Add `use_frameworks!` to the end of the `Podfile`.

# Licence #
```
The MIT License (MIT)

Copyright (c) 2015 Eric Marchand (phimage)

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```