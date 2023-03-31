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

## Telephone Number Validator

- Create a function that validates North American phone number format
- It must contain 10 digits including three digit area code.
- It can contain 11 digits including country code, which must be 1
  - country code must be single digit
- It may or may not contain hyphens between block sets.
- It may or may not contain parentheses to envelope the the area code, exclusively
- It must start with a number
- If using spaces or hyphens, it must follow 1-3-3-4 pattern
- It cannot contain any special characters aside from opening and closing parentheses for the area code and hyphens to separate the pattern specified above.

- I think regex is the way to go, but it's going to be a lengthy one.

```js
function telephoneCheck(str) {
  if ((/^1?[-\s]?\(?\d{3}\)?[-\s]?\d{3}[-\s]?\d{4}$/).test(str)) {
    return true;
  }
  return false;
}
```

- This code almost passes, but there is a problem.
- I have to ensure that if there is an opening parenthesis for the area code, there must be a closing parenthesis, and vice versa.
- The above code leaves the opening and closing parentheses independently optional, which will cause errors.

```js
function telephoneCheck(str) {
  if ((/^((1?[-\s]?\d{3}\)[-\s]?\d{3}[-\s]?\d{4})|(1?[-\s]?\d{3}[-\s]?\d{3}[-\s]?\{4}))$/).test(str)) {
    return true;
  }
  return false;
}
```

- This code works, but I am not too proud of it.
- It just looks unnecessarily long.
- There must be another way to keep it simpler.
- I've tried using positive lookahead `(?=\(\d{3}\))` and negative `(?!\d{3}\))` as a means to ensure that if there is an opening parenthesis, it must be followed by a closing parenthesis, or if it does not have an opening parenthesis, it must not have a closing parenthesis.
- I'm still not familiar with lookaheads.
- I will be coming back to this problem again later.

## Cash Register

- create a cash register drawer function
  - purchase price (price) is the first argument
  - payment (cash) is the second argument
  - cash-in-drawer (cid) is the third argument
- return `{status: "INSUFFICIENT_FUNDS", change: []}` if cid is less than the change due, or exact change is not available
- return `{status: "CLOSED", change: [...]}` with `cid` as the value for `change` if `change` is equal to the change due
- return `{status: "OPEN", change: [...]}` with the change due in coins and bills, sorted in highest to lowest order, as the value of the `change` key.

- It's obvious that the `change due` is `cash` minus `price`
- create an index array of objects to assign the monetary value of each coin and bill.
- Use `for` loop `while (change >= 0)` to iterate through the index array and `cid` to determine which bill or coin should be returned as change
- Use a series of `if` statements to distinguish the three scenarios described above.
- I need to think about this a bit more...

- Trying to work this case by case.

```js
function checkCashRegister(price, cash, cid) {
  let change = cash - price;
  let result = {status: "", change: []};
  console.log(change);
  const currArr = [
    ["ONE HUNDRED", 100],
    ["TWENTY", 20],
    ["TEN", 10],
    ["FIVE", 5],
    ["ONE", 1],
    ["QUARTER", 0.25],
    ["DIME", 0.10],
    ["NICKEL", 0.05],
    ["PENNY", 0.01]
  ];

  while (change >= 0) {
    for (let i = 0; i < currArr.length; i++) {
      if (change >= currArr[i][1]) {
        change -= currArr[i][1];
        console.log(change)
        break;
      }
    }
  }
  return result;
}

checkCashRegister(19.5, 20, [["PENNY", 1.01], ["NICKEL", 2.05], ["DIME", 3.1], ["QUARTER", 4.25], ["ONE", 90], ["FIVE", 55], ["TEN", 20], ["TWENTY", 60], ["ONE HUNDRED", 100]]);
```

- The above code is far from complete.
- I just wanted to get it started and see what I need to do.
- It's obvious that the change will equal to the value of `cash` minus the value of `price`.
- `const` variable `currArr` is created to outline the value of each coin and bill.
- `while` loop is used with a condition `change >= 0`
- Use a `for` loop to iterate through the `currArr` array to determine which currency will be used to return the change.

- but the above code does not update the `while` loop properly and is triggering a `potential infinite loop`.
- I need to think a bit more about how to approach this.
  - How to assign the `result {status: "", change: []}`
  - How to keep track of how much cash is remaining in the entire `cid`
    - Do I need another `for` loop to iterate through the `cid` array to check its balance?

- After some thought, I should revert the `currArr` order from smallest to largest currency.
- I reversed it because of the `if` statement `(change >= currArr[i][1])`
  - Since the array begins with 'PENNY', `currArr[0][1]` will always be smaller than `change'
  - Why didn't I think about reversing the `for` loop?
- `cid` and `currArr` should be in-line so that the index value matches for modifying the `cid` values.

```js
function checkCashRegister(price, cash, cid) {
  let change = cash - price;
  let result = {status: "", change: []};
  console.log(change);
  const currArr = [
    ["PENNY", 0.01],
    ["NICKEL", 0.05],
    ["DIME", 0.10],
    ["QUARTER", 0.25],
    ["ONE", 1],
    ["FIVE", 5],
    ["TEN", 10],
    ["TWENTY", 20],
    ["ONE HUNDRED", 100],
  ];

  while (change >= 0) {
    for (let i = currArr.length - 1; i >= 0; i--) {
      if (change >= currArr[i][1]) {
        change -= currArr[i][1];
        console.log(change)
        break;
      }
    }
  }
  return result;
}

checkCashRegister(19.5, 20, [["PENNY", 1.01], ["NICKEL", 2.05], ["DIME", 3.1], ["QUARTER", 4.25], ["ONE", 90], ["FIVE", 55], ["TEN", 20], ["TWENTY", 60], ["ONE HUNDRED", 100]]);
```

- Instead of having to write out 'currArr[i][1]' every time, it would be better to assign it as a value to a new variable

```js
function checkCashRegister(price, cash, cid) {
  let change = cash - price;
  let result = {status: "", change: []};
  console.log(change);
  const currArr = [
    ["PENNY", 0.01],
    ["NICKEL", 0.05],
    ["DIME", 0.10],
    ["QUARTER", 0.25],
    ["ONE", 1],
    ["FIVE", 5],
    ["TEN", 10],
    ["TWENTY", 20],
    ["ONE HUNDRED", 100],
  ];

    for (let i = currArr.length - 1; i >= 0; i--) { // reverse for loop
      let currNow = currArr[i][1];  // currency being processed
      let cidNow = cid[i][1];       // corresponding cid currency
      let changeAcc = [currArr[i][0], 0]; // keeping a track of change being returned

    while (change >= currNow && cidNow > 0) {  // run the loop while cid has enough of the currNow value in the drawer

      if (change >= currNow) { //if currNow value can be used to return the change
        change -= currNow;    // take away value of currNow from change 
        cidNow -= currNow;    // subtract currNow value from corresponding cid
        changeAcc[1] += currNow;  // Add currNow value to changeAcc
        result.change.push(changeAcc); // push changeAcc to change property in the result.
        console.log(result)
      }
    }
  }
  return result;
}

checkCashRegister(19.5, 20, [["PENNY", 1.01], ["NICKEL", 2.05], ["DIME", 3.1], ["QUARTER", 4.25], ["ONE", 90], ["FIVE", 55], ["TEN", 20], ["TWENTY", 60], ["ONE HUNDRED", 100]]);
```

- It seems to work, but it's pushing `changeAcc` as separate elements rather than returning a single accumulated result.
- I still need to figure out how to check `cid` if it has sufficient funds in order to update the status of the result.
- Also, the code looks too complicated and unreadable.
- More thinking tomorrow.

- After some thought, trial and error, I've decided to rename the variables since it was getting a bit confusing.
- Did some shuffling around because I wasn't getting the result I wanted.
- `result.change.push` should be outside of the `while` loop to prevent it from pushing the same currency value in separate arrays.

```js
function checkCashRegister(price, cash, cid) {
  let change = cash - price;
  let result = {status: "", change: []};
  console.log(change);
  const currArr = [
    ["PENNY", 0.01],
    ["NICKEL", 0.05],
    ["DIME", 0.10],
    ["QUARTER", 0.25],
    ["ONE", 1],
    ["FIVE", 5],
    ["TEN", 10],
    ["TWENTY", 20],
    ["ONE HUNDRED", 100],
  ];

  for (let i = currArr.length - 1; i > 0; i--) {
    let currName = currArr[i][0];
    let currValue = currArr[i][1];
    let currTotal = cid[i][1];
    let currCount = 0;

    while (change >= currValue && currTotal > change) {
      change -= currValue;
      currTotal -= currValue;
      currCount += currValue;
    }
    if (currCount > 0) {
      result.change.push([currName, currCount]);
    }
  }
  return result;
};
```

- The `for` loop will iterate through `currArr` backwards to be in line with `cid`
- `currName` is the currency name
- `currValue` is the currency value
- `currTotal` is the total amount the current currency in `cid`
- `currCount` keeps track of how much change is being returned.

- `while` value of `change` is greater than or equal to the current `currValue` in iteration, AND `currTotal` in `cid` is greater than the value of `change`
  - subtract current `currValue` from `change`
  - subtract current `currValue` from `currTotal` in `cid`
  - add current `currValue` to `currCount`
  - Note that the `while` loop will terminate if non of the `currValue` in `currArr` is greater than the `change` value as it decrements.

- After the `while` loop is terminated, `if` `currCount` is greater than 0, meaning, if return change value has been accrued to `currCount`
- `push` an array of `currName` and `currCount` to the `change` property of `result'.

- Based on the initial `cid` passed to the function, the above code will return `{ status: "", change: [['QUARTER', 0,5]]}`
- Still not quite sure what to do with the `status` property.

- Trying to change up some logic here.
- If the total sum of `cid` is equal to the change due, it should return `CLOSED`, even before running the loop.
- If the total sum of `cid` is less than the change due, it should immediately return `INSUFFICIENT_FUNDS`.
  - But, it also needs to return `INSUFFICIENT_FUNDS` if `cid` is not able to return exact change.

```js
let cidTotal = cid.map(([_, num]) => num).reduce((acc, elem) => acc + elem, 0).toFixed(2);

if (cidTotal == change) {
  result.status = "CLOSED";
  result.change = cid;
}
```

- `map` is used with `destructuring assignment` to extract the numerical values of the nested arrays.
- `reduce` is used to acquire the sum of the elements.
- `toFixed(2)` is used to limit the decimal places by 2.
- `if` `cidTotal` is equal to the value of `change`, return status `CLOSED` and `cid` as the value for `change` property.

- I have to figure out how to make all this work together

- I found out how to simplify the `cidTotal` function

```js
let cidTotal = cid.reduce((acc, [_, num]) => acc + num, 0);
```

- I didn't know that the value passed to `reduce` argument can be an array to access its element independently.

- An attempt to put the above together

```js
function checkCashRegister(price, cash, cid) {
  let change = cash - price;
  let result = {status: "", change: []};
  console.log(change);
  const currArr = [
    ["PENNY", 0.01],
    ["NICKEL", 0.05],
    ["DIME", 0.10],
    ["QUARTER", 0.25],
    ["ONE", 1],
    ["FIVE", 5],
    ["TEN", 10],
    ["TWENTY", 20],
    ["ONE HUNDRED", 100],
  ];

  let cidTotal = cid.reduce((acc, [_, num]) => acc + num, 0).toFixed(2);

  if (change > cidTotal) {
    result.status = "INSUFFICIENT_FUNDS";
    result.change = [];
    return result;
  } else if (cidTotal == change) {
    result.status = "CLOSED";
    result.change = cid;
    return result;
  };

  for (let i = currArr.length - 1; i >= 0; i--) {
    let currName = currArr[i][0];
    let currValue = currArr[i][1];
    let currTotal = cid[i][1];
    let currCount = 0;
  

  while (change >= currValue && currTotal >= currValue) {
    change -= currValue;
    currTotal -= currValue;
    currCount += currValue;
    }
    if (currCount > 0) {
      result.status = "OPEN";
      result.change.push([currName, currCount]);
    }
  }
  return result;
}
```

- still need to figure out how to return 'status: "INSUFFICIENT_FUNDS", change: [];` when exact change cannot be returned.
  - I tried `else if (change % currValue !== 0)`, but this won't work because 'PENNY' will always return 0
  - I need to figure out a different condition
- Also, it's not iterating to the last penny if multiple currency arrays are pushed to `result.change`
