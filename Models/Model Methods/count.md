# .count( `criteria` , `criteria` , [`callback`] )
### Purpose
Returns the number of records in your database that meet the given search criteria.

### Overview
#### Parameters

|   |     Description     | Accepted Data Types | Required ? |
|---|---------------------|---------------------|------------|
| 1 |    Find Criteria    | `{}`,`[{}]`, `string`, `int`  | Yes |
| 2 |     Callback        | `function`          | No        |

#### Callback Parameters

|   |     Description     | Possible Data Types |
|---|---------------------|---------------------|
| 1 |  Error              | `Error`             |
| 2 |  Number of Records  | `int`               |

### Example Usage

```javascript 
User.count({name:'Flynn'}).exec(function countCB(err,found){
  console.log('There are '+found+' users called \'Flynn\'');
  });
  
// There are 1 users called 'Flynn'
// Don't forget to handle your errors

```
### Notes
> Any string arguments passed must be the ID of the record.



<docmeta name="uniqueID" value="count42579">
<docmeta name="displayName" value=".count( `criteria` , `criteria` , [`callback`] )">

