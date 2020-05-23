# LUA Notes

### Comment

* Single line comment
```--```
* Multi-line comments
```
-[[
    Annotate
-]]
```
Or
```
-[[
    Annotate
  ]]
```
* **Multi-line comments will end prematurely when encountering the string represented by [[and]], you can use ---- [= [comment content] =] to solve**
```
-[= [
    Annotate
  ] =]
```





### Chunk chunk

##### lua syntax-chunk
* The lua interpreter processes lua code in the form of program blocks
* Each piece of executable lua code can become a program block
* lua block refers to one or more legal executable statements
* A program block consists of one or more lua statements
* Simple program block: one statement
* Complex program blocks: multiple different statements and function definitions
**The semicolon at the end of the statement defaults, such as a line of Togo statement, it is recommended to separate by semicolon**





### type of data

##### table (create different data types: arrays, dictionaries, etc.)
* table data structure itself supports polymorphism, flexible definition, is a mixture of arrays and collections
* table uses an associative array, you can use the value of the art type as an index, but the value cannot be nil
* The table is not fixed in size, you can be angry that you need to expand
* lua module (mobule), package (package), and object (Object) are also based on table

```
t = {
    "a",
    "b",
    "c",
    [4] = "d",
    [5] = "f",
    [6] = function () {}
}
```

##### Table operation

| Operation | Method | Use |  
| :----: | :---- | :----------- |  
| Connection | table.concat (table [, sep [, start [, end]]]) | concat is the abbreviation of concatenate (chain, connection). The table.concat () function lists the array part of the specified table in the parameter All elements from the position to the end position are separated by the specified separator (sep). |  
| Added | table.insert (table, [pos,] value) | Insert an element of value at the specified position (pos) in the array part of the table. The pos parameter is optional, and the default is the end of the array part. |  
| ~~ maxn ~~ | ~~ table.maxn (table) ~~ | Specifies the largest key value among all positive key values ​​in the table. If there is no element with a positive key value, 0 is returned. (This method no longer exists after ~~**Lua5.2**~~) |  
| Remove | table.remove (table [, pos]) | Returns the element of the table array part at pos. The elements after it will be moved forward. The pos parameter is optional, the default is the length of the table, which is deleted from the last element Up. |  
| Sort | table.sort (table [, comp]) | Sorts the given table in ascending order. |  





### Variable

* **The default variable is a global variable**, including variables within the method, if you set a local variable, add the modifier local (here it is easy to cause exceptions and bugs, use with caution,**use local variables as much as possible**) Benefits of using local variables:
    -Avoid naming conflicts and overwriting of variables
    -Faster access to local variables
    -Avoid logic error defects

* Global variable deletion: only need to assign the variable value to nil.





### Operator

##### calculating signs
* Relational operator: ```<``` 、```>```、```<=``` 、```>=``` 、 ```==``` 、 ```~=```(**Not equal**)
* Logical operator: ```and or not```
* Join operator: (two dots)

##### Operational considerations
* "0" and 0 (false, not equal)
* nil is only equal to nil itself
* The logical operators false and nil are false (false), others are true (including 0 is also true)
* The operation results of and and or are not true and false, but are related to two operands:
    * ```a and b```-if a is false, then return a, otherwise return b
    * ```a or b```-if a is true, then return a, otherwise return b
* Lua compares numbers according to the traditional number size, and compares strings in alphabetical order (but the alphabetical order depends on the local environment (character encoding or other order is different)





### Function

* ```()``` Brackets are only used in functions, other places can be ignored by default





### Loop

##### while
```
while (condition)
do
   statements
end
```

##### for
```
for var = exp1, exp2, exp3 do
    -
end
```

##### repeat
```
repeat
   statements
until (condition)
```

##### break and goto

Goto syntax format: ```goto Label```; The format of Label is``` :: Label :: ```
```
local a = 1
:: label :: print ("--- goto label ---")

a = a + 1
if a <3 then
    goto label-jump to label label when a is less than 3
end

```

##### continue is not officially built and can be implemented in other ways

Reasons for not providing continue:    
Lua-FAQ:  
This is a common complaint. The Lua authors felt that continue was only one of a number of possible new control flow mechanisms (the fact that it cannot work with the scope rules of repeat / untilwas a secondary factor.)  

##### Several ways to simulate "continue" in lua:

* Use the repeat loop to wrap the code that needs to continue to skip, and use break to jump out of the loop. It should be noted that the repeat statement in lua exits when the loop condition is true
```
for i = 1, 10 do
    repeat
        if i% 2 == 0 then
            break
        end
        print (i)
        break
    until true
end
```

* Use the while loop to wrap the code that needs to continue to skip, use break to jump out of the loop
```
for i = 1, 10 do
    while true do
        if i% 2 == 0 then
            break
        end
        print (i)
        break
    end
end
```

* After lua5.2 version, you can use goto statement to simulate
```
for i = 1, 10 do
    if i% 2 == 0 then
        goto continue
    end
    print (i)
    :: continue ::
end
```