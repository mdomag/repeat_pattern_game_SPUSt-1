**A simple pattern repetition game designed for NI USB-6008+SPUSt-1. It can be used as a scheme for a similar device.**

NI USB-6008 is a data acquisition device, whereas SPUSt-1 is an actual device used here for playing. This project utilizes interactive elements of SPUSt-1: 2 rows of 3 colored diodes each, a row of 3 buttons and a knobe.

This project is divided into subVIs, main program is located in the projekt.vi file - open it to run the whole game. Upon starting the program, user can choose the display time - this modifies the number of points given per right move. To start a game, user has to click the green button. The game displays a pattern on colored diodes. Using buttons, user has to correctly repeat a previously displayed sequence to score a point. After losing, a special sequence is displayed and a score is written onto a file chosen by the user. The game also displays the top 10 scores in descending order.

I realized this project using a state machine. To give a quick rundown of what each state does:

- Initialize: starts the task for each of the physical channels used here

- Start - loops over until user clicks the green button (port1/line2), reads data from the knobe (ai1), converts it and accordingly sets time and points per move, and also displays a high score list

- Add element - generated random power of 2 (1/2/4) and adds it to the pattern sequence

- Display - loops over until all the elements of the sequence have been displayed, element of a sequence is a number (1/2/4) it gets converted to a boolean array and negated, as this is a fitting input for diodes physical channel (port0/line0:2) since they are activated by a low state [i.e. 2 -> 010 -> 101 -> diode 1 is on]

- Check - loops over until user presses a button, then it checks whether it's a right move by comparing it to the sequence

  - to be more specific: program waits until not a single button is pressed, then waits 300ms to avoid reading data from bouncing and only after that waits for a proper button click
    
- Game result - displays appropriate diode sequence (losing/high score) and writes the score to the file (it prompts the user to choose it if it's not set)

- High score and Game over - display adequate diode sequence

- Stop - clears all the data sent to physical channels and stops all the tasks.
