1. Introduction
1.1 Project Overview:
	This report documents our implementation for the 3rd A.I. mini project titled "Bust the Ghost". Bust the Ghost is a game that we decide to implement in form of a web app using  and where players use a sensor to reveal the proximity of a hidden ghost within a grid. Upon selecting a tile, the sensor assigns a color to indicate the ghost's distance from that cell. Additionally, a direction sensor is integrated and provides information about the direction of the ghost from the selected tile. This feature enhances the gameplay experience by providing players with additional clues to the ghost's whereabouts, contributing to the overall challenge and excitement of the game.
1.2 Game Mechanics:
	Colors such as green signify greater distance from the ghost, while red indicates the presence of the ghost in the selected cell. However, the sensor is prone to inaccuracies, necessitating a probabilistic reasoning approach to infer the ghost's likely position based on observed cell colors. In addition to this color-based proximity information, the direction sensor provides further insights into the ghost's location. Depending on the direction sensor's readings, players receive directional cues indicating the relative orientation of the ghost from the selected tile. This additional mechanic adds depth to the gameplay, requiring players to consider both proximity and direction cues to effectively locate the ghost within the grid.
1.3 Objective
	The objective of this project is to develop the game interface and implement a Bayesian probabilistic inference algorithm. This algorithm will calculate and display the probability of the ghost's presence in each cell, ultimately identifying the cell most likely to contain the ghost.

	2. Game Description
1.2 Overview of "Bust the Ghost" Game:
	"Bust the Ghost" is a grid-based game designed as part of the 3rd A.I. mini project. The objective of the game is to locate a hidden ghost within a grid using sensor readings. Players interact with the grid by selecting cells to perform sensor readings, with each cell revealing clues about the ghost's proximity and direction. The game incorporates probabilistic reasoning to infer the ghost's likely position based on observed sensor data.
2.2 Game Components and Mechanics:
	The game components of "Bust the Ghost" consist of a 9x12 grid interface serving as the playing area, where players interact by selecting tiles to perform sensor readings. Interactive buttons are provided for gameplay actions such as "Bust the ghost" and "Peep", facilitating user engagement. Additionally, the integration of a direction sensor enhances gameplay mechanics by providing orientation information about the ghost's position relative to the selected tile, thereby enriching the player experience and adding complexity to the game dynamics.
2.3 User Interface Description:
	Grid Interface: The user interface features a 9x12 grid where gameplay occurs. Each grid tile is clickable for performing sensor readings or busting the ghost.
	Buttons: Interactive buttons for "Bust" after which you guess the cell that contains the ghost, "Peep", and a toggle for activating the peep feature are provided for user interaction.
	Game Info Displayer: A game info displayer presents information about the remaining credits, bust attempts remaining, and the current score. Additionally, a message area informs the player of game outcomes such as winning, incorrect guesses, or running out of credits, ensuring a clear and engaging user experience.
	Feedback: Real-time feedback is provided to the user through color-coded tiles indicating the ghost's proximity and direction. Additionally, the interface displays information through the message section about the remaining credit (attempts) and current score, updating after each gameplay action to provide continuous feedback to the user.

3. Probabilistic Inferencing
3.1 Random Variables and Domains:
	The game "Bust the Ghost" involves two primary random variables:
	G: Represents the ghost's location within the grid. It has a domain consisting of coordinates within the grid, ranging from (1,1) to (9,12) for a 9x12 grid. At the start of the game, the ghost is placed randomly on the grid and his location (x,y) is printed in the console, this done with the function PlaceGhost() defined as the following:
 
	S: Denotes the sensor reading obtained upon clicking a cell. It has a domain that contains Red, Green, Orange, and Yellow, representing different distances of the ghost from the clicked cell.
 
3.2 Conditional Probability Distributions:
The conditional probability distributions in the game model the relationship between the sensor readings (S) and the actual distance of the ghost from the clicked cell. These distributions are used to determine the color displayed on the cell after a sensor reading. For instance:
P(S=Color∣Distance from Ghost) assigns probabilities to different colors based on the distance of the ghost from the clicked cell. This distribution reflects the sensor's sensitivity and the likelihood of observing a particular color given the actual distance of the ghost.
We have the following tables for the different distributions of colors depending on the distance from the ghost:
+5 Cells away:
Color	Green	Orange	Yellow	Red
Probability	0.7	0.1	0.1	0.1

3 or 4 Cells away:
Color	Green	Orange	Yellow	Red
Probability	0.1	0.5	0.3	0.1

2 or 1 Cells away:
Color	Green	Orange	Yellow	Red
Probability	0.1	0.3	0.1	0.5

0 Cells away:
Color	Green	Orange	Yellow	Red
Probability	0.01	0.01	0.01	0.97

3.3 Bayesian Inference for Ghost Location:
Bayesian inference is employed in the game to update the probabilities of the ghost's location (G) based on observed sensor readings (S). After each click, the posterior probability Pt(G=Li) of the ghost being in each cell Li is updated using Bayes' theorem: 
Pt(G=Li)=P(S=Color at location Li∣G=Li)×P_(t-1)(G=Lj) Here, Pt−1(G=Lj) represents the prior probability distribution of the ghost's location, which is initialized as a uniform distribution. The posterior probability is updated iteratively, taking into account the likelihood of observing the sensor reading given the ghost's location and the prior probability of the ghost's location. This iterative process allows for continuous refinement of the estimated ghost location based on the observed sensor data.
         

	4. Implementation Details
4.1 Functions and Structures Overview:
	The implementation of "Bust the Ghost" involves several key functions and structures to manage gameplay, probabilistic modeling, and user interface interactions. These include event handlers for user clicks, functions for updating game state, and structures for managing ghost placement and probability calculations.
	The user interface (UI) for "Bust the Ghost" game is implemented using HTML, CSS, and JavaScript. The grid layout, buttons, game info display, and feedback messages are all created using HTML elements styled with CSS. JavaScript is utilized to handle user interactions, update game state, and manage game logic.
4.2 Ghost Placement Algorithm:
The placeGhost function is responsible for randomly placing the ghost within the grid at the start of the game. It utilizes a uniform distribution to select a random grid cell as the ghost's initial position, ensuring fairness and unpredictability in ghost placement.
 
4.3 Computing Initial Prior Probabilities:
The computeInitialPriorProbabilities function initializes the prior probabilities for each grid cell before any sensor readings are taken. It assigns equal probabilities to all cells, assuming an initial lack of information about the ghost's location.

4.4 Color Perception and Distance Sensing:
The toggleCellColor function handles the color perception and distance sensing aspect of the game. It calculates the distance between the clicked cell and the ghost, determines the appropriate color based on the distance, and updates the cell's color accordingly. This function also deducts credits for each sensor reading and provides real-time feedback to the user.
   

4.5 Updating Posterior Ghost Location Probabilities:
The toggleCellColor function dynamically updates the posterior probabilities of the ghost's location after each sensor reading by calling the probabilities function. This function adjusts the probabilities of all cells based on the observed color of the clicked cell and the current ghost location, implementing Bayesian inference to iteratively refine the estimation of the ghost's position. Through conditional probability distributions and normalization, the probabilities are recalibrated to reflect the likelihood of the ghost being present in each cell, contributing to the continuous refinement of the game's probabilistic model.

4.6 Probabilities Normalization:
After updating the posterior ghost location probabilities in the toggleCellColor function, a crucial step is undertaken to ensure the integrity and coherence of the probability distribution. The totalProbability variable computes the sum of probabilities across all grid cells, facilitating normalization to maintain a valid probability distribution. By dividing each cell's probability by the total sum and rounding to three decimal places, the probabilities are adjusted to collectively sum up to 1, conforming to the fundamental principles of probability theory. This normalization process guarantees that the probabilistic model accurately reflects the relative likelihood of the ghost's presence in different grid cells, enabling effective decision-making based on the inferred probabilities.
 

5. Bonus Feature: Direction Sensor 
 
As part of enhancing the immersive experience of our "Bust the Ghost" game, we decided to introduce a bonus feature: the Direction Sensor. This addition aims to provide players with valuable clues about the potential locations of ghosts within the game environment. By leveraging probabilistic inference techniques, the Direction Sensor adds an exciting dimension to the gameplay, requiring players to strategically interpret sensor readings to uncover ghost positions. 
 
5.1 Conditional Distributions for Direction Sensor: 
 
Incorporating a direction sensor into our "Bust the Ghost" game adds an exciting dimension, allowing players to receive hints about the direction of nearby ghosts. To implement this feature effectively, we first defined the conditional distributions for the direction sensor. 
 
  
 
The direction sensor provides probabilistic information about the possible directions in which a ghost might be located relative to the player's position. We established these probabilities based on factors such as the player's movement and the ghost's previous positions. 
 
For instance, if the player moves in a certain direction and encounters no ghosts, the probability of a ghost being in that direction decreases. Conversely, if a ghost is detected in a particular direction, the probability of finding additional ghosts in that direction increases. We used Bayesian inference to update these probabilities dynamically as the player navigates the game environment. 
 
5.2 Formula for Updating Posterior Probabilities: 
 
The formula for updating posterior probabilities involves combining prior knowledge with new evidence obtained from the direction sensor. We used Bayesian inference to update the probabilities based on the likelihood of observing the sensor readings given the current state of the game. 
 
Mathematically, this process can be represented as: 
 
  
 
By iteratively applying this formula as the player interacts with the direction sensor, we continuously refine our estimates of ghost positions, enhancing the gameplay experience and increasing the player's chances of success. 
 
Overall, integrating a direction sensor adds depth and complexity to the game, requiring players to strategically interpret sensor readings and adjust their gameplay tactics accordingly. 
 
 
6. User Experience 
 
6.1 Interaction Flow: 
The user experience of "Bust the Ghost" centers around intuitive interaction flows that engage players while navigating through the game environment. Here's a breakdown of the interaction flow: 
 
	Game Launch: Upon launching the game, players are greeted with an enticing title screen and a brief introduction to the game mechanics. 
 
	Main Menu: From the title screen, players can access the main menu, where they have several options, including starting a new game, accessing settings, or viewing instructions. 
 
	New Game Setup: When initiating a new game, players are prompted to set up game parameters such as difficulty level and whether to enable the Direction Sensor bonus feature. 
 
	Gameplay: Once the game begins, players are presented with the game grid, where they can click on individual cells to reveal potential ghost locations. They can also toggle the Direction Sensor on or off during gameplay. 
 
	Direction Sensor Interaction: If the Direction Sensor is enabled, players can activate it to receive directional clues about nearby ghosts. These clues are displayed visually or through text prompts, helping players deduce the optimal locations to uncover ghosts. 
 
	Ghost Encounter: When a player reveals a cell containing a ghost, they are informed of their success and awarded points based on the game's scoring system. 
 
	Game Over: The game continues until all ghosts are successfully uncovered or until the player exhausts their attempts. At this point, the game ends, and the player's final score is displayed. 
 
 
6.2 Game Logic Flow: 
Behind the scenes, the game operates on a robust logic flow that governs various aspects of gameplay and ensures a seamless user experience. Here's an overview of the game logic flow: 
 
	Initialization: Upon launching the game, necessary variables and data structures are initialized, including the game grid, ghost placements, and player score. 
 
	User Input Handling: As players interact with the game grid, their inputs are processed to determine the action taken, whether it's revealing a cell or activating the Direction Sensor. 
 
	Ghost Placement: The game randomly places ghosts within the grid, ensuring a varied and challenging gameplay experience for each session. 
 
	Scoring Mechanism: Points are awarded to players based on their actions, such as successfully uncovering a ghost or utilizing the Direction Sensor effectively. 
 
	Direction Sensor Functionality: If enabled, the Direction Sensor provides players with directional clues about nearby ghosts. This functionality relies on probabilistic inference techniques to calculate the most probable ghost locations based on sensor readings. 
 
	Game Progression: The game progresses as players uncover cells and reveal potential ghost locations. The game ends when either all ghosts are uncovered, or the player exhausts their attempts. 
 
	Game Over Handling: Upon game completion, the final score is calculated and displayed to the player, along with an option to start a new game or return to the main menu. 
 

7. Demonstration

7.1 Youtube Demo Link:

https://www.youtube.com/watch?v=ys2yXCiyemc

7.2 Brief Overview of the Demo:
	
The demo offers a concise yet thorough exploration of the "Bust the Ghost" game, providing a comprehensive overview of its features and mechanics. Through systematic interaction with the grid and utilization of game functionalities like sensor readings and "Bust the ghost" actions, the video demonstrates the efficacy of the probabilistic calculations in accurately updating the ghost's location probabilities. Each gameplay action dynamically influences the inferred probabilities, showcasing the game's responsiveness and fidelity to probabilistic reasoning principles. Overall, the demo effectively illustrates the game's immersive experience and the effectiveness of its probabilistic model in providing engaging gameplay dynamics.
