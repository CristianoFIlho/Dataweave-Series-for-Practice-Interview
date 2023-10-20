# Dataweave-Series-for-Practice-Interview-Part-1
Dataweave Series for Practice &amp; Interview Part-1 - Practice Dataweave here :- Dataweave Playground  Important Note:- First Copy Input, Output, DW Code &amp; Use Json Online Validator, to format the JSON, since medium changes the JSON.

Certainly! Here is the content transformed into a `README.md` file:

```markdown
# Dataweave Code Snippets

## 1. Filter Odd & Even Numbers from an Array

**Input:**

```
[0,1,2,3,4,5,6,7,8,9]
```

**Output:**

```json
{
  "Even": [0,2,4,6,8],
  "Odd": [1,3,5,7,9]
}
```

**Dataweave Code:**

```dataweave
{
  "Even": payload filter (($ mod 2)==0),
  "Odd": payload filter !(($ mod 2)==0)
}
```

## 2. Interchange Key-Value in an object

**Input:**

```
[
 {
 "message1":"Hello World"
 },
 {
 "message2":"Hello Shubham"
 }
]
```

**Output:**

```
[
 {
 "Hello World": "message1"
 },
 {
 "Hello Shubham": "message2"
 }
]
```

**Dataweave Code:**

```dataweave
payload map ((item, index) -> 
 item mapObject(v,k,n)-> {
 (v) : k
 }
)
```

## 3. Reverse the Words of a Sentence

**Input:**

```
{
 "Reverse": "Dataweave 2.0 is Cool"
}
```

**Output:**

```
"Cool is 2.0 Dataweave"
```

**Dataweave Code:**

```dataweave
(payload.Reverse splitBy(' '))[-1 to 0] joinBy(' ')
```

## 4. Filter an Array by an Objects field Value(e.g., Year)

**Input:**

```
[
 {
 "Product":"A",
 "Year":"2020"
 },
 {
 "Product":"B",
 "Year":"2019"
 }
]
```

**Output:**

```
[
 {
 "Product": "A",
 "Year": "2020"
 }
]
```

**Dataweave Code:**

```dataweave
payload filter $.Year>2019
```

## 5. Filter Even Values out from any array

**Input:**

```
[0,1,2,3,4,5,6,7,8,9]
```

**Output:**

```
[0,2,4,6,8]
```

**Dataweave Code:**

```dataweave
payload filter isEven($)
```

## 6. Print Last x characters of a number or String

**Input:**

```
[
 {
 "num":"000147"
 },
 {
 "num":"100297"
 },
]
```

**Output:**

```
[
 {
 "num": "147"
 },
 {
 "num": "297"
 }
]
```

**Dataweave Code:**

```dataweave
import last from dw::core::Strings

payload map(v,k)-> (num: v.num last 3)
```

## 7. Count the Number of Characters in a String

**Input:**

```
{
 "string":"12shubham245fc"
}
```

**Output:**

```
9
```

**Dataweave Code:**

```dataweave
import countCharactersBy,isAlpha from dw::core::Strings

payload.string countCharactersBy isAlpha($)
```

## 8. Hamming Distance

**Input:**

```
[
 {
 "hola":"bola"
},
{
 "chao":"figo"
}
]
```

**Output:**

```
[
 {
 "Hamming Distance": 1
 },
 {
 "Hamming Distance": 3
 }
]
```

**Dataweave Code:**

```dataweave
import hammingDistance from dw::core::Strings

(payload map(v,k)->{
 (v mapObject(value,key,index)->{
 "Hamming Distance":key hammingDistance value
 })
})
```

## 9. Find Number of Object or Key-Value Pairs in an Array

**Input:**

```
[{
 "k1":"v1"
},
{
 "k2":"v2"
}
]
```

**Output:**

```
2
```

**Dataweave Code:**

```dataweave
sizeOf(payload)
```

## 10. Generate Random Numbers

**Input:**

```
[10]
```

**Output:**

```
[89,26,21,77,80,31,24,13,54,55]
```

**Dataweave Code:**

```dataweave
(1 to payload[0]) map randomInt(100)
```
```

Feel free to let me know if you need any further assistance!
