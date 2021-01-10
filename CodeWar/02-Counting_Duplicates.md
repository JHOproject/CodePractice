# Task:Count the number of Duplicates
Write a function that will return the count of distinct case-insensitive alphabetic characters and numeric digits that occur more than once in the input string. The input string can be assumed to contain only alphabets (both uppercase and lowercase) and numeric digits.
## Test
```javascript
Test.assertEquals(duplicateCount(""), 0);
Test.assertEquals(duplicateCount("abcde"), 0);
Test.assertEquals(duplicateCount("aabbcde"), 2);
Test.assertEquals(duplicateCount("aabBcde"), 2,"should ignore case");
Test.assertEquals(duplicateCount("Indivisibility"), 1)
Test.assertEquals(duplicateCount("Indivisibilities"), 2, "characters may not be adjacent")
```
## My Solution
```javascript
function duplicateCount(text){
  let arr = [];
  let count = 0;
  let arrCount = new Array();
  let newText = text.toLowerCase();
  for(let i=0;i<newText.length;i++){
        if(arr.indexOf(newText[i]) == -1){
            arr.push(newText[i]);
        }else{
            arrCount.push(newText[i]);
            console.log(newText[i]);
        }
  }
  let result = arrCount.reduce((obj,item)=>{
    obj[item]=obj[item]? obj[item]+=1 : 1;
    return obj; 
  },{});
  return Object.keys(result).length;
}
```
## Best Practice
```javascript
function duplicateCount(text){
  return (text.toLowerCase().split('').sort().join('').match(/([^])\1+/g) || []).length;
}
```