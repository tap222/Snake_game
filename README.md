# Snake_game

15-110 Lecture Topics:  Snake
Week #9  Oct 18 – Oct 22

    Introduction
    In this self-paced tutorial, you will learn how to write the game of Snake using Python and Tkinter (it is assumed you are already familiar with each).  In the game of Snake, the player uses the arrow keys to move a "snake" around the board.  As the snake finds food, it eats the food, and thereby grows larger.  The game ends when the snake either moves off the screen or moves into itself.  The goal is to make the snake as large as possible before that happens.

    If you are unsure of this description of the game, or just curious where we are headed with our design, you can run the final version (snake8.py).  Play with it a bit.  That's our goal.  Let's get to it!!!!
     
    Our Design
    There are many ways to go about writing Snake.  So how are we going to do this?  We'll choose a way that is fairly easy to understand (knowing that there are other approaches that are, for example, more efficient).  In our game, we will use a  2d list of integers called the snakeBoard.  While the board we display shows colors for the snake's body and for food, the snakeBoard will hold integer values corresponding to snake body parts and the food.  For example, consider this situation:

    SnakeArray

    In this picture, the snake is blue and the food is green.  Of course, while we're playing the game, we do not display these numbers (though for debugging purposes, you can turn them on or off by pressing 'd').  The numbers are only included here to help explain how the snakeBoard works.  So what do the values mean?  Here are the rules regarding the values in the snakeBoard:

    1)  0 means the cell is empty (and so the corresponding board cell is white).
    2)  -1 means the cell holds food (and so the corresponding board cell is green).
    3)  Positive values mean the cell holds part of the snake (and so the corresponding board cell is blue).
    4)  The head of the snake is the largest value in the entire list.  In the example, the head of the snake is the 9.  (Note that we could have made this the tail, instead, but we chose to do it this way.)

    Why is the head of the snake important?  Because the snake moves forward from its head!

    Ordinarily, at the start of the game the snake's body is just one cell.  However, it turns out to be helpful while writing the game to start with a larger snake already pre-created.  So we'll do that, and later on, once our game is working, we'll fix that.  So let's get started!
     
    Snake Task #1:  Create and Display the Initial Snake
    Start: 	events-example0.py and snake0.py
    Goal: 	Write an init function (and several helper functions) so that the program creates the snakeBoard like this (with positive values highlighted for clarity):

        snakeBoard = [ [ 0, 0, 0, 0, 0, 0, 0, 0, 0, 0 ],
                       [ 0, 0, 0, 0, 0, 0, 0, 0, 0, 0 ],
                       [ 0, 0, 0, 0, 0, 0, 0, 0, 0, 0 ],
                       [ 0, 0, 0, 0, 4, 5, 6, 0, 0, 0 ],
                       [ 0, 0, 0, 0, 3, 0, 7, 0, 0, 0 ],
                       [ 0, 0, 0, 1, 2, 0, 8, 0, 0, 0 ],
                       [ 0, 0, 0, 0, 0, 0, 9, 0, 0, 0 ],
                       [ 0, 0, 0, 0, 0, 0, 0, 0, 0, 0 ],
                       [ 0, 0, 0, 0, 0, 0, 0, 0, 0, 0 ]
                       [ 0, 0, 0, 0, 0, 0, 0, 0, 0, 0 ]
                     ]

    And then draws the initial board like this:

    Snake1

    Printing these instructions:

    Snake!
    Use the arrow keys to move the snake.
    Eat food to grow.
    Stay on the board!
    And don't crash into yourself!

    Here is the init function we used:

    def init(canvas):
        printInstructions()
        loadSnakeBoard(canvas)
        redrawAll(canvas)

    Of course, you have to write these three functions.  The first function just prints the instructions to the console window (not ideal, but good enough for our purposes).  The second function, loadSnakeBoard, loads the 2d list representing the board into canvas.data["snakeBoard"].  The third function, redrawAll, draws the grid and the snake body based on the values in the snakeBoard.

    With this description, see if you can start from snake0.py and write snake1.py.  That said, you should feel free to consult the solutions as needed during this tutorial (that's why they are provided, after all).
    Solution: 	snake1.py

     
    Arrow Keys (Snake Movement without Error Checking)
    This is the largest, and most complicated, step in writing this program.  Good luck!
    The next step is for the snake to start moving in response to the arrow keys.  To get this working, we'll write the moveSnake function.  This function takes the direction that we want to snake to move.  How do we specify a direction?  There are several good choices, but we'll use this one:  the function will take two paremeters -- drow and dcol -- signifying the change in the row and the change in the col for one step in that direction.  For example, to take one step to the right on the board, the row stays the same and the column increases by 1.  So, when heading to the right, drow is 0 and dcol is +1.  Similarly, to take one step up, the row decreases by 1 and the col stays the same.  So, when heading up, drow is -1 and dcol is 0.  This may seem complicated, but in fact it helps keep our code cleaner!

    Next, let's consider how we'll move the snake.  Say the snake is of length 9, as in the first example above.  To move the snake, we'll first place a 10  in the next cell in the desired direction.  So, temporarily, the snake is of length 10.  Then we'll shrink it back down to 9 by removing the old tail.  How?  For this, we'll use the standard nested for loops to visit every value in the snakeBoard list, and if that value is positive (part of the snake), then we'll subtract 1 from it.  This turns that 10 into a 9, the 9 into an 8, and so on... Finally, the old snake's tail, which was a 1, becomes a 0, which means that it is no longer part of the snake.  And that is how the snake "moves" forward.  You may need to think about this a bit for it to make sense.  You can also look at our solution, of course, to better understand the details.  So let's do it!
     
    Snake Task #2:  Arrow Keys (Snake Movement without Error Checking)
    Start: 	snake1.py
    Goal: 	Hint:  It's a good idea to start from our solution to the previous step (snake1.py, in this case), even if you solved it yourself!  This way, you will start each section consistently with the tutorial.

    Continuing from the last step, add snake movement in response to arrow keys.  This will be accomplished in several steps:
        Make two variables -- headRow and headCol -- that keep track of where the snake's head is.  We'll need that for movement!  Store these in canvas.data.

        Write the function findSnakeHead.  This function should find the snake's head in the snakeBoard. That is, it should set headRow and headCol (in canvas.data) so that snakeBoard[headRow][headCol] is the largest value in the list.
         
        Call the findSnakeHead function from the loadSnakeBoard function (after it sets the snakeBoard).
         
        Write the function removeTail.  This function subtracts 1 from every positive value in the snakeBoard list, as described above.
         
        Write the function moveSnake:

          def moveSnake(canvas, drow, dcol):

        This function moves the snake one step forward in the given direction.  For now, the moveSnake function does not worry about whether or not the snake can legally move in this direction!  It just moves there.  To do this, the function should:
            Compute the location of the newHeadRow and newHeadCol :

                newHeadRow = headRow + drow;
                newHeadCol = headCol + dcol;

            Make the snake grow by 1 (in the example above, this is where the snake briefly stretched from 9 to 10) by setting the snakeBoard value for the new head to be one larger than the old head's value.
             
            Update the headRow and headCol values in canvas.data to their new values.
             
            Call the removeTail function you just wrote, which gets rid of the old tail and makes the snake shrink back to its correct size (and thus appear to move forward one step in the given direction).  
             
        Now the snake can move, but only in theory -- the user has no way to make it move!  To fix that, write the keyPressed function so that up, down, left, and right call the moveSnake function with the appropriate values for drow and dcol.  While you're at it, quit the game if the user presses 'q', and restart it if the user presses 'r'.  (Again, if you're stumped by any of this, check out the solution to this step -- snake2.py -- provided below.)

    When you are done:  Test your program by moving the snake with the arrow keys.  It moves!   Wow!!!  Excellent!!!!

    Note: your snake will not yet move on its own.  That happens later.  For now, it will only move in response to arrow keys.
    Solution: 	snake2.py

     
    Snake Movement with Error Checking
    So that was exciting!  And while there are several more steps to go, you've written well over half the code already, including most of the hard parts.  Great job!!!  However, we're still not quite done with snake movement.  To see this, run your program and make the snake run into itself.  It doesn't die!  Even worse, make the snake crawl off the board.  Not only doesn't the snake die, but your program crashes!  Oh no!  (Why does it crash?  Because you are trying to access values outside the legal bounds of the snakeBoard list.  Whoops.)  So we'll fix that now.
     
    Snake Task #3:  Snake Movement with Error Checking
    Start: 	snake2.py
    Goal: 	Continuing from the last step, change the moveSnake function so that it deals with the two error cases that can occur during snake movement.
        If the snake would move off the board (that is, if the new headRow or headCol would be negative or would be too large), do not move the snake, but just call gameOver.
         
        Also, if the snake would crash into itself, once again do not move the snake, but just call gameOver.

    To do this, you'll also have to move the code that already was in the moveSnake function into an "else" clause.  This should become clear as you do the task.

    That's it.  Now you have snake movement with proper error checking.  Good job!

    Test your snake by making it move into itself.  Also, crawl off the board in each of the four directions (sometimes code works for some directions but not others, so test all of them!).
    Solution: 	snake3.py

     
    Food Placement
    Now that our snake moves, it needs food so it can eat and grow.  Now, we want to place food randomly on the board.  This should be easy:  just pick a random row and a random col and we're done.  Not so fast!  We do not want to place the food on the snake.  So we have to keep picking random locations until we find one where the snake is not.   And so we shall!
     
    Snake Task #4:  Food Placement
    Start: 	snake3.py
    Goal: 	Continuing from the last step, we'll add random food placement as follows:
        Write the function placeFood.  This function will place food (-1) in a random location on the snakeBoard, but keep picking random locations until we find one that is not part of the snake!  Then it sets that location in snakeBoard to -1, meaning food is there.  Hint:  to do this, use random.randint(a,b), which returns a random integer between a and b inclusively.  Don't forget to import random.
         
        Modify the loadSnakeBoard function to call the placeFood function.
         
        Now that we can place food, we should also add it to the visible board!  To do this, modify the drawSnakeBoard function so that it not only checks each cell for the snake (positive values), but now it also checks for food (-1), and sets the visible board to green in that case.

    When you're done, you should see green food randomly placed on the board at the start of the game.  Now, we haven't implemented eating food, so if you try to eat it, it simply disappears and that's that.  No worries (for now).  But if you want to test your code, you can repeatedly press "r" to reset the game, and you should see the food appearing in random locations on the board (and not on the snake).  Voila!
    Solution: 	snake4.py

     
    Eating Food
    Now that we have food, we should let the snake eat it!  This is surprisingly straightforward at this point, so let's just do it.
     
    Snake Task #5:  Eating Food
    Start: 	snake4.py
    Goal: 	Continuing from the last step, we'll add eating food simply by changing the moveSnake function. and nothing else!  In that function, right after the case where you check if the snake will run into itself (that is, if the snakeBoard contains a positive number in the new location), add a case (an "elif" clause) that checks if the snake is about to eat food (-1 in that location).  Then, if the snake is eating food, do the following:
        Expand the snake by 1 just like in the normal movement case.
         
        Update the headRow and headCol just like in the normal movement case
        .
        Do not call removeTail here.  The snake actually grows in this case!
         
        Since we just ate the food, call the placeFood function to place food in a new random location. 

    That's it!  To test this, move the snake up to food then slowly make sure that the snake grows properly when it eats the food, and that food appears at some other random location at that time.  Sweet!
    Solution: 	snake5.py

     
    Animation (Timer-Based Movement)
    It's time for our snake to come alive and to move on its own!  Once again, this step takes heavy advantage of the great code you've already written, so it's not that difficult.  As with all animations, we will use the timerFired function.  And in that function we'll call moveSnake, which we've already written.  The only hard part is this:  what direction should we move the snake?  Well, the direction it last moved in.  Right?  So we need to remember that.  And how do we remember values between function calls?  By storing values in canvas.data!  So we need two new values:  canvas.data["snakeDrow"] and canvas.data["snakeDcol"].

    Why these names?  Remember:  we store directions in this game using drow and dcol.  Now we have to set those values.  Where do we do that?  In moveSnake!  Every time we move the snake, we'll simply remember which direction we moved it in.  That way, when the step function is called, we'll just use those values and keep heading in the same direction.  That's it.

     
    Snake Task #6:  Animation (Timer-Based Movement)
    Start: 	snake5.py
    Goal: 	Continuing from the last step, we'll add animation (timer-based movement) as follows:
        Add the snakeDrow and snakeDcol values to canvas.data as described above.
         
        In loadSnakeBoard, we need to set an initial direction.  We'll choose to head left initially, so in that function set snakeDrow to 0 and snakeDcol to -1 (see why?).
         
        In the moveSnake function, store the direction of the move in these values in canvas.data.
         
        Finally, in the timerFired function, call moveSnake, using the values of snakeDrow and snakeDcol that we stored when we last moved the snake.

    That's it!  And when you get this working, suddenly the game springs to life!!!!  While we'll make a couple tiny improvements, now you have a really solid Snake game!!!!!  Excellent!!!
    Solution: 	snake6.py

     
    Variable-Sized Game and Single-Cell Snake To Start
    At this point, we'll add two changes that make sense to do together.  Right now, the size of the snakeBoard is hardwired to be 10x10.  So our game must be 10x10  See?  We'll change this to be a variable-sized list filled with 0's.  Great!  Also, now we'll make the snake one cell large at the start.  Do you see why this is a good time to make that change, too?  So let's do it.
     
    Snake Task #7:  Variable-Sized Game and Single-Cell Snake To Start
    Start: 	snake6.py
    Goal: 	Continuing from the last step, change the loadSnakeBoard function in two ways:
        Change the way the snakeBoard is created so that it is variable-sized.  Change the run function so that it takes rows and cols as parameters, and stores these in canvas.data. To test this, make the board 8 row by 16 columns by calling run(8,16).
         
        Place the snake in the middle of the board by setting the middle cell of the snakeBoard equal to 1.  (Do you see why that has the desired effect?)

    When you're done, you program should look like this:

    snake7

    And now we've got a really solid Snake game!  Just one last change....
    Solution: 	snake7.py

     
    Funny Timing After Key Presses
    The way the game is written now, if you press an arrow key just before the timer event, you get two very rapid moves back-to-back.  This is not ideal.  One way to fix this is to ignore the next timer event after a key press event (why?).  How do we do that?  We introduce a new value in canvas.data, ignoreNextTimerEvent, which will be a boolean value (True or False).  We'll have our keyPressed function use that variable to indicate that the next timer event should be ignored, and then our timerFired function will check that variable, and if it's True, the timerFired function will do nothing at all (hence, ignore the event).
     
    Snake Task #8: Funny Timing After Key Presses
    Start: 	snake7.py
    Goal: 	Fix the funny timing problem after key presses, as follows:
        Add a value to canvas.data named ignoreNextTimerEvent.
         
        Set this to False in loadSnakeBoard (right?).
         
        In the keyPressed function, set the variable to True.
         
        In your timerFired function, only call moveSnake if ignoreNextTimerEvent is False (since if it is True, we should ignore the event by not calling moveSnake!).  Then, in any case, set ignoreNextTimerEvent to False (we don't want to ignore the next timer event!).

    Also, as a small aside, I changed the timer to 150 milliseconds at this point, since I think that leads to slightly more fun game play.
    Solution: 	snake8.py

     
    More Features
    We have written a fun-to-play game of Snake.  However, much work remains before this could be considered a complete application.  For example, we might add a score, a high score list, levels, save-game and restore-game, a splash screen at startup, a help screen, a pause screen, sounds, images, buttons, menus, etc, etc, etc, not to mention packaging our app as an installable program or for use in a web browser.  We will cover some of these issues later in this course, and others are left for you and your ingenuity (and your Google searches!).

    Summary
    So now you've written Snake!!!  Way to go!!!  You have earned your stripes, and you are now totally ready to move on to... Tetris!  Kudos!!!
