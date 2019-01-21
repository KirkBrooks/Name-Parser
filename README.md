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
 A partnership is an entity of two people. Each per






Note: A current limitation of this parser is the assumption the last name (family name) will be the last name(s) in the string. 

