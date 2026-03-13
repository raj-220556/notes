---
title: PHP Course
description: 
author: z3ncrypt3dBu9
date: 2026-01-24 11:33:00 +0800
categories: [01_Knowledge-Base, 01_Technologies]
tags: [Web2, PHP, Course, Technologies, NotCompleted!!!!]
---

# PHP Course

## Table Of Content
- [Introduction](#introduction)


## Introduction
---

### Defination 

PHP is a **popular general-purpose scripting language** that is especially suited to web development. Fast, flexible and pragmatic, PHP powers everything from your blog to the most popular websites in the world.
- It is used to Dynamic web pages.
- It runs on a **Server**(not in a Browser).
- 70% of Websites still using PHP

PHP is a server-side scripting language embedded in HTML in its simplest from.  PHP allows web developers to create dynamic content and **interact with databases**. PHP is known for its **simpilicity**, **speed**, and **flexibility** - features that have made it a cornerstone in a web development world.
- It was realsed in 1995, Desigined by **Rasmus Lerdorf**.
- PHP is an acronym: **Personal Home Page**.
- later changed into PHP = **PHP HyperText Preprocessor**.

### How Does PHP work?
![How php works](/posts/01_Knowledge-Base/phpBrocode/phpworks.png){: width="972" height="589" }

### Syntax
Along with HTML: 
```html
<html>  
    <h2> Hello World </h2>
</html>
<?php
    $food = "pizza";
    echo "I Like {$food}";
?>
```

### Prerequistes
- HTML, MYSQL

### Requirements
First of all You need a Laptop 😅
- TextEditor(VScode)
  - change in settings `"php.validate.executablePath": "/opt/lampp/bin/php"`
  - install extensions `live server`, `PHP server`, `PHP Intelephense`.
- [XAMPP](https://apachefriends.org) for a Server
  - in **Linux**: `/opt/lampp` (default server folder)
  - add folder for our webpages inside `/opt/lampp/htdocs`

  - To start the server `/opt/lampp/lampp start`
  - To conect Server http://localhost/foldername

## Varibales
```php
<?php

    // Print statement
    echo "I Like a Pizza " . "<br>";
    echo "I LOVE hacking <br>";

    /* This is
        Multiline Comment
    */


    // Variable = a reuseable container that holds data 
    //             string, integer,  float, boolean
    
    $age = 18;
    echo "I am $age years old <br>";

    $cash = 40;
    echo "I have \${$cash} cash. <br>";

    $name = "z3nbu9";
    echo "My name is $name. <br>";

    $gpa = 8.01;
    echo "I have $gpa gpa <br>";

    $employed = true;
    $isalive = false;
    echo $employed . "<br>";
    echo $isalive . "NOthing display <br>";

    $total = null; // Used for declaring variables
    echo $total;


?>
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <h3>This is HTML Document</h3>
</body>
</html>

```

## Arithmetic operators
```index.php
<?php 
    // Arithemetic operators
    // + - * / ** %

    // Increment/decrement operators
    // ++ --

    // Operator Precedence

    $x = 10;
    $y = 5;
    $result = null;

    // $result = $x + $y;
    // $result = $x - $y;
    // $result = $x * $y;
    // $result = $x / $y;
    // $result = $x ** $y;
    // $result = $x % $y;

    echo $result;


    $counter = 10;

    // $counter = $counter + 1;
    // $counter += 1;
    // $counter++;

    // $counter = $counter - 1;
    // $counter -= 1;
    // $counter--;


    echo $counter;

    $total = 1 + 2 - 3 * 4 / 5 ** 6;

    echo $total;

    

?>

```

