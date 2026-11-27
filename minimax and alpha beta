# Practical 4 — Minimax and Alpha-Beta on Tic-Tac-Toe
# Run:  python P4_tictactoe.py
# Library: none. Only plain Python 3.
# We use a mid-game board so the tree is small enough to print quickly.
# On an empty board both methods still choose the same first square,
# but minimax visits hundreds of thousands of nodes.
 
EMPTY = " "
 
# X to move. This is a classroom board, not an empty start.
BOARD = [
    "X", EMPTY, EMPTY,
    EMPTY, "O", EMPTY,
    EMPTY, EMPTY, EMPTY,
]
 
 
def winner(b):
    lines = [
        (0, 1, 2), (3, 4, 5), (6, 7, 8),
        (0, 3, 6), (1, 4, 7), (2, 5, 8),
        (0, 4, 8), (2, 4, 6),
    ]
    for a, c, d in lines:
        if b[a] != EMPTY and b[a] == b[c] == b[d]:
            return b[a]
    if EMPTY not in b:
        return "draw"
    return None
 
 
def minimax(b, is_max, stats):
    # Walk the whole remaining tree. MAX wants +10. MIN wants -10.
    stats["nodes"] += 1
    w = winner(b)
    if w == "X":
        return 10
    if w == "O":
        return -10
    if w == "draw":
        return 0
    if is_max:
        best = -999
        for i in range(9):
            if b[i] == EMPTY:
                b[i] = "X"
                best = max(best, minimax(b, False, stats))
                b[i] = EMPTY
        return best
    best = 999
    for i in range(9):
        if b[i] == EMPTY:
            b[i] = "O"
            best = min(best, minimax(b, True, stats))
            b[i] = EMPTY
    return best
 
 
def alphabeta(b, is_max, alpha, beta, stats):
    # Same values as minimax. Skip a branch when alpha >= beta.
    stats["nodes"] += 1
    w = winner(b)
    if w == "X":
        return 10
    if w == "O":
        return -10
    if w == "draw":
        return 0
    if is_max:
        value = -999
        for i in range(9):
            if b[i] == EMPTY:
                b[i] = "X"
                value = max(value, alphabeta(b, False, alpha, beta, stats))
                b[i] = EMPTY
                alpha = max(alpha, value)
                if alpha >= beta:          # THE CUT LINE
                    stats["pruned"] += 1
                    break
        return value
    value = 999
    for i in range(9):
        if b[i] == EMPTY:
            b[i] = "O"
            value = min(value, alphabeta(b, True, alpha, beta, stats))
            b[i] = EMPTY
            beta = min(beta, value)
            if alpha >= beta:              # THE CUT LINE
                stats["pruned"] += 1
                break
    return value
 
 
def best_move(board, use_ab):
    b = list(board)
    stats = {"nodes": 0, "pruned": 0}
    choice, best = None, -999
    for i in range(9):
        if b[i] != EMPTY:
            continue
        b[i] = "X"
        if use_ab:
            val = alphabeta(b, False, -999, 999, stats)
        else:
            val = minimax(b, False, stats)
        b[i] = EMPTY
        if val > best:
            best, choice = val, i
    return choice, best, stats
 
 
def show(b):
    for r in range(3):
        print(" ", "|".join(b[r * 3 + c] if b[r * 3 + c] != EMPTY else "." for c in range(3)))
 
 
if __name__ == "__main__":
    print("PRACTICAL 4  |  X to move")
    show(BOARD)
    mm = best_move(BOARD, False)
    ab = best_move(BOARD, True)
    print()
    print("Minimax     square", mm[0], "value", mm[1], "nodes", mm[2]["nodes"])
    print("Alpha-Beta  square", ab[0], "value", ab[1], "nodes", ab[2]["nodes"],
          "cuts", ab[2]["pruned"])
    print()
    print("Same square and same value. Alpha-beta visits fewer nodes.")

