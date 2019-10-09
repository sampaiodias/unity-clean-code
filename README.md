# Unity Clean Code

This repository is dedicated to teach Unity developers of different backgrounds and programming skill levels to create cleaner code, which in most cases is also more maintainable and sometimes even faster! If you have any suggestions or found any errors below, be sure to submit an Issue or a Pull Request.

This guide will not cover everything there is to learn about clean code for Unity, but only the most important bits in a concise manner. If you want to learn more, I personally suggest reading the [C# programming guide](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/) when in doubt about a specific C# feature, and/or the book "*Clean Code: A Handbook of Agile Software Craftsmanship*" by Robert C. Martin for a broader understanding of the topic.

One of the most covered topics in this guide is naming conventions for C#. Keep in mind, however, that naming conventions are often tweaked **on purpose** by organizations for various reasons (Unity and Microsoft included). If you have a good reason to not follow them, do it! Just remember that if a project is using a different style convention, use it on the entire project.

- [The Basics](#the-basics)
- [Identation](#identation)
- [Variables](#variables)
- [Methods](#methods)
- [Statements](#statements)
- [Namespaces](#namespaces)
- [Comments](#comments)
- [Automated Tests](#automated-tests)
- [Advanced Tips and Patterns](#advanced-tips-and-patterns)
- [References](#references)

## The Basics
If you're reading this guide, you probably have a general understanding of what C# is, how to write (at least) some simple scripts in Unity and what not. However, I often see programmers struggle to truly understand the first principles they ever see when creating a new script. Let's take a look at it:

```csharp
using UnityEngine;
using System.Collections;

public class MyCustomScript : MonoBehaviour {

	// Use this for initialization
	void Start () {
	
	}
	
	// Update is called once per frame
	void Update () {
	
	}
}
```

The code above is the basic template of a new C# script in Unity. We can divide it in a few portions.

```csharp
using UnityEngine;
using System.Collections;
```

The first lines of a script are dedicated to "importing" the necessary code libraries to make our code work. Think of it as dependencies: you can't use a feature of the UnityEngine library if you don't explicitly declare you are *using* them. Make sure you leave them at the very first lines of your script.

```csharp
public class MyCustomScript 
```

The script file we created defines a class (an abstraction of an object), that is public (accessible by other pieces of code), and with the name of the file you typed in. In this case, the class is called *MyCustomScript*. This is a **horrible** name! There is no way we can understand what this class does just by looking at its name (which should be your goal). Use meaninful names, like "PlayerHealth" or "Projectile", **always** in Pascal Case.

```csharp
                            : MonoBehaviour {

	// Use this for initialization
	void Start () {
	
	}
	
	// Update is called once per frame
	void Update () {
	
	}
}
```

Starting at the very top (after public class MyCustomScript), we define what class (if any) our own class inherits from. In this case, it is Unity's MonoBehaviour. This is arguably the most important class in game development for Unity now, as it's what "marks" a class to be a component that can be attached to a GameObject. Inheriting from another class is not just marking it, it is what "enables you to create new classes that reuse, extend, and modify the behaviour that is defined in other classes". The methods Start and Update, for example, is something that Unity will only call during their predefined events if your class *is* a MonoBehaviour, which you will then extend the methods you need with the proper funcionality.

One common mistake to begginers is thinking that everything has to be a MonoBehaviour or inherit from something else. That is very far from the true, and actually a bad practice. For example, if you have a concept or abstraction that only handles data, consider making it a class that doesn't inherit from another class.

## Identation

At first glance, identation is a simple topic: managing *spaces* inside your code. It is what makes your code more readable by separating different words or symbols with a space, tab, or new line. Understanding the importance of a code with the correct identation is often hard to developers who are new to programming, and the best way I ever found to change that was to show a piece of code with really bad identations. Let's take our script template to a new level (of uglyness):

```csharp
// The code below is an example of *bad* identation.

using UnityEngine;   using System.Collections;
public class MyCustomScript    :MonoBehaviour{
//Use this for initialization
void Start() {
}
// Update is called once per frame
    void Update ( ) { } 
}
```

How hard it is for you to read the code above? If you're used to programming in Unity maybe not very much, but it is already more time-consuming to understand what this script does and where each part of the could should be. If we took our approach to a real script, with 50 or 100 lines or code, readability would be absolutely awful.

Many programming languages (C# included) have suggested guidelines or rules on how to properly ident your code, but truly learning it is not an easy task for most people. Luckily for us, there is a tool that will makes this process much easier: auto-formatting. If you're not sure you can handle identation by yourself, let the computer do the job for you and learn it by example. As you're probably using Visual Studio to code, my suggestion is to install the '*Productivity Power Tools*' plugin for Visual Studio and have the "Format document on save" option enabled. If you don't want to (or can't) install it, you can always use the "*Ctrl+K, Ctrl+D*" shortcut to format the currently opened script.

## Variables

As you probably know, variables (or members, technically speaking) are what stores the data of your application during runtime. Because your game probably has lots of data, having meaningful and readable names for them is vital. We will also talk about serialization of variables here, a topic of special importance in Unity.

### Naming

This is a tricky topic within the community. While certain languages have very strict naming conventions, C# is a little bit "relaxed" within a few cases, leaving the convention open for the organization to decide. With this in mind, what you will see below is a mix of what is the C# guidelines for naming variables (including fields and properties) and what is very common and/or widely adopted.

- Use meaningful names
```csharp
// Do
public int healthAmount;
public string teamName;
// Do NOT
public int hp;
public string tName;
```

- Use readable names
```csharp
// Do
public int movementSpeed;
// Do NOT
public int mvmtSpeed;
```

- Use nouns as names for variables
```csharp
// Do
public int movementSpeed;
// Do NOT
public int getMovementSpeed;
```

- Use the correct "casing" for the kind of variable
```csharp
public int movementSpeed; // Public variable, Camel Case
private int _movementSpeed; // Private variable, Camel Case with optional '_' at the start
public int Movement Speed { get; set; } // Property, Pascal Case
private const int MovementSpeed = 10; // Constant, Pascal Case
```

-  Avoid using abbreviations or single characters (unless it's math-related)
```csharp
// Do
public string groupName;
// Do NOT
public int grpName;

// Recommended for math-related scripts, like Vector2
public int x;
public int y;
```

- Explicitly use the 'private' keyword
```csharp
// Do
private bool _isJumping;
// Do NOT
bool _isJumping;
```

- Use the 'var' keyword when the second part of the variable attribution clearly reveals the type. Only for variables declared inside a method or scope (local variable)
```csharp
// Do
var players = new List<Players>();
// Do NOT
var players = PlayerManager.GetPlayers();
```

### Serialization

When you create a C# script using Unity and let the class inherit from MonoBehaviour, values that are "public" to the engine can be edited using the Inspector window. After doing it and saving the scene, these values are **serialized** (or saved) into the scene file.  As the goal of this is not to say when to use or not use serialized variables, let's focus on *how* you can declare a variable to be serialized:

```csharp
// Serialized
public int movementSpeed;
public int movementSpeed = 10;
[SerializeField] private int _movementSpeed;
[SerializeField] private int _movementSpeed = 10;

// NOT Serialized
private int _movementSpeed;
private int _movementSpeed = 10;
public int MovementSpeed { get; set; }
```

As a side note, attributes like the [SerializeField] can also be placed to the line above the declaration to improve readability, like this:

```csharp
[SerializeField]
private int _movementSpeed;
[SerializeField, AnotherCoolAttribute]
private bool _isEnemy;
```

## Methods

If you think about it, methods are the core part of a software: they **do** something. Because of their nature, when you create a method you should always name them using a verb. Don't forget what you learned above about naming variables: use descriptive, meaningful and readable names. Also remember to name methods using Pascal Case.

```csharp
// Do
public void SetInitialScore()
{

}

// Do NOT
public void InitialScore()
{

}

public void setInitialScore()
{

}

public void SetInitScr()
{

}
```

A key feature of methods are parameters. When creating them, use Camel Case and avoid prefixes.

``` csharp
// Do
public bool IsNewHighScore(int currentScore) 
{

}

// Do NOT
public bool IsNewHighScore(int CurrentScore) 
{

}

public bool IsNewHighScore(int _currentScore) 
{

}
```

Keep the structure of your methods clear with a low amount of information inside it. If your method contains more than 10 lines of code, it is probably a good candidate to be split into two or more methods. And finally, returning to the identation topic, follow the structure below for methods:

```csharp
public void DoSomething()
{ // Braces on a new line, starting at the same position of the method declaration.
	// Method content is a 'Tab' to the right after the braces.
	string somethingCool = "Cool";
	DoSomethingElse(somethingCool);
}
```

## Statements

Statements, as defined by Microsoft, are the "actions that a program takes", like declaring variables, calling methods, and looping through collections. We can divide statements into two categories: single-line, and multi-line. 

```csharp
private void DoSomething()
{
	// Single-line statement. In this case, declaring and initializing a variable;
	var randomNumbers = new int[3];

	// Multi-line statement. In this case, looping through 'randomNumbers'
	foreach (int number in randomNumbers)
	{
		DoSomethingElse(number);
	}
}
```

For single-line statements there's not much we can actually talk about aside from keeping every statement in a separate line, having lines of code that are not too long, and other small tips. But there's one exception: line breaks. Use them when having to divide a long statement seems awkward or troublesome.

```csharp
// Do

Debug.Log("This is just an example log that will print a long text with the values of the script: "
	+ playerName + " " + playerHealth);

string logMessageForPlayersInformation = 
	PlayerManager.GetPlayerInformation(GetPlayerIndex("Player1")) +
	PlayerManager.GetPlayerInformation(GetPlayerIndex("Player2"));
Debug.Log(logMessageForPlayersInformation);

// Do NOT

// Too long
Debug.Log("This is just an example log that will print a long text with the values of the script: " + playerName + " " + playerHealth); 

string logMessageForPlayersInformation = 
	PlayerManager.GetPlayerInformation(
		GetPlayerIndex("Player1")) +
	PlayerManager.GetPlayerInformation(
		GetPlayerIndex("Player2"));
Debug.Log(logMessageForPlayersInformation);
```

The complexity actually steps in when it comes to multi-line statements. When you decide to create blocks of code inside of another block, your script will face a problem similar to the issue presented above. Take a look at this example:

```csharp
var someValue = 100;
var myValues = new int[10, 5]
for (int i = 0; i < 10; i++)
{
	for (int j = 0; j < 5; j++)
	{
		if (i >= 1)
		{
			someValue--;
		}
		else
		{
			while (someValue > 50)
			{
				someValue -= 10;
			}
		}
		myValues[i, j] = someValue;
	}
}
```

The code above is not meant to do something useful in particular, but if you were given the task to understand what this algorithm does, that would be a hard job. The amount of complexity added to the code because of these **nested** statements make their readability and maintainablity really low. And trust me, I've seen worse examples of pieces of code that actually shipped to a real game. As a general rule of thumb, keep your methods with a maximum of one multi-line statement nested inside another multi-line statement. When possible, no nesting or even no multi-line statements at all is desirable.

```csharp
// Goal
var someValue = 100;
var myValues = new int[10, 5]
for (int i = 0; i < 10; i++)
{
	for (int j = 0; j < 5; j++)
	{
		DoSomething(someValue, i, j);
	}
}

// Ultimate Goal
int[,] myValues = CalculateValueMatrix(100, 10, 5);
```

## Namespaces

Maybe one of the most useful features of C#, namespaces are (in short) a way to better organize your scripts. Unfortunately, namespaces were also one the most underused C# features by Unity developers for quite some time (maybe because of Unity's very own scripting documentation not showing it properly), but that's something I'm personally seeing some changes within the community. Namespaces are not only useful, they are powerful, but only when you properly use them. And the most important part about it is... naming them!

Before we go to what you should or shouldn't do with namespaces, take a look at this familiar piece of code:

```csharp
using UnityEngine;
using System.Collections;
```

Every new MonoBehaviour is *using* these two namespaces: UnityEngine and System.Collections. The first one is provided by Unity and contains a lot of scripts, like the MonoBehaviour class. The second one is provided by Microsoft and and also contains many interfaces and classes that define various collections of objects, such as lists and dictionaries. One important part of namespaces is that you can create namespaces inside another namespace. For example, Collections is actually nested inside of the System namespace.

Now that these concepts are behind us, let's take a look at how to create them and how to name them!

```csharp
using System;

namespace Company.Product.Feature
{
	public class Example
	{
	
	}
}
```

After declaring what other namespaces this file is using, we create a namespace and encapsulate all other code inside the scope of this namespace. The naming for it should start with the organization name and then the name of the product (for example: Github.ExampleProduct). All names are in Pascal Case, with dots separating the nested hierarchy. You may or may not continue with the nesting, using now the name of a specific feature of your product (for example: Github.ExampleProduct.Database). All names should be in singular, but consider using plural when it makes the name of the namespace better explain what it contains (like Collections, of System.Collections). Finally, don't use prefixes or other symbols, like underscores.

## Comments

With the honorable objective of helping humans understand the code, comments are an incredible way of documentating your scripts and improving maintainability. They are easy to use, but also easy to use in excess. To increase the quality of your code, here are the simple guidelines to when you should create a comment:

1) To document a class, method, enum, or struct.
2) To serve as a header of the file (mainly for a copyright notice).
3) To explain a statement that is inherently complex or not obvious to understand.

Let's explore each of these three situations, starting with first one. If you type */* (dash symbol) three times on the line above of one of the structures mentioned above, a special comment section will show up.

```csharp
/// <summary>
/// 
/// </summary>
public class Example
{

}
```

This is not a convention to just make your comments pretty or easier to read. This is actually a standard to help automated tools (like your best friend IntelliSense) to parse your comments and generate useful content. Because of this, be sure to use this pattern to document your code.

```csharp
// Do

/// <summary>
/// Calculates the total area of a circle.
/// </summary>
/// <param name="radius">The radius of the circle</param>
private float CalculateCircleArea(float radius)
{
    return 3.14f * radius * radius;
}

//Do NOT

// Calculates the total area of a circle, given a certain radius.
private float CalculateCircleArea(float radius)
{
    return 3.14f * radius * radius;
}
```

For the second situation there are not many standards, you are free to type int almost any way you need, as long as it starts in the very first line of the document. Here's an example:

```csharp
// --------------------------------------------------------------------------------------------------------------------
// <copyright file="Script.cs" company="{Company}">
//
// Copyright (C) 2019 {Company}
//
// This program is free software: you can redistribute it and/or modify
// it under the +terms of the GNU General Public License as published by
// the Free Software Foundation, either version 3 of the License, or
// (at your option) any later version.
//
// This program is distributed in the hope that it will be useful,
// but WITHOUT ANY WARRANTY; without even the implied warranty of
// MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
// GNU General Public License for more details.
//
// You should have received a copy of the GNU General Public License
// along with this program.  If not, see http://www.gnu.org/licenses/. 
// </copyright>
// <summary>
// {one line to give the program's name and a brief idea of what it does.}
// 
// Email: {Email}
// </summary>
// --------------------------------------------------------------------------------------------------------------------

// Rest of the code goes here
```

The last situation is where inexperienced programmers "abuse" comments, putting them on a huge number of statements. This is unnecessary, and it actually does more harm than good! Creating comments should be for special occasions, and not the norm. It is hard to say exactly when you should use them, so follow the example below (where the comments are completely useless) so you don't make the same mistakes.

```csharp
// Do NOT! Once again, this is an example of what you should NOT do!

private void CreateEnemy(GameObject prefab)
{
	// The GameObject of the enemy to be instantiated;
	GameObject enemy;
	// Creates the enemy in the scene
	enemy = Instantiate(prefab)
	
	enemy.GetComponent<Enemy>().InitiateMovementBehaviour(); // Makes the enemy walk around the world
}
```

## Automated Tests

## Advanced Tips and Patterns

## References
- "*Clean Code: A Handbook of Agile Software Craftsmanship*" by Robert C. Martin
- "*Game Programming Patterns*" by Robert Nystrom
- C# Documentation by Microsoft
