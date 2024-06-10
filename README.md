def initialize_board():
    return [' ' for _ in range(9)]

def print_board(board):
    for i in range(3):
        print('| ' + ' | '.join(board[i*3:(i+1)*3]) + ' |')

def check_winner(board, player):
    win_conditions = [(0, 1, 2), (3, 4, 5), (6, 7, 8), (0, 3, 6), (1, 4, 7), (2, 5, 8), (0, 4, 8), (2, 4, 6)]
    return any(board[a] == board[b] == board[c] == player for a, b, c in win_conditions)

def is_board_full(board):
    return ' ' not in board

def display_board(board):
    for i in range(3):
        print(' ' + board[3*i] + ' | ' + board[3*i+1] + ' | ' + board[3*i+2])
        if i < 2:
            print("---|---|---")

def player_move(board):
    move = -1
    while move not in range(1, 10) or board[move-1] != ' ':
        move = int(input("Enter your move (1-9): "))
    board[move-1] = 'X'

def minimax(board, depth, is_maximizing):
    if check_winner(board, 'O'):
        return 1
    elif check_winner(board, 'X'):
        return -1
    elif is_board_full(board):
        return 0

    if is_maximizing:
        best_score = float('-inf')
        for i in range(9):
            if board[i] == ' ':
                board[i] = 'O'
                score = minimax(board, depth + 1, False)
                board[i] = ' '
                best_score = max(score, best_score)
        return best_score
    else:
        best_score = float('inf')
        for i in range(9):
            if board[i] == ' ':
                board[i] = 'X'
                score = minimax(board, depth + 1, True)
                board[i] = ' '
                best_score = min(score, best_score)
        return best_score

def ai_move(board):
    best_score = float('-inf')
    move = 0
    for i in range(9):
        if board[i] == ' ':
            board[i] = 'O'
            score = minimax(board, 0, False)
            board[i] = ' '
            if score > best_score:
                best_score = score
                move = i
    board[move] = 'O'

def main():
    board = initialize_board()
    display_board(board)
    while True:
        player_move(board)
        if check_winner(board, 'X'):
            display_board(board)
            print("You win!")
            break
        if is_board_full(board):
            display_board(board)
            print("It's a draw!")
            break
        ai_move(board)
        if check_winner(board, 'O'):
            display_board(board)
            print("AI wins!")
            break
        if is_board_full(board):
            display_board(board)
            print("It's a draw!")
            break
        display_board(board)

if __name__ == "__main__":
    main()
