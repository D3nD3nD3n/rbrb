Addresses from version 1.92F

Effects are any type of moving object that lacks a health bar. Bullets, particles from landing, damage numbers, and the ribbon that follow Erina around are all effects. The basic movement of effects is done through a vector, but this can be modified by scripts or other programmed behavior. Effect assets are located in \gfx\eff and \gfx\obj\eff.


### Create Effect Function
The function responsible for creating effects is located at rabiribi.exe+13C90. The create effect function requires 17 different values with 13 of them being on the stack. The stack should have the following values in FILO order:

1. Y Position (Float)
2. Size (Float)
3. Speed (Float)
4. Angle (Float)
5. Transparency (Int)
6. Behavior (Int)
7. Sprite (Int)
8. Layer (Int)
9. Var 5 (Int)
10. Var 4 (Int)
11. Var 3 (Int)
12. Var 2 (Int)
13. Var 1 (Int)

The following registers should hold the remaining variables:

**XMM0:** Damage (Float)

**XMM3:** X Position (Float)

**ECX:** Sound (Int)

**EDX:** Effect Type (Int)

### Variable Details
Some variables can be easily explained while others just require testing.

**Angle:** Angle is in degrees.

**Transparency:** Transparency is from 00 to FF.

**Behavior:** Behavior is one of the variables that requires testing. It is seemingly values 0-114. The behavior variable is used for common behaviors such as ribbon's attacks.

**Sprite:** Sprite requires testing. Tested values as high as 203, but suspect 0-255. Some sprites are duplicated.

**Layer:** Sprite layer to draw on. Greatest success with layer 7.

**Var 1-5:** These variables inconsistently modify the effect depending on other values in the effect. Sprite 10's sprite is modified by var 4, while sprite 9 is modified by var 1. Behavior can also be modified and there can be overlap between sprite and behavior modifications.

**Damage:** This is the final damage amount that will be dealt. 

**Effect Type:** Determines collision type. 0 = no character interaction, 1 = collision with enemy, 2 = collision with player. All attacks from enemies use effect type 2.

**Sound:** I forget what this does. Oops.

### Standard Effect Building

The standard format for bullet building is to push the integers (5-13 on the stack list), and then move the floats in. XMM registers lack a push instruction, so they must be "pushed" on the stack through the movss instruction. This is the standard format for effect creation seen in disassembly:

```assembly
push 00 						//FINAL var 1
push 00 						//FINAL var 2
push 00							//FINAL var 3
push 00							//FINAL var 4
push 00 						//FINAL var 5
push 07 						//FINAL layer
push 01 						//FINAL sprite
push 01 						//FINAL behavior
push 000000FF						//FINAL transparency
sub esp,10						//move stack pointer
movd xmm0,180						//move angle
cvtdq2ps xmm0,xmm0					//convert angle from int to float
movss [esp+0C],xmm0					//FINAL angle
mov [esp+08],40000000					//FINAL speed
mov [esp+04],40000000					//FINAL size
mov ebx,[rabiribi.exe+1308B20] 				//move player pointer
movss xmm0,[ebx+10]					//move player y pos
movss [esp],xmm0					//FINAL y pos
movss xmm3,[ebx+C]					//move player x pos	
movss xmm0,40000000					//move damage
mov ecx,01						//move sound
mov edx,00000001					//move effect type
call rabiribi.exe+13C90					//create effect function
add esp,34						//reset stack pointer
```

Instructions commented as FINAL are where values are put on the stack. The final `add esp,34` instruction after the create effect fuction call is required.

Cicini's basic green shot start at rabiribi.exe+10A469. This is a good starting point for tinkering. Var 4 should be set to 0 when working with the attack because it modifies behavior 1B, which is the scripting behavior.
