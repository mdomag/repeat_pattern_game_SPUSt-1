A simple pattern repetition game designed for NI USB-6008+SPUSt-1. It can be used as a scheme for a similar device. 
NI USB-6008 is a data aquisition device, whereas SPUSt-1 is an actual device used here for playing. This projects utilizes interactive elements of SPUSt-1: 2 rows of 3 colored diodes each, row of 3 buttons and a knobe.

Project is divided into subVIs, main program is located in the projekt.vi file - open it to run the whole game. 
Upon starting the program user can choose display time - this modifies the amount of points given per right move. To start a gamne user has to click the green button. The game displays a pattern on colored diodes. Using buttons, user has to correctly repeat previously displayed sequence to score a point. After loosing a special sequence is displayed and score is written to a file chosen by the user. The game also displays top 10 scores in the descending order. 

I realised this project using a state machine. To give a quick rundown of what each state does:

 - Initialize: starts the task for each of the physical channels used here
    
 - Start - loops over until user clicks the green button (port1/line2), reads data from the knobe (ai1), converts it and accordingly sets time and points per move, also displays high score list
    
 - Add element - generated random power of 2 (1/2/4) and adds it to pattern sequence
    
- Display - loops over until all the elements of the sequence had been displayed, element of a sequence is a number (1/2/4) it gets converted to a bool array and negated as this is a fitting input for diodes physical channel (port0/line0:2) since they are activated by a low state [i.e. 2 -> 010 -> 101 -> diode 1 is on]
    
 - Check - loops over until user pressed a button, then it checks whether it's a right move by comparing it to sequence
    
   - to be more specific: program waits until not a single button is pressed, then waits 300ms to avoid reading data from bouncing and only after that waits for a propper button click
        
- Game result - displays appropriate diode sequence (loosing/high score) and writes score to the file (it prompts the user to choose it if it's not set)
    
 - High score and Game over - display adequate diode sequence
    
 - Stop - clears all the data sent to physical channels and stops all the tasks.



