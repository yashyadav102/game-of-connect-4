# game-of-connect-4
public static void main(String[] args) {
    Scanner scanner = new Scanner(System.in);
    initializeBoard();

    boolean gameEnded = false;
    System.out.println("Welcome to Connect Four!");
    printBoard();

    while (!gameEnded) {
        System.out.println("Player " + currentPlayer + "'s turn.");
        int col;

        while (true) {
            System.out.print("Choose a column (0 to 6): ");
            col = scanner.nextInt();
            if (isValidMove(col)) {
                break;
            } else {
                System.out.println("Column full or out of bounds. Try again.");
            }
        }

        int row = dropDisc(col);
        printBoard();

        if (checkWin(row, col)) {
            System.out.println("Player " + currentPlayer + " wins!");
            gameEnded = true;
        } else if (isBoardFull()) {
            System.out.println("It's a draw!");
            gameEnded = true;
        } else {
            switchPlayer();
        }
    }

    scanner.close();
}

private static void initializeBoard() {
    for (int i = 0; i < ROWS; i++)
        for (int j = 0; j < COLUMNS; j++)
            board[i][j] = ' ';
}

private static void printBoard() {
    System.out.println();
    for (int i = 0; i < ROWS; i++) {
        System.out.print("|");
        for (int j = 0; j < COLUMNS; j++) {
            System.out.print(board[i][j] + "|");
        }
        System.out.println();
    }
    System.out.println(" 0 1 2 3 4 5 6\n");
}

private static boolean isValidMove(int col) {
    return col >= 0 && col < COLUMNS && board[0][col] == ' ';
}

private static int dropDisc(int col) {
    for (int i = ROWS - 1; i >= 0; i--) {
        if (board[i][col] == ' ') {
            board[i][col] = currentPlayer;
            return i;
        }
    }
    return -1; // Should not happen if isValidMove is checked
}

private static void switchPlayer() {
    currentPlayer = (currentPlayer == 'R') ? 'Y' : 'R';
}

private static boolean isBoardFull() {
    for (int j = 0; j < COLUMNS; j++) {
        if (board[0][j] == ' ')
            return false;
    }
    return true;
}

private static boolean checkWin(int row, int col) {
    return checkDirection(row, col, 0, 1)   // Horizontal
        || checkDirection(row, col, 1, 0)   // Vertical
        || checkDirection(row, col, 1, 1)   // Diagonal /
        || checkDirection(row, col, 1, -1); // Diagonal \
}

private static boolean checkDirection(int row, int col, int dRow, int dCol) {
    char symbol = board[row][col];
    int count = 1;

    // Check forward direction
    int r = row + dRow, c = col + dCol;
    while (r >= 0 && r < ROWS && c >= 0 && c < COLUMNS && board[r][c] == symbol) {
        count++;
        r += dRow;
        c += dCol;
    }

    // Check backward direction
    r = row - dRow; c = col - dCol;
    while (r >= 0 && r < ROWS && c >= 0 && c < COLUMNS && board[r][c] == symbol) {
        count++;
        r -= dRow;
        c -= dCol;
    }

    return count >= 4;
}
