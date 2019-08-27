### HarvardX |  PH526x 
## Using Python For Research
### Week 2 Homework: Exercises 1-5

**Exercise 1**
For our tic-tac-toe board, we will use a numpy array with dimension 3 by 3. Write a function create_board() that creates such a board with the value of each cell set to the integer 0. Call create_board() and store it. What is the correct numpy function to initialize our tic-tac-toe board?

    np.zeros((3,3), dtype=int)

**Exercise 2**
Players 1 and 2 will take turns changing values of this array from a 0 to a 1 or 2, indicating the number of the player who places a marker there. Create a function place(board, player, position), where:

-   player is the current player (an integer 1 or 2).
-   position is a tuple of length 2 specifying a desired location to place their marker. Your function should only allow the current player to place a marker on the board (change the board position to their number) if that position is empty (zero). Use create_board() to store a board as board, and use place to have Player 1 place a marker on location (0, 0). What is the correct way to use the place function to have Player 1 place a marker on location (0,0)?

`place(board, 1, (0,0))`

**Exercise 3**

Create a function possibilities(board) that returns a list of all positions (tuples) on the board that are not occupied (0). (Hint: numpy.where is a handy function that returns a list of indices that meet a condition.) Note that board is defined as at the end of Exercise 2. Call possibilities(board) to see what it returns! What does possibilities(board) return?

    **[(0, 1), (0, 2), (1, 0), (1, 1), (1, 2), (2, 0), (2, 1), (2, 2)]**
   
   
    import random
    import matplotlib.pyplot as plt
    import numpy as np
    
    def create_board():
    	board = np.zeros((3,3), dtype=int)
    	return board
    
    board = create_board()
    
    def place(board, player, position):
    	if board[position] == 0:
    		board[position] = player
    		return board
    
    board = create_board()
    place(board, 1, (0, 0))
    
    def possibilities(board):
    	return list(zip(*np.where(board == 0)))
    
    possibilities(board)

**Exercise 4**

The next step is for the current player to place a marker among the available positions. In this exercise, we will select an available board position at random and place a marker there. Write a function random_place(board, player) that places a marker for the current player at random among all the available positions (those currently set to 0). Find possible placements with possibilities(board). Select one possible placement at random using random.choice(selection). Note that board is already defined as at the end of Exercise 2. Call random_place(board, player) to place a random marker for Player 2, and store this as board to update its value.

    Enter the first number. Enter the number only.  **1**
    Enter the second number. Enter the number only.  **0**


    import random
    import matplotlib.pyplot as plt
    import numpy as np
    
    def create_board():
    	board = np.zeros((3,3), dtype=int)
    	return board
    
    board = create_board()
    
    def place(board, player, position):
    	if board[position] == 0:
    		board[position] = player
    		return board
    
    board = create_board()
    place(board, 1, (0, 0))
    
    def possibilities(board):
    	return list(zip(*np.where(board == 0)))
    
    possibilities(board)

    def random_place(board, player):  
	    selections = possibilities(board)
	    if len(selections) > 0:
		    selection = random.choice(selections)
		    place(board, player, selection)
	    return board
    
    random_place(board, 2)

**Exercise 5**

We will now have both players place three markers each. A new board is given by the sample code. Call random_place(board, player) to place three pieces each on board for players 1 and 2. Print board to see your result. At what positions does player 1 have markers after three rounds of random placement?

    **(0,2), (1,1), (2,1)**
    
    import random
    import matplotlib.pyplot as plt
    import numpy as np
    
    def create_board():
        board = np.zeros((3,3), dtype=int)
        return board
    
    board = create_board()
    
    def place(board, player, position):
        if board[position] == 0:
	        board[position] = player
		    return board
    
    board = create_board()
    place(board, 1, (0, 0))
    
    def possibilities(board):
        return list(zip(*np.where(board == 0)))
    
    possibilities(board)
    
    def random_place(board, player):
        selections = possibilities(board)
	    if len(selections) > 0:   
		    selection = random.choice(selections)
		    place(board, player, selection)
    return board
    
    random_place(board, 2)
    
    random.seed(1)    
    board = create_board()
    
    for i in range(3):
        for player in [1, 2]:
	        random_place(board, player)
    print(board)

