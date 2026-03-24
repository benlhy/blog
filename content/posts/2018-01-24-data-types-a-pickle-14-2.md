---
title: "Data types, pointers, and a pickle? (14)"
date: 2018-01-24T15:55:00+00:00
slug: "data-types-a-pickle-14-2"
draft: false
cover:
  image: "/images/2018/01/pointer.jpg"
  relative: false
  hiddenInList: false
tag: ["Arduino", "ULCEK Tutorials", "float", "int", "pointers", "Guides"]
---

When programming in Arduino, you might have occasion to go beyond the standard datatypes of `int` and `String`.

# Data Types

You might have noticed that if you did the following:

```
int count = 5;
int groups = 2;
Serial.println(count/groups);

```

Serial would have printed `3`. This can be a head-scratcher, especially since we never had the occasion to use other data types. A data type refers to what the data can represent. This primarily has to do with `int`, which cannot represent decimal points. The solution is to use a `float` which is another data type, except that now it can handle floating point operations. For example, if we changed the example above a little:

```
float count = 5;
float groups = 2;
Serial.println(count/groups);

```

It would yield the expected value of `2.5`. The trade-off is that in order to do this slightly more complex operation, `float` takes up more memory. To be precise, it uses up twice as much. If you are using a chip like the ESP8266 which has loads of memory, this isn't much of an issue, however, for a chip like the ATTINY45, where space and execution speed is limited, the use of floats might not be the most efficient means of calculation. In the words of the [Arduino website](https://www.arduino.cc/en/Reference/Float) on float:

> Programmers often go to some length to convert floating point calculations to integer math.

So, for our example above, the solution is to add more zeros into our variables:

```
int count = 50;
int groups = 20;
Serial.println(count/groups);

```

The result will yield 25. We can do this without incurring an increase in memory because `ints` can hold values as large as 32768.

They are useful for calibrating values, for example, an analog input where the signal is not as clear as a digital one.

What if you never have an occasion to use such a large value? For example, an analog sensor such as a photosensor might go from 0 to 255. That is a lot of unused memory! The solution is to use `char`, a datatype that is half as large as in `int`.

The interesting thing about `char` is that it is both a number and a character (hence the name). If you printevaribled `124`, a char datatype will print out `|`, hardly the thing we were expecting! The problem is that `Serial.println` prints the ASCII representation of that number. The solution is then to either specify that we wanted to print out a number `Serial.println(charValue,DEC)` or use a little trick to shift `char` to the value that we want to print, if it is 0 ~ 9, `charValue = charValue - '0';`. Doing so resets the count to the list of integers in the ASCII table, and so we can print the value that we intended.

# Pointers

In poking around the code on the web you might have seen data types like these:

```
int instantRead;
int *sensor1;
sensor1 = &instantRead;

```

`*` and `&` are special values that are not part of the variable's name. Instead, `*` means that `sensor1` is a pointer to something that is of type `int`. A pointer is a variable that holds the address (as opposed to the value) of a value.

## What are addresses and pointers?

For example, imagine a wall of boxes labeled with numbers. `box_5_addr` refers **to**  the box labeled '5' (a pointer variable) and `box_3` refers to the value in `box_3`. In your hand is an instruction to look **in** box 5.

```
where_are_my_oranges = *box_5_addr;

```

But when you open box 5, instead it contains a note "Look in box 3".

```
box_5_addr = &box_3;

```

You then go to box 3, and you find two oranges!

```
box_3 = 2;

```

This implies that best_value = 2, also note that these lines are written in reverse order of which they should appear in the code for purposes of illustration.

So if you declare a variable as a pointer, to get to the value inside the variable, you would have to *dereference* the pointer. Pointers accept addresses, so you can pass an address to a pointer like how you'd pass a value to a variable. Pointers are essentially some kind of 'address variable'.

```
int* box_4; // declare a pointer
*box_4 = 10; // dereference it, store value
int box_3; // declare a varible
box_3 = 2;
box_4 = &box_3; // give the address of box_3 to the pointer box_4

```

Why do we do this instead of referring straight to box 3? Well in this case we can save on space: the box that contains the address doesn't have to be as big as the box that contains the oranges. A pointer that points to a float variable doesn't actually use the same space as the float variable, it doesn't need to, it just needs to store the address of the variable. This gives more flexibility and power to the programmers.

In this case, all that talk about 8-bit and 16-bit microcontrollers suddenly becomes relevant because 8-bits refer to the size of each block of memory, which is how large the address needs to be.

Despite the analogy about searching, pointers also come out to be faster because there isn't a need to duplicate the data.

When you pass a value into a function, it isn't writable by the function because it creates a copy that only exists within the function. However, with a pointer, you are able to modify the variable that exists outside the function by using a pointer.

Note that `*` also serves as a deference operator. That means that if `*(&number)` means that they both cancel out and we get the value that is stored at number.

```
function(int *point){ // we need an address to be passed here
 *point = *point + 1; // increment the value at the address by 1
}

int value = 3;
function(&value); // we pass the address of value to our function
Serial.println(value); // note that there is no return value on the function

```

```
int pickle,jar;
int *hand;
hand = &jar;
jar = 20;

```

#### What you learned

- What are data types.

- What are pointers.

- How to use different data types

- How to use pointers

#### Challenges

- Write a function that changes changes an external value.

- Solve the following problem by determining what will be printed out before running the program:

```
int pickle,jar;
int *hand;
hand = &jar;
jar = 20;
Serial.println(hand);
pickle = *hand;
jar = jar - 10; // Big hands
Serial.println(pickle);
*hand = *hand + 5 ; // Can't fit through the jar
Serial.println(jar);
pickle = pickle + 2; //Uh, the pickles were eaten?
Serial.println(jar);

```
