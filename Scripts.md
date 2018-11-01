Scripts modify effects that have a behavior set to 0x15. When the behavior is 0x15, Var 4 is responsible for which script to use. Scripts are accessed from a tree and have wildly varying IDs. 

Every frame, the effect array is iterated over. If any effects have the script behavior, Var 4 is taken and the tree is searched. If a match is found, the program jumps to the script's memory location and the script is used.

At the very least, a script must contain the exit jump:
```assembly
jmp rabiribi.exe+25390A
```


### Accessing Current Effect

The script loop stores the current effect's index in the effect array at [EBP-2C]. Because effects are 0x94 bytes long, the effect's memory offset can be found by multiplying the index by 0x94. After that, adding the effect array pointer will give the exact location of the effect in memory. The effect's properties can be found in the [Effect Properties](https://github.com/D3nD3nD3n/rbrb/blob/master/Effect%20Properties.md) file.

This is the game's standard format for getting an effect's memory location:
```assembly
imul ebx,[ebp-2C],00000094 	        //store offset * 0x94
add ebx,[rabiribi.exe+12E8F8C] 		//add effect array pointer
```
The compiler is very inefficient with this and will frequently get the effect's memory location multiple times in different general registers instead of storing it in EBX. I recommend using EBX for the effect memory location because none of the helper functions or arithmetic instructions make use of EBX.
### Using Scripts

Anything can go inside of a script. Scripts can modify player information, the effect information it is tied to, map information, enemy information, etc. Here is a simple script that increases an effect's angle by 5 every frame:
```assembly
imul ebx,[ebp-2C],00000094 	        //store offset * 0x94 in EBX
add ebx,[rabiribi.exe+12E8F8C] 		//add effect array pointer
mov eax,5                               //Move 5 into eax
movd xmm0,eax                           //Move eax's value into XMM0
cvtdq2ps xmm0,xmm0                      //Convert the int in XMM0 to a float and store it in XMM0
addss xmm0,[ebx+0C]                     //Add the effect's current angle into XMM0
movss [ebx+0C],xmm0                     //Move the new angle to the effect's angle memory location
jmp rabiribi.exe+25390A                 //Jump out of script
```
