# Task:Count the number of Duplicates
## Test
```javascript
Test.assertEquals(alphabetPosition("The sunset sets at twelve o' clock."), "20 8 5 19 21 14 19 5 20 19 5 20 19 1 20 20 23 5 12 22 5 15 3 12 15 3 11");
Test.assertEquals(alphabetPosition("The narwhal bacons at midnight."), "20 8 5 14 1 18 23 8 1 12 2 1 3 15 14 19 1 20 13 9 4 14 9 7 8 20");
```
## My Solution
```javascript
function alphabetPosition(text) {
  let arr = ['a','b','c','d','e','f','g','h','i','j','k','l','m','n','o','p','q','r','s','t','u','v','w','x','y','z'];
  let newText = text.toLowerCase().split('');
  let newArr = [];
  newText.forEach((val, item)=>{
     if(arr.indexOf(val) !== -1){
         newArr.push(arr.indexOf(val)+1);
     }
  });
  return newArr.join(' ');
}
```
## Other Solutions
```javascript
function alphabetPosition(text) {
  return text
    .toUpperCase()
    .match(/[a-z]/gi)
    .map( (c) => c.charCodeAt() - 64)
    .join(' ');
}
```

