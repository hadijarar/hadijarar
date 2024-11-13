import tkinter as tk
from tkinter import messagebox
import random

# Initialize game variables
current_player = "X"  # Human player is "X", Computer is "O"
board = [["", "", ""], ["", "", ""], ["", "", ""]]

def check_winner():
    # Check rows, columns, and diagonals for a win
    for i in range(3):
        if board[i][0] == board[i][1] == board[i][2] != "":
            return board[i][0]
        if board[0][i] == board[1][i] == board[2][i] != "":
            return board[0][i]
    if board[0][0] == board[1][1] == board[2][2] != "":
        return board[0][0]
    if board[0][2] == board[1][1] == board[2][0] != "":
        return board[0][2]
    return None

def is_draw():
    # Check if all cells are filled, meaning the game is a draw
    for row in board:
        if "" in row:
            return False
    return True

def button_click(row, col):
    global current_player
    
    if board[row][col] == "" and current_player == "X" and not check_winner():
        # Human makes a move
        board[row][col] = "X"
        buttons[row][col].config(text="X")
        
        # Check for a winner or draw after human move
        winner = check_winner()
        if winner:
            messagebox.showinfo("Tic-Tac-Toe", f"Player {winner} wins!")
            reset_board()
            return
        elif is_draw():
            messagebox.showinfo("Tic-Tac-Toe", "It's a draw!")
            reset_board()
            return

        # Switch to computer's turn
        current_player = "O"
        computer_move()

def computer_move():
    global current_player
    
    if current_player == "O" and not check_winner():
        # Get a list of empty cells
        empty_cells = [(r, c) for r in range(3) for c in range(3) if board[r][c] == ""]
        
        # Make a random move from the list of empty cells
        if empty_cells:
            row, col = random.choice(empty_cells)
            board[row][col] = "O"
            buttons[row][col].config(text="O")
        
            # Check for a winner or draw after computer move
            winner = check_winner()
            if winner:
                messagebox.showinfo("Tic-Tac-Toe", f"Player {winner} wins!")
                reset_board()
                return
            elif is_draw():
                messagebox.showinfo("Tic-Tac-Toe", "It's a draw!")
                reset_board()
                return
        
            # Switch back to human's turn
            current_player = "X"

def reset_board():
    # Reset the game board and GUI buttons
    global board, current_player
    board = [["", "", ""], ["", "", ""], ["", "", ""]]
    current_player = "X"
    for row in range(3):
        for col in range(3):
            buttons[row][col].config(text="")

# Create main window
root = tk.Tk()
root.title("Tic-Tac-Toe Against Computer")

# Create a 3x3 grid of buttons
buttons = [[None for _ in range(3)] for _ in range(3)]
for row in range(3):
    for col in range(3):
        buttons[row][col] = tk.Button(root, text="", font=("Arial", 24), width=5, height=2,
                                      command=lambda r=row, c=col: button_click(r, c))
        buttons[row][col].grid(row=row, column=col)

# Run the application
root.mainloop()

