# Tutorial

The completed code is in the file `PIG`.

*__Note:__ Indentation in the code snippets below is just for visibility. DO NOT INDENT.*

#### Step 1: Learn the game
Familarize yourself with the rules of Pig. More information can be found [here](http://cs.gettysburg.edu/projects/pig/).

#### Step 2: Create the TI Program file
Get out your trusty TI-84 and create a new program called "PIG".

#### Step 3: Add a Welcome Message
The first thing that your program should do is clear the current screen.

```javascript
 1| ClrHome
```
Then add the welcome message.

```javascript
 2|
 3| Disp "PIG, A JEOPARDY DICE GAME"
 4| Disp ""
 5| Disp "RACE TO 100 POINTS BY"
 6| Disp "ROLLING DIE REPEATEDLY"
 7| Disp "DURRING YOUR TURN. TURNS"
 8| Disp "ALTERNATE WHEN YOU HOLD"
 9| Disp "OR ROLL A 1 (PIG)."
10| Disp ""
11| Disp "PRESS ENTER"
12| Pause
13|
```

`Pause` is important so that the message is not cleared before the user has the chance to see it!


#### Step 4: Let the Player Choose to Go First or Second

Underneath the welcome message, add a menu so that the player can choose to be player 1 or 2:

```javascript
15|
16| Menu("BE PLAYER 1 OR 2?","1",1,"2",2,"EXIT",4)
17| Lbl 1
18|     1->H
19|     Goto 3
20| Lbl 2
21|     2->H
22|
23| Lbl 3
24| ClrHome
25|
```

On selecting "1", the menu will take the user to label 1 and set the human player number variable `H` to 1. Then `Goto 3` will skip setting `H` to 2. Selecting 2 will skip Lbl 1 and set `H` to 2.  

#### Step 5: Set the Current Player and the scores variables

```javascript
25|
26| 2->C
27|
28| 0->U
29| 0->O
30|
```


We set the current player `C` to 2, but this will be changed at the beginning of the game loop. `U` is the human score and `O` is the computer score.

#### Step 6: Create Game Loop

```javascript
30|
31| While U<100 and O<100
32|    
33|     (3-C)->C
34|
35|     If C=H
36|     Then
37|        Disp "YOUR TURN"
: |        
61|     Else
62|         Disp "COMPUTER'S TURN"
: |       
84|     End
85|
86|     Disp "----COMPUTER SCORE: "+toString(O)
87|     Disp "----YOUR SCORE: "+toString(U)
88| End
89|
```

While the human score and the computer score are less than 100, the game continues and another turn is taken. Line 33 switches players. If the current player is the human player number, it is the human's turn, otherwise it is the computer's turn. At the end of each turn we display the current scores (lines 86-7).



#### Step 6: Program the Human's Turn

```javascript
34|    
35|    If C=H
36|    Then    
37|        Disp "YOUR TURN"
38|        0->T
39|        1->B
40|        While T+U<100 and B
41|            Input "1 TO ROLL, * TO HOLD: ",I
42|            If I=1
43|            Then
44|                randInt(1,6)->R
45|                If R=1
46|                Then
47|                    Disp "YOU ROLL PIG! (T:0)"
48|                    Disp "PRESS ENTER"
49|                    Pause
50|                    0->T
51|                    0->B
52|                Else
53|                    (T+R)->T
54|                    Disp "YOU ROLL "+toString(R)+" (T:"+toString(T)+")"
55|                End
56|            Else
57|                0->B
58|            End
59|        End
60|        (U+T)->U
61|     Else
```

Set the turn total `T` to 0 and set the boolean break flag `B` to true (1). The `While` loop that is started on line 40 will loop every roll, and will be exited when `B` is set to false (0), or the turn total and the human score sum to at least 100.

For each roll loop, take human choice input `I` (line 41).
- If the choice is to roll, generate a random integer `R` between 1 and 6 (line 44).
    - If `R` is a pig, display that message, and `Pause` for user to confirm (this is to ensure that the player sees the message before the computer plays and possibly floods the screen with output)  (lines 47-9). Then set the turn total to 0 and set `B` to 0 in order to break out of the roll loop (lines 50-1).
    - If `R` is not a pig, add the roll amount to the turn total and display what was rolled as well as the new turn total (lines 53-4).
- If the choice is to hold, set `B` to 0 to break out of the loop (line 57).

After the roll loop is exited, add the turn total to the human score (line 60).



#### Step 8: Program the Computer's Turn

```javascript
61|     Else
62|         Disp "COMPUTER'S TURN"
63|         0->T
64|         1->B
65|         While O+T<100 and B
66|             If U>71 or O>71 or T<21+round((U-O)/8)
67|             Then
68|                 randInt(1,6)->R
69|                 If R=1
70|                 Then
71|                     Disp "COMPUTER ROLLS PIG! (T:0)"
72|                     0->T
73|                     0->B
74|                 Else
75|                     (T+R)->T
76|                     Disp "COMPUTER ROLLS "+toString(R)+" (T:"+toString(T)+")"
77|                 End
78|             Else
79|                 Disp "COMPUTER HOLDS"
80|                 0->B
81|             End
82|         End
83|         (O+T)->O
84|     End
85|
```

This computer player acts according to the "keep pace and end race" policy. This policy is summarized by it's creator, [Dr. Todd Neller](http://cs.gettysburg.edu/~tneller),
> - If the player's score i plus the turn total k is greater than or equal to the goal score of 100, hold.
> - Otherwise, if either player has a score greater than or equal to 71, roll.
> - Otherwise, roll if and only if the turn total is less than 21 + round((j - i) / 8).

Other than this policy, the computer's turn is structured similarly to the player's.


#### Step 9: Display Winner

Once the game loop has been exited, current player is the winner. (This is why we switched the current player at the beginning of the game loop instead of the end).

```javascript
 90| If C=H
 91| Then
 92|     Disp "YOU WIN!"
 93| Else
 94|     Disp "COMPUTER WINS!"
 95| End
 96|
 97| Disp ""
 98| Disp "PRESS ENTER"
 99|
100| Pause
101|
```

#### Step 10: Ask to Play Again


```javascript
102| Menu("PLAY AGAIN?","YES",0,"NO",4)
103| Lbl 4
104| ClrHome
```
Right after the welcome message `Pause`, add the following label that this menu can follow, so that a user can restart the game:

```javascript
13|
14| Lbl 0
15|
```
Now you're done and you can play some Pig!
