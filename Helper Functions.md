Addresses from version 1.92F

### Random number function
**rabiribi.exe+7E10**

**Input**
* ECX: Lower bound
* EDX: Upper bound

**Output**
* EAX: Result
* EDX: Distance from lower bound

**Example:**
```assembly
mov ecx,-10
mov edx,10
call rabiribi.exe+7E10
```

* EAX will contain a number between -10 and 10
* EDX will contain a number between 0 and 20

### Effect: Get closest object
**rabiribi.exe+8A250**

This function takes an effect index as its only parameter. Effect indexes are most easily accessed in the script loop where the current effect index is stored in the stack.

**Input**
* ECX: Effect index

**Output**
* EAX: Closest object index
* ECX: Effect memory offset
* EDX: Closest object index

### Effect: Get distance and angle TO object
**rabiribi.exe+CB20**

This function takes an effect index and an object index and gets some measurements from the effect to the object.

**Input**
* ECX: Effect index
* EDX: Object index

**Output**
* XMM0: Angle to object
* XMM1: X distance to object
* XMM2: Y distance to object
