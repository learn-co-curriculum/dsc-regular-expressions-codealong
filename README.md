
# Regular Expressions - Codealong

## Introduction

In this lab, we'll make use of some common regex patterns to search through text. 

## Objectives

You will be able to:

* Identify common use cases where regular expressions are useful
* Understand and explain basic pattern matching with regular expressions
* Create regex patterns to filter through text

## Getting Started

In this lab, we're going to explore some of the more common operations used to filter text and match patterns with regular expressions, or _regex_ for short. 

For our source material, we'll be searching through the file `restaurant.txt`.  This is a sample file containing a menu from a fictional restaurant. 

## Using Regex Testers

Working with regex patterns is an iterative process--it's quite rare that we'll write the pattern that does exactly what we want while capturing all edge cases the first time through. To make matters simpler, most developers and data scientists make use of popular regex tester websites, which allow you to put in a block of text and and test your patterns by quickly seeing visually how what a pattern will grab out of the the text block. During this lab, we strongly recommend you take some time to use a regex tester website such as [Regex Pal](https://www.regexpal.com/) or [regexr](https://regexr.com/) so that you can visually inspect how changing your regex pattern affects your results when working towards a correct answer! 

## Regex Cheat Sheet

When learning regex, the symbols and notation for patterns can seem a bit intimidating. Don't worry about trying to memorize these symbols and patterns--it's easier to just keep a cheat sheet handy and look them up as needed. Instead, focus on understanding the basic structure of regex patterns, and then look up the syntax needed to build your pattern.  

For reference, we've embedded a common Regex Cheat Sheet found online. Core regex syntax tends to be the same across all languages, so you don't have to worry too much about language-specific features for regex, or looking up regex for the wrong language. Although some languages may add in some special bells and whistles for regex, the core functionality and syntax for regex is generally the same across the board. 

<img src='regex_cheat_sheet.png'>


### The Data

For this lab, we'll be working with a plain text file in this repo called `menu.txt`. Our goal will be to use regex to scrape various types of information from this file.

Let's get started!

We'll begin by importing everything we'll need for this lab, as well as loading in the file itself. 

Run the cell below to do this now. 


```python
import re

with open('menu.txt', 'r') as f:
    file = f.read()

print(file)
```

### Creating a Basic Pattern With Character Classes

We'll start by creating a basic pattern, and then using the `re` library to search through the file for any instances that match this pattern. 

The most simple types of patterns we can use are character classes. These allow us to match predefined things such as words or digits. Let's quickly try getting all the digits. 

Run the cell below to find all the digits in the document. 


```python
pattern = '\d'
p = re.compile(pattern)
digits = p.findall(file)
digits
```

It worked, but not in the way we might have expected. This is because the pattern has found every individual number as an individual match. If you look at the order of the numbers and compare it to the menu text above, you notice that they are in the order you'd see if you read them from left to right starting at the top of the menu. 

Let's try modifying our pattern, so that it only gets numbers with a dollar sign in front of them. 

#### Escaping Metacharacters

In regex, the dollar sign is a **_Metacharacter_**. This character means something. We can't just use the dollarsign in a pattern, anymore that we can use a reserved keyword like `for` as a varaible name in python. Since the dollar sign is a reserved symbol in regex, we'll need to escape it using a `\`.  This tells the regex compiler that we are talking about the actual dollar sign symbol, not using it to talk about the end of a string. 
In the cell below:

* Modify the pattern so that it includes a dollar sign, followed by a digit character class. 
* Compile the new pattern. 
* Use the compiled pattern's `.findall()` method on the pattern. 
* Display the results

**_NOTE:_** Make sure there is no space between the dollar sign and the digit character class, since it will treat the space as a character that is a part of the pattern! 


```python
pattern = '\$\d'
p = re.compile(pattern)
digits = p.findall(file)
digits
```

That's better, but still not perfect! Now, the pattern only gets the first digit for prices. For instance, the pattern truncates `$10` to `$1`.

To fix this, we could also get the next 3 characters that follow a match by adding `.{3}` to the end of pattern. Let's try this in the cell below.  


```python
pattern = '\$\d.{3}'
p = re.compile(pattern)
digits = p.findall(file)
digits
```

This seems to have worked for some, but not all. It also left out any prices have less than 3 characters after the initial match, such as \$10. 


We need to crete a pattern that gets all the numbers in a price. To do this, we'll make use of groups and ranges!

### Using Groups, Ranges, and Quantifiers

Groups and ranges allow us to create more complex patterns by grouping them together. In these groups, we can also provide rangers, to make our patterns less verbose without making them any less powerful. If we want to get any lowercase letters, we could create a group that contains a letter range like `[a-z]`. If we wanted to get all letters regardless of case, we could use the range `[a-zA-Z]`. We could even include things like numbers in here. If wanted to create a grouping that matches any numbers 0-9, or any uppercase or lowercase letters between a and z, we could use `[a-zA-z0-9]`. 

Run this pattern in the cell below, and see what it matches. 


```python
pattern = '[a-zA-Z0-9]'
p = re.compile(pattern)
digits = p.findall(file)
digits
```




    ['F',
     'l',
     'a',
     't',
     'i',
     'r',
     'o',
     'n',
     'S',
     'c',
     'h',
     'o',
     'o',
     'l',
     'C',
     'a',
     'f',
     'e',
     'M',
     'e',
     'n',
     'u',
     'A',
     'p',
     'p',
     'e',
     't',
     'i',
     'z',
     'e',
     'r',
     's',
     'N',
     'a',
     'c',
     'h',
     'o',
     's',
     '1',
     '0',
     'C',
     'a',
     'l',
     'a',
     'm',
     'a',
     'r',
     'i',
     '1',
     '2',
     '3',
     'C',
     'h',
     'e',
     'e',
     's',
     'e',
     'P',
     'l',
     'a',
     't',
     't',
     'e',
     'r',
     '8',
     '7',
     '5',
     'E',
     'n',
     't',
     'r',
     'e',
     'e',
     's',
     'C',
     'h',
     'i',
     'c',
     'k',
     'e',
     'n',
     'S',
     'a',
     'n',
     'd',
     'w',
     'i',
     'c',
     'h',
     '1',
     '6',
     '9',
     '5',
     'A',
     'f',
     'r',
     'i',
     'e',
     'd',
     'c',
     'h',
     'i',
     'c',
     'k',
     'e',
     'n',
     's',
     'a',
     'n',
     'd',
     'w',
     'i',
     'c',
     'h',
     'w',
     'i',
     't',
     'h',
     'l',
     'e',
     't',
     't',
     'u',
     'c',
     'e',
     't',
     'o',
     'm',
     'a',
     't',
     'o',
     'a',
     'n',
     'd',
     'm',
     'a',
     'y',
     'o',
     'A',
     'd',
     'd',
     'c',
     'h',
     'e',
     'e',
     's',
     'e',
     'f',
     'o',
     'r',
     '1',
     '5',
     '0',
     'F',
     'l',
     'a',
     't',
     'i',
     'r',
     'o',
     'n',
     'S',
     't',
     'e',
     'a',
     'k',
     '2',
     '2',
     'A',
     'p',
     'r',
     'i',
     'm',
     'e',
     'c',
     'u',
     't',
     'o',
     'f',
     'F',
     'l',
     'a',
     't',
     'i',
     'r',
     'o',
     'n',
     'S',
     't',
     'e',
     'a',
     'k',
     'c',
     'o',
     'o',
     'k',
     'e',
     'd',
     't',
     'o',
     'y',
     'o',
     'u',
     'r',
     'l',
     'i',
     'k',
     'i',
     'n',
     'g',
     'C',
     'o',
     'm',
     'e',
     's',
     'w',
     'i',
     't',
     'h',
     'a',
     's',
     'i',
     'd',
     'e',
     'o',
     'f',
     'v',
     'e',
     'g',
     'e',
     't',
     'a',
     'b',
     'l',
     'e',
     's',
     'A',
     'd',
     'd',
     'a',
     's',
     'a',
     'l',
     'a',
     'd',
     'o',
     'r',
     'c',
     'u',
     'p',
     'o',
     'f',
     's',
     'o',
     'u',
     'p',
     'f',
     'o',
     'r',
     '3',
     'G',
     'a',
     'r',
     'd',
     'e',
     'n',
     'S',
     'a',
     'l',
     'a',
     'd',
     '1',
     '4',
     'A',
     's',
     'a',
     'l',
     'a',
     'd',
     'w',
     'i',
     't',
     'h',
     's',
     't',
     'u',
     'f',
     'f',
     'f',
     'r',
     'o',
     'm',
     't',
     'h',
     'e',
     'g',
     'a',
     'r',
     'd',
     'e',
     'n',
     'o',
     'n',
     'o',
     'u',
     'r',
     'r',
     'o',
     'o',
     'f',
     '3',
     't',
     'y',
     'p',
     'e',
     's',
     'o',
     'f',
     'd',
     'r',
     'e',
     's',
     's',
     'i',
     'n',
     'g',
     'a',
     'v',
     'a',
     'i',
     'l',
     'a',
     'b',
     'l',
     'e',
     'W',
     'a',
     'n',
     't',
     't',
     'o',
     'p',
     'l',
     'a',
     'c',
     'e',
     'a',
     'n',
     'o',
     'r',
     'd',
     'e',
     'r',
     'f',
     'o',
     'r',
     'd',
     'e',
     'l',
     'i',
     'v',
     'e',
     'r',
     'y',
     'C',
     'a',
     'l',
     'l',
     'u',
     's',
     'a',
     't',
     '5',
     '5',
     '5',
     '1',
     '2',
     '3',
     '8',
     '4',
     '5',
     '2']



Now, let's use some ranges, groupings, and quantifiers to match on any price in the menu. Write this pattern now in the cell below. Be sure to include the dollar signs in the pattern to match!

**_NOTE_**: This one will probably take you a little while to figure out. When writing this pattern, take some time to study all the potential cases we want to match inside the menu. Also, don't forget to escape metacharacters such as dollar signs and decimal points!


```python
pattern = '(\$\d+\.?\d*)'
p = re.compile(pattern)
digits = p.findall(file)
digits
```




    ['$10', '$12', '$8.75', '$16.95', '$1.50', '$22', '$3', '$14']



### Putting It All Together

To end this lab, we'll have you put your newfound regex skills to the test to see if you can write a pattern that gets the phone number from the bottom of the menu. How you match it is up to you, just as long as you get the phone number! It's okay if the dashes and parentheses are included in your match, but not the exclamation point at the end. 

In the cell below, write a regex pattern to match the phone number from the menu, and confirm that it works by compiling it and running in it. 


```python
pattern = '(\(\d{3}\) (\d{3}-\d{4}))'
p = re.compile(pattern)
digits = p.findall(file)
digits
```




    [('(555) 123-8452', '123-8452')]



## Summary

In this lab, we got some practice in using regex to filter text!
