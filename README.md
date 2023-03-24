# FCC---JS-Algorithms-and-Data-Structures-Projects

## Palindrome Checker

- Problem itself is self explanatory.
- I initially came up with a very inefficient solution, which doesn't even work properly

```js
function palindrome(str) {
  let arr = str.toUpperCase().replace(/[\W|_]+/g, "").split("");
let newArr1 = [];
let newArr2 = [];
  for (let i = 0; i < arr.length; i++) {
    newArr1.push(arr[i]);
    newArr2.unshift(arr[i]);
  };
  console.log(newArr1.join(""));
  console.log(newArr2.join(""));
  if (newArr1 === newArr2) {
    return true;
  }
  return false;
}
```

- The problem with this is that, first of all, it's inefficient.
- Secondly, the `if` statement is checking if `newArr1` and `newArr2` are being compared by reference, not by value.
- I can do another `for` loop through each of the newArrs, but that just seems silly.
  
```js
function palindrome(str) {
  let arr = str.toUpperCase().replace(/[\W|_]+/g, "").split("");
let newArr1 = [];
let newArr2 = [];
  for (let i = 0; i < arr.length; i++) {
    newArr1.push(arr[i]);
    newArr2.unshift(arr[i]);
  };
  if (newArr1.join("") === newArr2.join("")) {
    return true;
  }
  return false;
}
```

- This little tweak should compare the string values of the two and return a correct result.
- But still, I am not happy with this solution.
- I should be able to iterate through each character of the string as it compares the original to the reversed version.
- Then I remembered (I came across) the `reverse()` method.

```js
function palindrome(str) {
  let arr = str.toUpperCase().replace(/[\W|_]+/g, "").split("");
  let reverseArr = arr.slice().reverse();

  for (let i = 0; i < arr.length; i++) {
    if (arr[i] !== reverseArr[i]) {
      return false;
  }
}
  return true;
}
```

- The string argument is converted into all uppercase-alphanumeric-only array, which is split up by each character
- That newly created array is sliced to make a copy, and reversed, which is then assigned to `reverseArr`
- Use a `for` loop to iterate through both arrays.
- As it iterates, if character at index [i] of `arr` is not strictly equal to character at index [i] of `reverseArr`, return false
- If the iteration reaches the end of both strings without returning false, it is a palindrome. Return true.

## Roman Numeral Converter

- Thought about using a switch statement. But seemed like too much labor.
- Then I tried to write out the logic in my head, step by step, using recursion, and came up with the following

```js
function convertToRoman(num) {
  let romanIndex = ['M', 'CM', 'D', 'CD', 'C', 'XC', 'L', 'XL', 'X', 'IX', 'V', 'IV', 'I'];
  let arabicIndex = [1000, 900, 500, 400, 100, 90, 50, 40, 10, 9, 5, 4, 1];

  if (num === 0) {
    return "";
  } else {
  for (let i = 0; i < arabicIndex.length; i++) {
    if (num % arabicIndex[i] === "number" > 0) {
       let rem = convertToRoman(num - arabicIndex[i]);
       rem.push(romanIndex[i]);
       return rem.join("");
      }
    }
  } 
  return "";
}
   
console.log(convertToRoman(36));
```

- Declare two variables with an array of roman numeral strings, and another with an array of corresponding arabic numerals.
- Define a base case for recursion
  - if num === 0, return an empty array.
- if num is not strictly equal to 0, start a `for` loop to iterate through the `arabicIndex` array.
  - as it iterates through the `arabicIndex`, if num is divisible by an arabic numeral at index[i], and the remainder is a 'number' greater than 0,
  - call `convertToRoman` again with the value of `num` minus the current value of arabicIndex[i].
- `unshift` the value of the current [i] in `romanIndex` to `rem` array.
- repeat process until `num` reaches 0.
- use `join` to combine the individual string variables collected in the rem array and return result
  - NOTE: if an empty array is not explicitly defined to push or unshift to, the `push` or `unshift` method will automatically create an array.
- If `num` is not divisible by any number in the `arabicIndex` array, return an empty string.

- Some problems with this code.
- first of all, in the `if` statement in the `for` loop, if the remainder is a `number` greater than 0 will always return true because the boolean value `number` will evaluate to `NaN`. So this statement does not work.
- I could change the statement so that it looks for the first number that is less than `num`
  - `if (num >= arabicIndex[i])`
- Also, `push` will return the result in reverse order. Use `unshift`
- `return rem.join("")` will end up returning a string, which will cause problems for `unshift`, which is an array method.

```js
function convertToRoman(num) {
  let romanIndex = ['M', 'CM', 'D', 'CD', 'C', 'XC', 'L', 'XL', 'X', 'IX', 'V', 'IV', 'I'];
  let arabicIndex = [1000, 900, 500, 400, 100, 90, 50, 40, 10, 9, 5, 4, 1];

  if(num === 0) {
    return [];
  } else {
    for (let i = 0; i < arabicIndex.length; i++) {
      if (num >= arabicIndex[i]) {
        let rem = convertToRoman(num - arabicIndex[i]);
        rem.unshift(romanIndex[i]);
        return rem;
      }
    }
  }
  return "";
}

console.log(convertToRoman(36));
```

- Since `unshift` is an array method, begin with an empty array instead of an empty string below the base case. Figure out how to convert it to a string later.
- When num === 0, return `rem`, which in the above case is `[ 'X', 'X', 'X', 'V', 'I' ]`
- I need to return a string, not an array.
- I tried `return [].join("")` below the base case. It didn't work since it converts the value into a string.
- I also tried `return rem.join("")` in the `if` statement, which also returns a string to cause problems for `unshift`.
- Both seems to be causing problems for `unshift`, so I will have to modify that line.

- I could try
  - `rem = rem.concat(romanIndex[i]);`, since `concat()` will return a string value. But this returns a reverse order array `[ 'I', 'V', 'X', 'X', 'X']`
    - This is returning an array result because the `romanIndex` values are being concatenated to the `rem` array
  - Then reverse the logic - `rem = romanIndex[i].concat(rem)`, which returns the proper result.
    - This returns a correct string value because the values gathered in `rem` array are being taken out and concatenated into a string value

OR

- I could also try
  - `rem = rem += romanIndex[i];`, which also returns the reverse order result
  - Then reverse it again - `rem = romanIndex[i] += rem`, which returns the correct result.

- `return []` below the base case is throwing some confusion.
- It has an effect on the code in some cases, and not in other cases.
- There really isn't any reason for the code to return an empty array. I could just begin with an empty string, which seems to fit the context better.

```js
function convertToRoman(num) {
  let romanIndex = ['M', 'CM', 'D', 'CD', 'C', 'XC', 'L', 'XL', 'X', 'IX', 'V', 'IV', 'I'];
  let arabicIndex = [1000, 900, 500, 400, 100, 90, 50, 40, 10, 9, 5, 4, 1];

  if (num === 0) {
    return "";
  } else {
    for (let i = 0; i < arabicIndex.length; i++) {
      if (num >= arabicIndex[i]) {
        let rem = convertToRoman(num - arabicIndex[i]);
        rem = romanIndex[i].concat(rem); // OR rem = romanIndex[i] += rem;
        return rem;
      }
    }
  }
  return "";
}
```

## Caesar's Cipher

- Write a function which takes ROT13 encoded string as input and returns a decoded string

- I came up with something without too much thought.

```js
function rot13(str) {
  let result = "";
  let alphaStr = "ABCDEFGHIJKLMNOPQRSTUVWXYZ"
  
  for (let i = 0; i < str.length; i++) {
    for (let j = 0; j < alphaStr.length; j++) {
      if (str[i] === alphaStr[j]) {
        if (j < 13) {
          result = result.concat(alphaStr[j + 13]);
          break;
        } else if (j > 12) {
          result = result.concat(alphaStr[j - 13]);
          break;
        }
      }
    }
  }
  return result;
}
```

- The logic is there, but I didn't account for non-alphabetic characters.
- Maybe I need to use regex.
- Also, it doesn't iterate to the end of the `str`.

So I tried the following

```js
function rot13(str) {
  let result = "";
  let alphaStr = "ABCDEFGHIJKLMNOPQRSTUVWXYZ"
  for (let i = 0; i < str.length; i++) {
    for (let j = 0; j < alphaStr.length; j++) {
      if (str[i] !== (/[A-Z]+/g)) {
        result = result.concat(str[i]);
        break;
      } else if (str[i] === alphaStr[j]) {
        if (j < 13) {
          result = result.concat(alphaStr[j+13]);
          break;
        } else if (j > 12) {
          result = result.concat(alphaStr[j-13]);
        }
        console.log(result)
      }
    }
  }
  return result;
}

console.log(rot13("SERR PBQR PNZC"));
```

- But there seems to be something wrong with the `if` statement comparing the string element to a regex.
  - Probably not a valid comparison statement
- I could check if the string element is not a part of the `alphaStr`, and if not, concat to the result string as it is.
  - Use `.test()` method
  - `if (!(/[A-Z]/).test(str[i]))`

```js
function rot13(str) {
  let result = "";
  let alphaStr = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";

  for (let i = 0; i < str.length; i++) {
    for (let j = 0; j < alphaStr.length; j++) {
      if (!(/[A-Z]/).test(str[i])) {
        result = result.concat(str[i]);
        break;
      } else if (str[i] === alphaStr[j]) {
        if (j < 13) {
          result = result.concat(alphaStr[j+13]);
          break;
        } else if (j > 12) {
          result = result.concat(alphaStr[j-13]);
        }
      }
    }
  }
  return result;
}
```

- code passes. But it looks a bit bloated and inefficient.
- Might try some other solutions using `.charAt` and `replace`

- meanwhile, I can slim down the above code a bit

```js
function rot13(str) {
  let result = "";
  let alphaStr = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";

  for (let i = 0l i < str.length; i++) {
    for (let j = 0; j < alphaStr.length; j++) {
      if (!(/[A-Z]/).test(str[i])) {
        result += str[i];
        break;
      } else if (str[i] === alphaStr[j]) {
        if (j < 13) {
          result += alphaStr[j+13];
          break;
        } else {
          result += alphaStr[j-13];
        }
      }
    }
  }
  return result;
}
```

- I might try a `forEach()` solution.