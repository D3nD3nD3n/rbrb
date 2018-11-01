Even though the create effect function only takes 17 parameters, effects in memory have 37 sets of 4 bytes.

Properties in italics should not be modified.

Hex offset | Property | Notes 
----------:|----------|------
00 | X Position | Float
04 | Y Position | Float
08 | Velocity | Float
0C | Angle | Float
10 | Size | Float
14 | Horizontal Squish | Float; Default: -10000; Stretches sprite
18 | ??? | Int; Default: 999
1C | Sound | Int
20 | Collision Type | Int; Determines which objects it collides with
24 | Lifetime | Int; How many frames have passed since creation
28 | Transparency | Int; 0-255/0-FF
2C | Behavior | Int
30 | Sprite | Int
34 | *Global Effect Tick* | Int; Which frame the effect was created on
38 | Damage | Float
3C | Freeze effect | Int; Effect freezes while > 0, decrements automatically
40 | Y Hitbox | Float; Default: 1; modifies hitbox height
44 | X Hitbox | Float; Default: 1
48 | Miscellaneous | Free memory
4C | Kill Effect | Int; Default: 0; Destroy effect if > 0
50 | Miscellaneous | Free memory
54 | Miscellaneous | Free memory
58 | Miscellaneous | Free memory
5C | Miscellaneous | Free memory
60 | Miscellaneous | Free memory
64 | Miscellaneous | Free memory
68 | Miscellaneous | Free memory
6C | Miscellaneous | Free memory
70 | Var 5 | 
74 | Var 4 | 
78 | Var 3 | 
7C | Var 2 | 
80 | Var 1 | 
84 | ??? | Default: 0
88 | Layer | Int
8C | ??? |
90 | *Effect ID* | Int; Unique identifier, do not modify
