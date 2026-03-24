---
title: "Arduino \"invalid application sizeof\" error"
date: 2017-12-30T21:07:08+00:00
slug: "arduino-invalid-application-sizeof-error"
draft: false
description: "Fear the orange bar in Arduino, for comes when all is not well."
cover:
  image: "/images/2018/01/yellowerror.png"
  relative: false
  hiddenInList: false
tag: ["error", "Arduino", "invalid", "sizeof", "nRF24l01", "Radiohead", "struct", "sending", "radio"]
---

This error was frustrating to fix but turned out it was a teensy tiny error of my own.

![snap-1](/images/2017/12/snap-1.jpg)

# Context

I was attempting to send a struct over a radio on Arduino

```
struct State {
    int x;
    int y;
};

struct State MyState;

....

radio.send((uint8_t *)&MyState, sizeof(struct MyState));

```

So I tried to get the size of an instance instead of the struct itself. No wonder Arduino complained that it couldn't find the struct with the error message of: `invalid application ... to incomplete type 'loop()::MyState'`.

Also note that `&` which points to the data, is required to convert the struct into a uint8_t type array for sending. Without it, MyState is a typedef of struct State, and the compiler wouldn't know how to convert a State into a uint8_t.

# Fix

Convert `sizeof(struct MyState)` to `sizeof(struct State)`.

# Post Note

Note that the image used in this post has nothing to do with the error.

# References

[1] [https://arduino.stackexchange.com/questions/26784/is-possible-to-concatenate-integers](https://arduino.stackexchange.com/questions/26784/is-possible-to-concatenate-integers)
[2] [https://stackoverflow.com/questions/8915230/invalid-application-of-sizeof-to-incomplete-type-with-a-struct](https://stackoverflow.com/questions/8915230/invalid-application-of-sizeof-to-incomplete-type-with-a-struct)
