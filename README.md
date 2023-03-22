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