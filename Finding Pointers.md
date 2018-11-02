### Player Pointer
1. Find and add player life to the address list.
2. Right click player life address, find out what accesses this address. Attach a debugger if asked.
3. Stop the scan in the lower right corner on the new window.
4. Right click the first instruction listed and click "Show this address in the disassembler". The first instruction should be 

`cmp dword ptr [eax+000004D8],00`

5. EAX holds the player pointer, so finding where EAX is last moved into will be the pointer. For the instruction we found, the move into EAX should be 3 lines above the instruction we entered on. For version 1.92F, this will be look like 

`mov eax,[rabiribi.exe+1308B20]`

### Effect Pointer
The Ribbon that follows the player around is always the first effect to be created on screen transition, so finding Ribbon's pointer will also find the start of the effect array.

1. Find Ribbon's x position as a float and add it to the address list. Moving to the right increases this value.
2. Right click the position address, find out what writes to this address. Attach a debugger if asked.
3. Move, then stop the scan on the new window.
4. Right click the only instruction and click "Show this address in the disassembler". The instruction should be 

`movss [edx],xmm0`

5. Directly above that instruction is the standard format for accessing the effect array:

```assembly
imul edx,[ebp-2C],00000094
add edx,[rabiribi.exe+12E8F8C]
```

The brackets in second line contain the effect pointer.
