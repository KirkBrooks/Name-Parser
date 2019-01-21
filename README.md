# Name Parser
I have been working on various approaches to managing names in my 4D databases for a long time. Names are tricky. Getting them right is difficult because of spelling errors, identifying exactly what the name refers to (eg. 'Walt Disney'), handling couples or partnerships, recognizing preferred and nick names and so on. If you have ever created a database that requires records for people and businesses you know the problems. Even relatively simple situations become complicated over time. Imagine a person whose name changes as the result of marriage. A time or two perhaps. How will you manage those changes? Many times you need or want to track alternate spellings of names. Then there are the prefix and suffixes to manage. Finally, if you are tracking customer or vendor names you've got he issue of the 'dba' or 'doing business as' name. 

The purpose of this component is to provide a single field I can include on a form for entering and displaying the name of a person, company or couple. 

Entities
---
I use the term "entity" for each name object and the idea is I'm dealing with the name of a legal entity. For simplicity each entity is considered to be a person, company or partnership. A partnership is two people. 

### Companies
 Company names are stored as they are entered and typed. The name is referred to as the 'corporate name'.
### Person
 A person may have a first, middle and last name, a preferred name and lists of alternate first and last names. 
### Partnerships
 A partnership is an entity of two people. Each person has their own name object.
 
 ## The Entity Object
 ```NameStr_parse_toEntityObj``` returns an entity object. For example: 
 
 ``` $obj := NameStr_parse_toEntityObj("kirk brooks") ```
 
will populate $obj as:
```
{
  "_ID": "C06715B08E5E485D8DAF8DA44AC248A0",
  "name": "Brooks, Kirk",
  "entityType": 0,
  "dba": "",
  "email": "",
  "members": [
    {
      "type": 0,
      "first": "Kirk",
      "middle": "",
      "last": "Brooks",
      "prefix": "",
      "suffix": "",
      "gender": "",
      "preferred": "Kirk",
      "altFirst": [],
      "altLast": [],
      "name": "Kirk Brooks"
    }
  ],
  "strInput": "kirk brooks",
  "runTime": 22,
  "showAlt": 1
}
```
Note: A current limitation of this parser is the assumption the last name (family name) will be the last name(s) in the string. 

The entity properties: 
  * \_ID: a uuid generated to identify the entity
  * name: the default name presentation, last name, first
  * entityType: 0=individual; 1=partnerhsip; 2=company
  * dba: "doing business as" name of the entity
  * email
  * members: array of name objects
  * strInput: text string parsed
  * runTime: milliseconds used by parsing
  * showAlt: flag used by detail form

The \_ID property is assigned to ensure each entity is uniquely identified. Feel free to overwrite it with your own id. 

The members array contains the name object of the person, company or members of a partnership.

The Parser
---
The parser attempts to identify the entity type of the string and then parse the names correctly. First it checks to see if the string looks like a business. This is a difficult call to make. ```NameStr_is_bizName``` analyzes the words in a string to see if they appear in a list of 2800+ words commonly found in business names. This is very effective for names containing words like 'company', 'inc.', 'corp.'. 'contruction' and so on. It's less accurate when a company name resembles a person name: 'Walt Disney', 'JP Morgan'.

Partnerships are assumed if the name contains '&' or the word 'and'. Absent either of these distinctions the string is parsed as a person. 

Entity and name properties can be explicitly identified. 

Explicitly identifying properties
---
A word can be explicitly identified as a particular property using the following:

| Entity Property | Chars | Example Input |
| --------------- | ----- | :------ |
| entityType | =0, =1, =2 | =2 Walt Disney|
| entityType | $ (first char) | $walt disney | forces entity to company
| dba | dba | joe blow dba Mongo Enterprises|
| dba | [dba name] | joe blow [Mongo Enterprises]|

The properties of a name object that can be identified: 

| Name Property | Chars | Example Input | Comment |
| ------------- | ----- | :------ | :------ |
| last | /name/ | kirk /brooks/ | this is also seen in gedcom files |
| last | ... , | smith jones, john | 'smith jones' is the last name |
| prefix | < | <Mr. kirk brooks | 
| suffix | > | kirk brooks >MD | 
| preferred | ~ | james smith ~JD | 'JD' is his preferred name
| preferred | ! | james !jonathan smith  | 'Jonathan' is his preferred and middle name

Words enclosed in double quotes are treated as a single phrase. 

## Alt Names

Alternate spellings of names are designated with parenthesis. 

` John Smith (Smyth) ` or ` John (Jon) \Smith (Smyth)\ `

Editing an entity
---
```Entity_edit_form``` provides an interface to easily edit an entity object. For example running this code:

```	
 $str:="kirk Brooks & mary smith"
 $obj:=NameStr_parse_toEntityObj ($str)
 Entity_edit_form ($obj)
```

results in this form being displayed: 

![alt text](https://github.com/KirkBrooks/Name-Parser/blob/master/images/scrnshot_1.png "The Entity Edit form.")

This form allows the user to edit and modify the entity object. 

