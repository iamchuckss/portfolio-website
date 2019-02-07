+++ 
title = "Cracking the Coding Interview Notes"
slug = "CTCI" 
tags = []
categories = []
+++

- [Chapter 1 - Arrays and Strings](#chapter-1---arrays-and-strings)
  - [Hash Table](#hash-table)
  - [ArrayList & Resizable Arrays](#arraylist--resizable-arrays)
  - [StringBuilder](#stringbuilder)
  - [1.1 - Is Unique](#11---is-unique)
      - [Approach 2: Using a bit vector](#approach-2-using-a-bit-vector)
      - [Discussion](#discussion)

---

# Chapter 1 - Arrays and Strings

Array questions and String questions are often interchangable.

---

## Hash Table

![Hash ](content/images/hash-table.png)

---

## ArrayList & Resizable Arrays

---

## StringBuilder


---

## 1.1 - Is Unique

Implement an algorithm to determine if a string has all unique characters. What if you cannot use additional data structures?

**Notes**

- First ask interview if the string is an ASCII / Unicode string
  - Demonstrate eye for detail & solid foundation in CS
- **Always clarify assumptions with interviewer**

##### Approach 1: Using an array of boolean values

- Assume string is ASCII
  - Immediately return false if string.length() > number of unique characters in the alphabet (128)
    - (size = 256 if assume extended ASCII)
  
```java
public boolean isUniqueString(String str) {
    if (str.length() > 128) return false;

    boolean[] char_set = new boolean[128];
    for(int i = 0; i < str.length(); i++) {
        int val = s.charAt(i);
        if(char_set[val]) { // Already found this char in string
            return false;
        } 
        char_set[val] = true;
    }
    return true;
}
```

**Analysis**

- Time complexity: **O(n)**
  - Can argue to be O(1), for loop will never iterate through more than 128 characters
  - Can be expressed as O(c) space and O(min(c, n)) time
- Space complexity: **O(1)**

#### Approach 2: Using a bit vector

**Notes**

- **Assume string only uses lowercase a-z**
- Reduced space usage by x8
  - Using just a single int

```java
boolean isUniqueString(String str) {
    int checker = 0;
    for(int i = 0; i < str.length(); i++) {
        int val = str.charAt(i) - 'a';
        if((checker & (1 << val)) > 0) {
            return false;
        }
        checker |= (1 << val);
    }
    return true;
}
```

#### Discussion

If we can't use additional data structures:

1. Compare every char of the string to every other char.
   1. Take O(n^2) time, O(1) space
2. IF modification of the input string is allowed
   1. Sort the string in O(n log(n)) time
   2. Linearly check for identical neighboring characters
   3. **Many sorting algorithms take up extra space

---

## 1.2 - Check Permutation

Given two strings, write a method to decide if one is a permutation of the other.

```java
public boolean isPermutationString(String s1, String s2) {
    if(s1.length() != s2.length()) return false;

    HashSet<Character> set = new HashSet<>();

    for(char c : s1.toCharArray()) {
        set.add(c);
    }
    for(char c : s2.toCharArray()) {
        if(set.add(c)) return false;
    }
    return true;
}
```

