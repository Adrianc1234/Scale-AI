## EXERCISE 1
Write a JavaScript function that receives 2 numbers and returns the multiplication of these numbers.
Sample Data and output:
Example: multiply(2,5)
Expected Output: 10

```javascript
function multiplication(a,b){
	return a * b
}

console.log(multiplication(2,5));
```


## EXERCISE 2

Write a JavaScript function that reverse a number.

Sample Data and output:
Example x = 32243;
Expected Output: 34223

```javascript
const reverse = (a) => parseInt(a.toString().split("").reverse().join(""))
console.log(reverse(1234));
```

## EXERCISE 3

write a function that receives as parameter a "first name", "last name" and "age" and returns a string:

"Hello my name is {{firstname lastname}} and I am {{age}} years old"

Sample Data and output:
Example: concatenation('omar', 'melendrez', 25);
Expected Output: Hello my name is Omar Melendres  and I am 25 years old.

```javascript
function phrase(nombre, apellido, edad){
  		return "Hello my name is "+ nombre +" " + apellido +" and I am "+ edad.toString() + " age years old";
}
console.log(phrase("Adrian", "Carmona", 12));
```

EXERCISE 4

write a function that receives 2 objects and returns only one with the information from both in one

Sample Data and output:
Example: combineObjects({nombre: 'omar'}, {edad: 25})
Expected Output: {nombre: 'omar', edad: 25}

```javascript
function combine(name, edad){
		// (B) COMBINE 2 OBJECTS
		var combined = {...name, ...edad};
		return combined
}

console.log(combine({nombre: 'omar'}, {edad: 25}))
```

## EXERCISE 5

write a function that validates that a received string contains a specific id

Sample Data and output:

Example 1: stringValidation("https://scale.com/product?variant=12345", "12345")
Expected Output 1: TRUE

Example 1: stringValidation("https://scale.com/product?variant=55555", "12345")
Expected Output 1: FALSE

```javascript
function search(url, matchS){
	const validation = url.includes(matchS);
  	return validation
}
//const url = "https://scale.com/product?variant=12345";
const url = "https://scale.com/product?variant=55555";
console.log(search(url, "12345"))
```

## EXERCISE 6

write a function that filters out from an Array all values that are not palindromes.

Sample Data and output:

Example 1: [ "heart", "damamad", "445544", "info", "Kayak"  ]
Expected Output 1: [ "damamad", "445544" , "Kayak" ]

```javascript
function palindromize(words) {
  	//entra el array
  	//map 1 by 1
  	return words.filter(word => {
    	var p = word.toLowerCase().split("").reverse().join("");
    	return p === word.toLowerCase() ? word : undefined;
    })

}
var words = [ "heart", "damamad", "445544", "info", "Kayak" ];
console.log(palindromize(words))
```

## EXERCISE 7

Write a function that validates that the elements of an array contain at least 5 letters and at least 1 repeated letter.

Sample Data and output:

Example 1: [ "house", "children", "rotator", "stats", "wow"  ]
Expected Output 1: [ "rotator", "stats" ]

```javascript
function palindromize(words) {
  	const arrayF = words.filter(string => string.length >= 5)
    const result = arrayF.filter(string =>{
    	for(let i = 0; i<string.length;i++){
        if(string.indexOf(string[i]) !== string.lastIndexOf(string[i])){
        return true
        }
        }
    })
    return result

}
var words = [ "house", "children", "rotator", "stats", "wow"];
console.log(palindromize(words))
```

## COMPRESOS
```javascript
const mult = (a,b) => a*b
const reversed = numb => Number(numb.toString().split('').reverse().join(''))
const combineObjects = (obj1, obj2) => ({...obj1, ...obj2})
const conca = (first, last, age) => `Hello my name is ${first} ${last} and I am ${age} years old`
const stringValidation = (url, id) => url.includes(id)
const palindrome = words => words.filter(word => ((word.toLowerCase() == word.toLowerCase().split('').reverse().join(''))))
const lengthVal = (arr) => arr.filter(word => ((word.length >= 5))).filter(word => (/(\w).*\1/i.test(word)))
```









