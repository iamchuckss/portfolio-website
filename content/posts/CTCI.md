+++
date = "2017-01-08"
title = "Cracking the Coding Interview Notes"
description = "The post demonstrates features of the coder theme."
images = ["/images/N90.jpg"]
math = "true"
+++

Table of Contents
=================

- [Chapter 1 - Arrays and Strings](#chapter-1---arrays-and-strings)
  - [Hash Table](#hash-table)
  - [ArrayList & Resizable Arrays](#arraylist--resizable-arrays)
  - [StringBuilder](#stringbuilder)
  - [1.1 - Is Unique](#11---is-unique)
      - [Approach 2: Using a bit vector](#approach-2-using-a-bit-vector)
      - [Discussion](#discussion)


# Chapter 1 - Arrays and Strings

Array questions and String questions are often interchangable.


## Hash Table

![Hash ](content/images/hash-table.png)


## ArrayList & Resizable Arrays


## StringBuilder


## 1.1 - Is Unique

Implement an algorithm to determine if a string has all unique characters. What if you cannot use additional data structures?

**Notes**

- First ask interview if the string is an ASCII / Unicode string
  - Demonstrate eye for detail & solid foundation in CS
- **Always clarify assumptions with interviewer**

#### Approach 1: Using an array of boolean values

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

#### Analysis

Confirm some details with our interviewer:

1. Understand if permutation comparison is case sensitive ("God" vs "dog")
2. Ask if whitespace is significant ("god    " vs "dog")


#### Approach 1: Sort the strings.

If two strings are permutations, they have the same characters, but in different orders. 

Therefore, sort them into the same order and compare them.

```java
public String sort(String s) {
    char[] content = s.toCharArray();
    java.util.Arrays.sort(content);
    return new String(content);
}

public boolean isPermutationString(String s1, String s2) {
    if(s1.length() != s2.length()) {
        return false;
    }
    return sort(s1).equals(sort(s2));
}

```

Though this algorithm is **not as optimal**, but it's **clean, simple, and easy to understand**. 


#### Approach 2: Check if two strings have identical character counts.

Counting the appearance of each characters. **Better efficiency**

```java
boolean isPermutation(String s1, String s2) {
    if (s1.length() != s2.length()) {
        return false;
    }

    int[] letters = new int[128]; // Assumption

    char[] s1_array = s1.toCharArray();
    for(char c : s1_array) { // count number of each char in s
        letters[c]++;
    }

    for(int i = 0; i < s2.length(); i++) {
        int c = (int) s2.charAt(i);
        letters[c]--;
        if(letters[c] < 0) {
            return false;
        }
    }
    return true;
}
```

Assumption made that char set was ASCII on line 6, always check about size of character set with interviewer.

## 1.3 - URLify

_Write a method to replace all spaces in a string with '%20'._

_You may assume that the string has sufficient space at the end to hold the additional characters, and that you are given the "true" length of the string. (Note: If implementing in Java, please use a character array so that the operation is performed in place.)_

Example:

Input:     "Mr John Smith     ", 13

Output:    "Mr%20John%20Smith"

#### Approach

A commmon approach in string manipulation problems is: Edit the string starting from the end and working backwards.

The extra buffer at the end allows us to change characters without worrying about what we're overwriting.

In this problem, we employ a two-scan approach.

1. In the first scan, we count the number of spaces.
    - Tripling this number, compute how many extra characters we need.
2. In the second scan (done in reverse order), we edit the string.

```java
public void replaceSpaces(char[] str, int trueLength) {
    int spaceCount = 0, index;
    for(int i = 0; i < trueLength; i++) {
        if (str[i] == ' ') {
            spaceCount++;
        }
    }

    index = trueLength + spaceCount * 2;
    if(trueLength < str.length) str[trueLength] = '\0'; // End array
    for(int i = trueLength - 1; i >= 0; i--) {
        if(str[i] == ' ') {
            str[index - 1] = '0';
            str[index - 2] = '2';
            str[index - 3] = '%';
            index = index - 3;
        } else {
            str[index - 1] = str[i];
            index--;
        }
    }
}
```

