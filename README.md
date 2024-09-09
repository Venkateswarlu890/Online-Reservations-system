
         def print_board(board):
  """Prints the current state of the Tic-Tac-Toe board."""
  for row in board:
    print(" | ".join(row))
    print("-" * 9)

def check_win(board, player):
  """Checks if the specified player has won the game."""
  # Check rows
  for row in board:
    if all(cell == player for cell in row):
      return True
  # Check columns
  for col in range(3):
    if all(board[row][col] == player for row in range(3)):
      return True
  # Check diagonals
  if all(board[i][i] == player for i in range(3)) or all(board[i][2 - i] == player for i in range(3)):
    return True
  return False

def is_board_full(board):
  """Checks if the board is full."""
  return all(all(cell != " " for cell in row) for row in board)

def play_tic_tac_toe():
  """Plays a game of Tic-Tac-Toe."""
  board = [[" ", " ", " "], [" ", " ", " "], [" ", " ", " "]]
  current_player = "X"

  while True:
    print_board(board)

    row = int(input("Enter the row (1-3): ")) - 1
    col = int(input("Enter the column (1-3): ")) - 1

    if board[row][col] != " ":
      print("Invalid move. Please try again.")
      continue

    board[row][col] = current_player

    if check_win(board, current_player):
      print_board(board)
      print(current_player + " wins!")
      break

    if is_board_full(board):
      print_board(board)
      print("It's a tie!")
      break

    current_player = "O" if current_player == "X" else "X"

if __name__ == "__main__":
  play_tic_tac_toe()          