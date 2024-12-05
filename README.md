import numpy as np

def is_valid(board, row, col, num):
    """Check if placing `num` at (row, col) is valid."""
    # Check row
    if num in board[row, :]:
        return False
    # Check column
    if num in board[:, col]:
        return False
    # Check 3x3 sub-grid
    sub_row, sub_col = 3 * (row // 3), 3 * (col // 3)
    if num in board[sub_row:sub_row+3, sub_col:sub_col+3]:
        return False
    return True

def find_empty(board):
    """Find the next empty cell in the board."""
    for row in range(9):
        for col in range(9):
            if board[row, col] == 0:
                return row, col
    return None

def solve_sudoku(board):
    """Solve the Sudoku puzzle using backtracking."""
    empty = find_empty(board)
    if not empty:
        return True  # Puzzle solved
    row, col = empty

    for num in range(1, 10):
        if is_valid(board, row, col, num):
            board[row, col] = num
            if solve_sudoku(board):
                return True
            board[row, col] = 0  # Backtrack

    return False

# Example Sudoku puzzle (0 represents empty cells)
sudoku_board = np.array([
    [5, 3, 0, 0, 7, 0, 0, 0, 0],
    [6, 0, 0, 1, 9, 5, 0, 0, 0],
    [0, 9, 8, 0, 0, 0, 0, 6, 0],
    [8, 0, 0, 0, 6, 0, 0, 0, 3],
    [4, 0, 0, 8, 0, 3, 0, 0, 1],
    [7, 0, 0, 0, 2, 0, 0, 0, 6],
    [0, 6, 0, 0, 0, 0, 2, 8, 0],
    [0, 0, 0, 4, 1, 9, 0, 0, 5],
    [0, 0, 0, 0, 8, 0, 0, 7, 9]
])

# Solve the Sudoku puzzle
if solve_sudoku(sudoku_board):
    print("Solved Sudoku Board:")
    print(sudoku_board)
else:
    print("No solution exists.")
