# Dataweave-Series-for-Practice-Interview-Part-10
Dataweave Series for Practice &amp; Interview Part-10 - Practice Dataweave here :- Dataweave Playground  Important Note:- First Copy Input, Output, DW Code &amp; Use Json Online Validator, to format the JSON, since medium changes the JSON.



```markdown
## Trim Only the End Spaces, and skip trimming a particular Key
### Explanation/Logic
In the following input, skip trimming the 'Code' Key and also trim only the end spaces. In the first object, only Billed and Salary need to be trimmed. In the second object, only Age and Salary need to be trimmed.

### Input
```json
{
  "Employee": [
    {
      "Code": "    501-AKASH",
      "Age": "26",
      "Salary": "900000     ",
      "Date": "03/04/2022",
      "Billed": "NO     "
    },
    {
      "Code": "    502-PRIYA",
      "Age": "30   ",
      "Salary": "1000000     ",
      "Date": "22/02/2021",
      "Billed": "YES"
    }
  ]
}
```

### Output
```json
{
  "Employee": [
    {
      "Code": "    501-AKASH",
      "Age": "26",
      "Salary": "         900000",
      "Date": "03/04/2022",
      "Billed": "NO     "
    },
    {
      "Code": "    502-PRIYA",
      "Age": "30",
      "Salary": "         1000000",
      "Date": "22/02/2021",
      "Billed": "YES    "
    }
  ]
}
```

### Dataweave Code
```dw
%dw 2.0
output application/json
fun trimEndSpace(val)= if(val[-1]=="") trimEndSpace(val[0 to -2]) else val
fun trimAllValues(obj)=
  obj mapObject {
        (if ('$$' == "Code") (($$): $) else ($$): trimEndSpace($))
      }
---
"Employee": payload.Employee map ((item, index) -> 
 trimAllValues(item)
)
```

## Print the following Input to Output pattern
### Explanation/Logic
In the output, the number of "tasks" objects depends on the number of unique Ids in the input. For example, if there are Ids AB, AC, AC, AC, AB, there will be 2 tasks (AB/AC) each for one unique id. The "lineItems" array always contains 2 objects, but the "charges" array objects depend on the number of ABs/ACs in the input.

### Input
```json
{
 "results": [
   {
     "id": "AB"
   },
   {
     "id": "AC"
   },
   {
     "id": "AC"
   },
   {
     "id": "AC"
   },
   {
     "id": "AB"
   }
 ]
}
```

### Output
```json
[
  {
    "AB": {
      "lineItems": [
        {
          "no": "1",
          "charges": [
            {
              "AB": "Delivery"
            },
            {
              "AB": "Delivery"
            }
          ]
        },
        {
          "no": "2",
          "charges": [
            {
              "AB": "Delivery"
            },
            {
              "AB": "Delivery"
            }
          ]
        }
      ]
    }
  },
  {
    "AC": {
      "lineItems": [
        {
          "no": "1",
          "charges": [
            {
              "AC": "Delivery"
            },
            {
              "AC": "Delivery"
            },
            {
              "AC": "Delivery"
            }
          ]
        },
        {
          "no": "2",
          "charges": [
            {
              "AC": "Delivery"
            },
            {
              "AC": "Delivery"
            },
            {
              "AC": "Delivery"
            }
          ]
        }
      ]
    }
  }
]
```

### Dataweave Code
```dw
%dw 2.0
output application/json
fun y(val)=payload.results reduce ((item, acc=0) -> if (item.id~=val) acc + 1 else acc)
---
payload.results distinctBy $ map 
{
    ($.id):{
        "lineItems":[
            {
                "no":"1",
                "charges": (0 to y($.id)-1) reduce ((item, acc=[{}]) -> if (item !=4)acc+ {
                    ($.id):"Delivery"} else acc ) filter $!={}
                },
    {
                "no":"2",
                "charges": (0 to y($.id)-1) reduce ((item, acc=[{}]) -> if (item !=4)acc+ {
                    ($.id):"Delivery"} else acc ) filter $!={}
            }
        ]
    }
}

```

## Keys should come dynamic in response
### Input
```json
[
  {
    "key1": "address",
    "value": "add-1"
  },
  {
    "key1": "address",
    "value": "add-2"
  },
  {
    "key1": "postal",
    "value": "208021"
  }
]
```

### Output
```json
[
  {
    "address": "add-1 add-2"
  },
  {
    "postal": "208021"
  }
]
```

### Dataweave Code
```dw
%dw 2.0
import * from dw::core::Objects
output application/json
---
valueSet(payload groupBy ((item, index) -> item.key1)) map ((group, index1) ->{
    (group[0].key1):flatten(group.value) joinBy " "
} )
```

## Business is parent of industry
### Input
```json
[
 {
  "business": {
   "id": "rel",
   "desc": "rel industry",
   "industry": {
    "id": "manufacture",
    "desc": "manufacturing",
    "businessLine": {
     "id": "residential",
     "desc": "residential manufacture",
     "facility": {
      "id": "mumbai",
      "desc": "mumbai location"
     }
    }
   }
  }
 }
]
```

### Output
```json
{
  "asset": [
    {
      "key": "rel",
      "desc": "rel industry",
      "parent": "business"
    },
    {
      "key": "manufacture",
      "desc": "manufacturing",
      "parent": "industry"
    },
    {
      "key": "residential",
      "desc": "residential manufacture",
      "parent": "businessLine"
    },
    {
      "key": "mumbai",
      "desc": "mumbai location",
      "parent": "facility"
    }
  ]
}
```

### Dataweave Code
```dw
%dw 2.0
output application/json
var temp={}
fun rec(value)= value mapObject ((val, key, index) ->if (typeOf(val)~="Object") {"parent":(key) } ++ rec(val) else {(key):(val)})
var pattern=[rec(payload
