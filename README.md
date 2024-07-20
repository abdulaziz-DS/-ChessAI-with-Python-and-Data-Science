# ChessAI with Python and Data Science

## Introduction
This Python program is an implementation of a chess bot that allows users to play against an AI opponent. The game is played on an 8x8 chess board, where players take turns moving their pieces according to the rules of chess. The goal is to checkmate the opponent's king. The bot uses the python-chess library to represent the chess board and implement the game logic, including a simple chess AI.

## Prerequisites
- Basic understanding of Python programming.
- Familiarity with functions, global variables, and control structures in Python.
- Python environment (like IDLE, Jupyter Notebook, or any IDE supporting Python).
- Knowledge of the rules of chess.

## Learning Objectives
- Implementing a chess game logic using Python and the python-chess library.
- Understanding the use of functions, global variables, and conditional statements.
- Developing a simple chess AI using evaluation functions and search algorithms.
- Handling user inputs and validating them.
- Visualizing the chess board and game progress using the `chess.svg` module.

## Installation Guide
Install the python-chess library using pip:
```bash
pip install python-chess
```

## Code

```python
# Step 1: Import 
import chess
import chess.svg
from IPython.display import SVG, display

# Step 2: Represent the Chess Board
board = chess.Board()

# Step 3: Implement the Game Logic
def handle_player_move():
    while True:
        move = input("Enter your move (e.g., e2e4): ")
        try:
            board.push_san(move)
            break
        except ValueError:
            print("Invalid move. Please try again.")

def handle_ai_move():
    best_move = None
    best_score = float('-inf')
   
    for move in board.legal_moves:
        board.push(move)
        score = evaluate_board(board)
        board.pop()
        
        if score > best_score:
            best_score = score
            best_move = move
    
    board.push(best_move)

# Step 4: Develop the Chess AI
PIECE_SQUARE_TABLES = {
    chess.PAWN: [
        0, 0, 0, 0, 0, 0, 0, 0,
        50, 50, 50, 50, 50, 50, 50, 50,
        10, 10, 20, 30, 30, 20, 10, 10,
        5, 5, 10, 25, 25, 10, 5, 5,
        0, 0, 0, 20, 20, 0, 0, 0,
        5, -5, -10, 0, 0, -10, -5, 5,
        5, 10, 10, -20, -20, 10, 10, 5,
        0, 0, 0, 0, 0, 0, 0, 0
    ]
}

def evaluate_board(board):
    score = 0
    
    # Material evaluation
    score += len(board.pieces(chess.PAWN, chess.WHITE))
    score -= len(board.pieces(chess.PAWN, chess.BLACK))
    
    # Piece-square tables
    score += sum(PIECE_SQUARE_TABLES[chess.PAWN][i] for i in board.pieces(chess.PAWN, chess.WHITE))
    score -= sum(PIECE_SQUARE_TABLES[chess.PAWN][chess.square_mirror(i)] for i in board.pieces(chess.PAWN, chess.BLACK))
    
    return score

def minimax(board, depth, alpha, beta, maximizing_player):
    if depth == 0 or board.is_game_over():
        return evaluate_board(board)
    
    if maximizing_player:
        max_eval = float('-inf')
        for move in board.legal_moves:
            board.push(move)
            eval = minimax(board, depth - 1, alpha, beta, False)
            board.pop()
            max_eval = max(max_eval, eval)
            alpha = max(alpha, eval)
            if beta <= alpha:
                break
        return max_eval
    else:
        min_eval = float('inf')
        for move in board.legal_moves:
            board.push(move)
            eval = minimax(board, depth - 1, alpha, beta, True)
            board.pop()
            min_eval = min(min_eval, eval)
            beta = min(beta, eval)
            if beta <= alpha:
                break
        return min_eval

# Step 1: Set up the Project
print("Welcome to the Chess Bot!")

# Visualization function
def display_board():
    board_svg = chess.svg.board(board=board)
    display(SVG(board_svg))

# Display the initial board
display_board()

# Game loop
while not board.is_game_over():
    handle_player_move()
    display_board()
    
    if board.is_game_over():
        break
    
    handle_ai_move()
    display_board()

if board.is_checkmate():
    if board.turn == chess.WHITE:
        print("Congratulations! You won.")
    else:
        print("The AI won. Better luck next time!")
elif board.is_stalemate():
    print("The game is a draw (stalemate).")
elif board.is_insufficient_material():
    print("The game is a draw (insufficient material).")
else:
    print("The game is a draw.")
```

## Project Steps
1. **Set up the Project**: Install Python and the python-chess library. Create a new Python file for your chess bot project and import the necessary modules.
2. **Represent the Chess Board**: Use the `chess.Board()` class to create a new chess board. Familiarize yourself with the methods to manipulate the board, like `push()`, `pop()`, `is_check()`, etc.
3. **Implement the Game Logic**: Write functions to handle player moves and the AI's moves. Use the `Board.push()` method to make moves on the board and `Board.is_legal()` to check if a move is legal.
4. **Develop the Chess AI**: Implement a simple evaluation function to assess the value of a board position. Use the Minimax algorithm with Alpha-Beta pruning to search the game tree and find the best move.
5. **Add Visualization**: Utilize the `chess.svg` module to generate SVG images of the chess board and display them using the `IPython.display` module. This will provide a beginner-friendly visualization of the game progress.
6. **Optimize the Chess AI**: Implement techniques like transposition tables, iterative deepening, and quiescence search to improve the AI's performance. Experiment with different evaluation functions and search algorithms.
7. **Test and Refine**: Thoroughly test your chess bot against known chess engines or online opponents. Identify and fix any bugs or edge cases in your implementation. Continuously improve your AI's performance by tweaking the evaluation function and search algorithms.

## Code Explanation
The code sets up the chess board, implements the game logic, and develops a simple chess AI using the Minimax algorithm with Alpha-Beta pruning. The `display_board()` function is used to provide a visual representation of the chess board, which can be helpful for beginners.

The evaluation function `evaluate_board()` assesses the value of a board position based on material evaluation and piece-square tables. The `minimax()` function recursively evaluates all possible moves to a certain depth and uses the evaluation function to assess the leaf nodes. Alpha-Beta pruning is used to optimize the search process.

## Pseudo Code
1. **Initialize the chess board** using `chess.Board()`.
2. **Display the initial chess board** using `display_board()`.
3. **While the game is not over**:
   - Get the player's move using `handle_player_move()`.
   - Update the board using `Board.push()`.
   - Display the updated board using `display_board()`.
   - Check for a win or draw using `Board.is_checkmate()` and `Board.is_stalemate()`.
   - If the game is not over, get the AI's move using `handle_ai_move()`.
   - Update the board using `Board.push()`.
   - Display the updated board using `display_board()`.
   - Check for a win or draw using `Board.is_checkmate()` and `Board.is_stalemate()`.
4. **If a player wins or the game is a draw**:
   - Declare the result.

## Challenges and Solutions
Challenge: Implementing an effective chess AI that can compete with human players.
Solution: Use advanced search algorithms like Alpha-Beta pruning and techniques like transposition tables and iterative deepening to improve the AI's performance. Continuously refine the evaluation function to better assess the value of board positions.

Challenge: Handling edge cases and invalid moves.
Solution: Thoroughly test the game logic and use `Board.is_legal()` to validate each move before updating the board.

## Testing
Test the chess bot with various inputs and scenarios to ensure all possible game situations are handled correctly. Verify that the game correctly identifies win conditions, draws, and illegal moves. Compare the bot's performance against known chess engines or online opponents to assess its effectiveness.

## Contributors
Abdul Aziz Shaikh
