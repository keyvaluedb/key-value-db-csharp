# <p style="text-align: center;" align="center"><img src="https://github.com/keyvaluedb/keyvaluedb.github.io/raw/master/icons/key-value-db-csharp.png" alt="key-value-db-csharp" style="width:180px;height:160px;" width="180" height="160" /><br /> key-value-db-csharp</p>

<p style="text-align: center;" align="center">Light weight package to quickly and easily manage, load, update and save key-value type database </p>

---

The sample use cases of this package is loading configuration file, language file, preference setting in an application. More use cases can be seen [here](https://keyvaluedb.github.io/usecases/index.html).

The package does not do any Input and Output operation as there are several way to read and write to file and the methods has their strength and weakness therefore the developer has to find the efficient way to load and save locally.

___

## Table of content
- [Installation](#installation)
- [Example](#example)
- [Legends](#legends)
- [API](#api)
	- [Creating/loading a document](#creating/loading-a-document)
	- [Inserting data](#inserting-data)
	- [Finding data](#finding-data)
	    - [Get KeyValue Object](#get-keyvalue-object)
	    - [Get Like KeyValue Object](#get-like-keyvalue-object)
	    - [Get](#get)
	    - [Get Like](#get-like)
	- [Updating data](#updating-data)
        - [Set](#set)
        - [Set KeyValue Object](#set-keyvalue-object)
	- [Inserting data](#inserting-data)
	- [Removing data](#removing-data)
	- [Size, Clear, IsEmpty](#size,-clear,-isempty)
        - [Size](#size)
        - [Clear](#clear)
        - [IsEmpty](#isempty)
    - [Saving collection](#saving-collection)
    - [Iterating collection](#iterating-collection)
- [How it works](#how-it-works)
- [Contributing](#contributing)
- [Support](#support)
- [License](#license)

## Installation

Package name is KeyValueDB.

Using Package-Manager:

```bash
Install-Package KeyValueDB -Version 1.2.0
```

Using .NET CLI:

```bash
dotnet add package KeyValueDB --version 1.2.0
```

Using Pakage Reference:

```xml
<PackageReference Include="KeyValueDB" Version="1.2.0" />
```

Using Packet CLI:

```xml
paket add KeyValueDB --version 1.2.0
```

## Example

The following example load, update, read and remove a simple key value object 

```csharp
using System;
using Io.Github.Thecarisma;

namespace Sample
{
    class Test 
    {
        public static void Main(string[] args)
        {
            //initialize the key-value 
            KeyValueDB keyValueDB = new KeyValueDB("Greet=Hello World,Project=KeyValueDB", true, '=', ',', false);

            //get an object
            Console.WriteLine(keyValueDB.Get("Greet"));

            //remove an object
            keyValueDB.Remove("Greet");

            //add an object
            keyValueDB.Add("What", "i don't know what to write here");

            //print all the objects
            foreach (var kvo in keyValueDB) {
                Console.WriteLine(kvo);
            }
        }
    }
}

```

## Legends

```
kvp  - Key Value Pair
kvdb - Key value Database
pss  - Possibly
kvo  - Key Value Object
```

## API

Only string type is used as the key and value of the kvo. A kvo can be used to replace or set the value for a key.

### Creating/loading a document

You can use the package to update and create an existing key value database. This library does not read the database from a file which means you have to find a way to read a string from the file. 

Create a new keyValueDB. The default seperator between the key and value is `=` and the delimeter between the kvp is `\n`(newline).

```csharp
KeyValueDB keyValueDB = new KeyValueDB();
```

To load existing KeyValueDB  

```csharp
KeyValueDB keyValueDB = new KeyValueDB(
        "Greet=Hello World,Project=KeyValueDB", //pss read string from file
        true, //case sensitive is true
        '=', //the seperator from key and value
        ',', //the delimeter for the key-value-pair
        false //error tolerance if true no exception is thrown
        );
```

### Inserting Data

The only accepted type that can be inserted is a valid `KeyValueObject` and `String`. The method `Add` can be used to add a new kvp into the object.

Add a kvp with it key and value

```csharp
keyValueDB.Add("Greet", "Hello World");
```

Add a kvp using the `KeyValueObject` class.

```csharp
KeyValueObject keyValueObject = new KeyValueObject("Greet", "Hello World");
keyValueDB.Add(keyValueObject);
```

### Finding Data

There are several way to find and get a value from the kvdb object. The value or the KeyValueObject can be gotten using the methods below

#### Get KeyValue Object

You can get the kvo using either the key or index. If the corresponding kvo is not found, an empty kvo is added to the db and then returned but not in the case when requested with the integer index. If a fallback kvo is sent as second parameter then when the request kvo is not found the fallback second parameter is added to the kvdb and then returned.

Get the kvo using it integer index

```csharp
keyValueDB.GetKeyValueObject(0);
//Io.Github.Thecarisma.KeyValueObject@55915408:Key=Greet,Value=Hello World
```

Get the kvo using it key 

```csharp
keyValueDB.GetKeyValueObject("Greet");
//Io.Github.Thecarisma.KeyValueObject@55915408:Key=Greet,Value=Hello World
```

Get the kvo using it key with fallback kvo

```csharp
KeyValueObject keyValueObject = new KeyValueObject("Name", "Adewale Azeez");
keyValueDB.GetKeyValueObject("Name", keyValueObject);
//Io.Github.Thecarisma.KeyValueObject@55915408:Key=Name,Value=Adewale Azeez
```

#### Get Like KeyValue Object

Get a kvo by checking the kvdb for the kvo object that contains a part of the key. If a fallback kvo is sent as second parameter then when the request kvo is not found the fallback second parameter is added to the kvdb and then returned.

Get a similar kvo using it key part 

```csharp
keyValueDB.GetLikeKeyValueObject("eet");
//Io.Github.Thecarisma.KeyValueObject@55915408:Key=Greet,Value=Hello World
```

Get a similar kvo using it key part with fallback kvo

```csharp
KeyValueObject keyValueObject = new KeyValueObject("Name", "Adewale Azeez");
keyValueDB.getKeyValueObject("Nam", keyValueObject);
//Io.Github.Thecarisma.KeyValueObject@55915408:Key=Name,Value=Adewale Azeez
```

#### Get

You can get a kvdb value using either the key or index. If the corresponding value is not found, an empty string is added to the db and then returned but not in the case when requested with the integer index. 

If a fallback kvo is sent as second parameter then when the request key is not found the fallback second parameter is added to the kvdb and then value is returned. If a string value is sent as the second value it is returned if the key is not found in the kvdb.

Get a value using it integer index

```csharp
keyValueDB.Get(0);
//"Hello World"
```

Get the value using it key 

```csharp
keyValueDB.Get("Greet");
//"Hello World"
```

Get the kvo using it key with fallback value

```csharp
keyValueDB.Get("Licence", "The MIT Licence");
//"The MIT Licence"
```

Get the kvo using it key with fallback kvo

```csharp
KeyValueObject keyValueObject = new KeyValueObject("Licence", "The MIT Licence");
keyValueDB.Get("Name", keyValueObject);
//"The MIT Licence"
```

#### Get Like 

Get a value by checking the kvdb for the kvo object that contains a part of the key. 

If a fallback kvo is sent as second parameter then when the request key is not found the fallback second parameter is added to the kvdb and then value is returned.

Get a value using it key part 

```csharp
keyValueDB.GetLike("eet");
//"Hello World"
```

Get a value using it key part with fallback kvo

```csharp
KeyValueObject keyValueObject = new KeyValueObject("Licence", "The MIT Licence");
keyValueDB.GetLike("Li", keyValueObject);
//"The MIT Licence"
```

### Updating Data

There are various way to update a kvp in the kvdb, the value can be changed directly or set to a new KeyValueObject. If you try to set a kvo that does not exist in the kvdb using it key, it is added to the kvdb.

#### Set

The `Set` method is used to change the value of the kvo using the index of the kvo or a kvo key. 

Set a kvo value using it index

```csharp
keyValueDB.Set(0, "Hello World from thecarisma");
//Io.Github.Thecarisma.KeyValueObject@55915408:Key=Greet,Value=Hello World from thecarisma
```

Set a kvo value using it key

```csharp
keyValueDB.Set("Greet", "Hello World from thecarisma");
//Io.Github.Thecarisma.KeyValueObject@55915408:Key=Greet,Value=Hello World from thecarisma
```

#### Set KeyValue Object

Completely change a KeyValueObject in the kvdb using either it index or it key. The kvo is completly replaced which means unique fields like the hashcode of the kvo changes. When the kvo is set using it key if the corresponding kvo does not exist it is added into the kvdb.
Note that this method completly changes the kvo so it can be used to replace a kvo.

Set a kvo using it index

```csharp
KeyValueObject keyValueObject = new KeyValueObject("Licence", "The MIT Licence");
keyValueDB.SetKeyValueObject(0, keyValueObject);
//Io.Github.Thecarisma.KeyValueObject@55915408:Key=Licence,Value=The MIT Licence
```

Set a kvo value using it key

```csharp
KeyValueObject keyValueObject = new KeyValueObject("Licence", "The MIT Licence");
keyValueDB.SetKeyValueObject("Greet", keyValueObject);
//Io.Github.Thecarisma.KeyValueObject@55915408:Key=Licence,Value=The MIT Licence
```

### Inserting Data

A new kvp can be inserted by invoking the `Add` method. The kvp can be added using it key and value or by directly adding the KeyValueObject to the kvdb. 

Add a new kvp using the key and value

```csharp
keyValueDB.Add("Key", "This is the value");
```

Add a new kvp using a new KeyValueObject

```csharp
KeyValueObject keyValueObject = new KeyValueObject("Key", "This is the value");
keyValueDB.Add(keyValueObject);
```

### Removing Data

Remove a kvp completely from the kvdb using either it key of the integer index. The kvp that was removed is returned from the method. If the index does not exist out of bound error occur and if a kvo with the key is not present nothing is done but an empty kvo is returned.

Remove a kvp using integer index

```csharp
keyValueDB.Remove(0);
//removes the first kvp in the kvdb
//Io.Github.Thecarisma.KeyValueObject@55915408:Key=Greet,Value=Hello World
```

Remove a kvp using it key

```csharp
keyValueDB.Remove("Greet");
//removes the first kvp in the kvdb
//Io.Github.Thecarisma.KeyValueObject@55915408:Key=Greet,Value=Hello World
```

## Size, Clear, isEmpty

### Size

Get the size of the kvo in the kvdb.

```csharp
keyValueDB.Size();
//4
```

### Clear

Remove all the elements and kvo from the kvdb

```csharp
keyValueDB.Clear();
//keyValueDB.Size() = 0
```

### isEmpty

Check whether the kvdb contains any kvo in it.

```csharp
keyValueDB.IsEmpty();
//False
```

## Saving collection

The kvp collection kvdb can be inspected as a string using the `ToString` method. The returned value can be saved locally by writing to a persistent storage or to a plain text file. The output of the `ToString` method is determined by the kvos, the seperator and the delimeter.

```csharp
keyValueDB.ToString();
// "Greet=Hello World,Project=KeyValueDB,Project=KeyValueDB,Licence=The MIT Licence"
```

## Iterating collection

The KeyValueDB object can be iterated natively using the `foreach..in` loop expression. 

```csharp
foreach (var kvo in keyValueDB) {
    //operate on the KeyValueObject
}
```

## How it works

KeyValueObject class contains the key and value field and the fields setter and getter. 
The KeyValueObject is the main internal type used in the KeyValueDB class.
 
In KeyValueDB the key value pair is stored in `List<KeyValueObject>` type, all search, 
updating and removal is done on the `keyValueObjects` in the class. The string sent as 
first parameter if parsed into valid key value using the separator and delimiter fields. The 
`ToString` method also parse the `keyValueObjects` content into a valid string with regards to the 
separator and delimeter. 

## Contributing

Before you begin contribution please read the contribution guide at [CONTRIBUTING GUIDE](https://keyvaluedb.github.io/contributing.html)

You can open issue or file a request that only address problems in this implementation on this repo, if the issue address the concepts of the package then create an issue or rfc [here](https://github.com/keyvaluedb/keyvaluedb.github.io/)

## Support

You can support some of this community as they make big impact in the developement of people to get started with software engineering.

- https://www.patreon.com/devcareer

## License

MIT License Copyright (c) 2020 Adewale Azeez - keyvaluedb

