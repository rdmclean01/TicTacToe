# TicTacToe

#ifndef MINIMAX_H_
#define MINIMAX_H_
#include <stdint.h>
#include <stdio.h>
#include <stdbool.h>

// Defines the boundaries of the tic-tac-toe board.
#define MINIMAX_BOARD_ROWS 3
#define MINIMAX_BOARD_COLUMNS 3

// These are the values in the board to represent who is occupying what square.
#define MINIMAX_USED_SQUARE 3           // Used when creating new board states.
#define MINIMAX_PLAYER_SQUARE 2
#define MINIMAX_OPPONENT_SQUARE 1
#define MINIMAX_EMPTY_SQUARE 0

// Scoring for minimax.
#define MINIMAX_PLAYER_WINNING_SCORE 10 // typically X
#define MINIMAX_OPPONENT_WINNING_SCORE -10 // typically O
#define MINIMAX_DRAW_SCORE 0
#define MINIMAX_NOT_ENDGAME -1 // Not an end-game.

#define MINIMAX_BOARD_SIZE 9


// Boards contain just an array of squares. I used a struct to provide additional abstraction
// in case I wanted to add something to the board type.
typedef struct {
	uint8_t squares[MINIMAX_BOARD_ROWS][MINIMAX_BOARD_COLUMNS];  // State of game as passed in.
} minimax_board_t;

// A move is simply a row-column pair.
typedef struct {
	uint8_t row;
	uint8_t column;
} minimax_move_t;

// Define a score type.
typedef int16_t minimax_score_t;

// This is a move and score pair 
typedef struct {
	minimax_move_t move;
	minimax_score_t score;
} minimax_moveScore_pair;

// This routine is not recursive but will invoke the recursive minimax function.
// It computes the row and column of the next move based upon:
// the current board,
// the player. true means the computer is X. false means the computer is O.
void minimax_computeNextMove(minimax_board_t* board, bool player, uint8_t* row, uint8_t* column);

// Determine that the game is over by looking at the score.
bool minimax_isGameOver(minimax_score_t score);

// Returns the score of the board, based upon the player and the board.
// This returns one of 4 values: MINIMAX_PLAYER_WINNING_SCORE, 
// MINIMAX_OPPONENT_WINNING_SCORE, MINIMAX_DRAW_SCORE, MINIMAX_NOT_ENDGAME
int16_t minimax_computeBoardScore(minimax_board_t* board, bool player);

// Init the board to all empty squares.
void minimax_initBoard(minimax_board_t* board);

/* These are simply test functions which should eventually be DELETED */
void test_Board(minimax_board_t* board, bool player);
void printBoard(minimax_board_t* board);
bool board_isFull(minimax_board_t* board);

#endif /* MINIMAX_H) */


#include "miniMax.h"





#include <stdio.h>
#include <stdint.h>
#include <stdbool.h>
#include "miniMax.h"


int main()
{
	printf("Main has begun\n");
	minimax_board_t board1;  // Board 1 is the main example in the web-tutorial that I use on the web-site.
	board1.squares[0][0] = MINIMAX_OPPONENT_SQUARE;
	board1.squares[0][1] = MINIMAX_EMPTY_SQUARE;
	board1.squares[0][2] = MINIMAX_PLAYER_SQUARE;
	board1.squares[1][0] = MINIMAX_PLAYER_SQUARE;
	board1.squares[1][1] = MINIMAX_EMPTY_SQUARE;
	board1.squares[1][2] = MINIMAX_EMPTY_SQUARE;
	board1.squares[2][0] = MINIMAX_PLAYER_SQUARE;
	board1.squares[2][1] = MINIMAX_OPPONENT_SQUARE;
	board1.squares[2][2] = MINIMAX_OPPONENT_SQUARE;

	minimax_board_t board2;
	board2.squares[0][0] = MINIMAX_OPPONENT_SQUARE;
	board2.squares[0][1] = MINIMAX_EMPTY_SQUARE;
	board2.squares[0][2] = MINIMAX_PLAYER_SQUARE;
	board2.squares[1][0] = MINIMAX_EMPTY_SQUARE;
	board2.squares[1][1] = MINIMAX_EMPTY_SQUARE;
	board2.squares[1][2] = MINIMAX_EMPTY_SQUARE;
	board2.squares[2][0] = MINIMAX_PLAYER_SQUARE;
	board2.squares[2][1] = MINIMAX_EMPTY_SQUARE;
	board2.squares[2][2] = MINIMAX_OPPONENT_SQUARE;

	minimax_board_t board3;
	board3.squares[0][0] = MINIMAX_OPPONENT_SQUARE;
	board3.squares[0][1] = MINIMAX_EMPTY_SQUARE;
	board3.squares[0][2] = MINIMAX_EMPTY_SQUARE;
	board3.squares[1][0] = MINIMAX_OPPONENT_SQUARE;
	board3.squares[1][1] = MINIMAX_EMPTY_SQUARE;
	board3.squares[1][2] = MINIMAX_EMPTY_SQUARE;
	board3.squares[2][0] = MINIMAX_PLAYER_SQUARE;
	board3.squares[2][1] = MINIMAX_EMPTY_SQUARE;
	board3.squares[2][2] = MINIMAX_PLAYER_SQUARE;

	minimax_board_t board4;
	board4.squares[0][0] = MINIMAX_OPPONENT_SQUARE;
	board4.squares[0][1] = MINIMAX_EMPTY_SQUARE;
	board4.squares[0][2] = MINIMAX_EMPTY_SQUARE;
	board4.squares[1][0] = MINIMAX_EMPTY_SQUARE;
	board4.squares[1][1] = MINIMAX_EMPTY_SQUARE;
	board4.squares[1][2] = MINIMAX_EMPTY_SQUARE;
	board4.squares[2][0] = MINIMAX_PLAYER_SQUARE;
	board4.squares[2][1] = MINIMAX_EMPTY_SQUARE;
	board4.squares[2][2] = MINIMAX_PLAYER_SQUARE;

	minimax_board_t board5;
	board5.squares[0][0] = MINIMAX_PLAYER_SQUARE;
	board5.squares[0][1] = MINIMAX_PLAYER_SQUARE;
	board5.squares[0][2] = MINIMAX_EMPTY_SQUARE;
	board5.squares[1][0] = MINIMAX_EMPTY_SQUARE;
	board5.squares[1][1] = MINIMAX_OPPONENT_SQUARE;
	board5.squares[1][2] = MINIMAX_EMPTY_SQUARE;
	board5.squares[2][0] = MINIMAX_EMPTY_SQUARE;
	board5.squares[2][1] = MINIMAX_EMPTY_SQUARE;
	board5.squares[2][2] = MINIMAX_EMPTY_SQUARE;

	uint8_t row, column;
	bool player = true;


	/*	int16_t testScore = minimax_computeBoardScore(&board1, player);
	printf("Board score %d\n", testScore);
	printBoard(&board1);
	if (minimax_isGameOver(testScore))
	printf("Game over %d\n", testScore);*/
	minimax_computeNextMove(&board1, true, &row, &column);
	printf("next move for board1: (%d, %d)\n\r", row, column);
	printBoard(&board1);
	minimax_computeNextMove(&board2, true, &row, &column);
	printf("next move for board2: (%d, %d)\n\r", row, column);
	printBoard(&board2);
	minimax_computeNextMove(&board3, true, &row, &column);
	printf("next move for board3: (%d, %d)\n\r", row, column);
	printBoard(&board3);
	minimax_computeNextMove(&board4, false, &row, &column);
	printf("next move for board4: (%d, %d)\n\r", row, column);
	printBoard(&board4);
//	minimax_computeNextMove(&board5, false, &row, &column);
//	printf("next move for board5: (%d, %d)\n\r", row, column);
//	printBoard(&board5);

	minimax_board_t boardt;
	player = false;
	while (1) {
		printf("Entered While\n");
	test_Board(&boardt, player);
	}

	/*	THIS IS THE CORRECT ANSWER
	*	Board 1: (1,1)
	*	Board 2: (1,1)
	*	Board 3: (0,1)
	*	Board 4: (0,1)
	*	Board 5: (0,2)
	*/
	getchar();
}
//minimax_score_t score = MINIMAX_NOT_ENDGAME;
minimax_move_t choice;

/*****************************/
void printBoard(minimax_board_t* board) {
	printf("\n");
	for (int m = 0; m <= 2; m++) {
		for (int n = 0; n <= 2; n++) {
			if (board->squares[m][n] == MINIMAX_OPPONENT_SQUARE)
				printf("O ");
			else if (board->squares[m][n] == MINIMAX_PLAYER_SQUARE)
				printf("X ");
			else
				printf("_ ");
			if (n == 2)
				printf("\n");
		}
	}
	//printf("\n");
}

minimax_score_t minimax(minimax_board_t* board, bool player) {
	//	printBoard(&board);
	minimax_score_t score = minimax_computeBoardScore(board, !player);
	//	printf("Minimax called %d\n", score);
	//	printBoard(board);
	if (minimax_isGameOver(score)) { // Base case, if game is over compute and return the score
									 //	printf("Game is over\n");
									 //	printBoard(board);
		return score; // BUG MAYBE, SHOULD IT BE PLAYER OR NOT
					  //why not just return score?
	}
	minimax_moveScore_pair moveScoreTable[MINIMAX_BOARD_SIZE] = { 0 };

	int moveScoreTable_Index = 0;
	for (int m = 0; m <= 2; m++) {		//for all columns
		for (int n = 0; n <= 2; n++) {  // for all rows
										//			printf("Row %d\t Col %d\n", m, n);
			if (board->squares[m][n] == MINIMAX_EMPTY_SQUARE) {
				//Assign the square to player or opponent
				if (player)
					board->squares[m][n] = MINIMAX_PLAYER_SQUARE;	 // Assigns to X for testing
				else
					board->squares[m][n] = MINIMAX_OPPONENT_SQUARE;  // Or assigns to O for testing
				score = minimax(board, !player);					 // Recursive function to fill up the board
				moveScoreTable[moveScoreTable_Index].score = score;  // adds the score to the table
				moveScoreTable[moveScoreTable_Index].move.row = m;   // adds the coordinates of the move 
				moveScoreTable[moveScoreTable_Index].move.column = n;// to the table
																	 //				printf("\n");
																	 //				printf("Score for move %d %d is %d\n", moveScoreTable[moveScoreTable_Index].move.row, moveScoreTable[moveScoreTable_Index].move.column, moveScoreTable[moveScoreTable_Index].score);
				board->squares[m][n] = MINIMAX_EMPTY_SQUARE; //resets the board to an empty square, undoing the test
				moveScoreTable_Index++;
			}
		}
	}
	minimax_score_t tempScore = MINIMAX_DRAW_SCORE;
	if (player) {									   // X wants the highest score
		for (int i = 0; i < MINIMAX_BOARD_SIZE; i++) { // goes through Table
			if (moveScoreTable[i].score > tempScore) { // if score is higher
				tempScore = moveScoreTable[i].score;   // resets tempScore to be highest 
				score = moveScoreTable[i].score;	   // sets score to be highest
				choice.column = moveScoreTable[i].move.column; //sets choice column to be best column
				choice.row = moveScoreTable[i].move.row;	   //sets choice row to be best row
			}
		}
	}
	else {											   // O wants lowest score
		for (int i = 0; i < MINIMAX_BOARD_SIZE; i++) { // goes through Table
			if (moveScoreTable[i].score < tempScore) { // if score is lower
				tempScore = moveScoreTable[i].score;   // resets tempScore to be lowest 
				score = moveScoreTable[i].score;	   // sets score to be lowest
				choice.column = moveScoreTable[i].move.column; //sets choice column to be best column
				choice.row = moveScoreTable[i].move.row;	   //sets choice row to be best row
			}
		}
	}
	//	printf("%d %d %d\n", moveScoreTable->move.row,moveScoreTable->move.column, moveScoreTable->score);


	return score;
}
// This routine is not recursive but will invoke the recursive minimax function.
// It computes the row and column of the next move based upon:
// the current board,
// the player. true means the computer is X. false means the computer is O.
void minimax_computeNextMove(minimax_board_t* board, bool player, uint8_t* row, uint8_t* column) {

	minimax(board, player);
	*row = choice.row;
	*column = choice.column;
}

// Determine that the game is over by looking at the score.
bool minimax_isGameOver(minimax_score_t score) {
	if (score == MINIMAX_NOT_ENDGAME)
		return false; // Game is not over if score is still NOT_ENDGAME)
	else
		return true; // If score has changed, GAME IS OVER
}


bool board_isFull(minimax_board_t* board) {
	for (int m = 0; m <= 2; m++) {
		for (int n = 0; n <= 2; n++) {
			if (board->squares[m][n] == MINIMAX_EMPTY_SQUARE) //IS THIS LOGIC CORRECT?
				return false;
		}
	}
	return true;
}

// Returns the score of the board, based upon the player and the board.
// This returns one of 4 values: MINIMAX_PLAYER_WINNING_SCORE, 
// MINIMAX_OPPONENT_WINNING_SCORE, MINIMAX_DRAW_SCORE, MINIMAX_NOT_ENDGAME
int16_t minimax_computeBoardScore(minimax_board_t* board, bool player) {

	int16_t target;
	minimax_score_t winningScore = MINIMAX_NOT_ENDGAME;
	if (player) {			//If player is X, set function variables to his values
		target = MINIMAX_PLAYER_SQUARE;
		winningScore = MINIMAX_PLAYER_WINNING_SCORE;
		//		printf("Enter Compute BoardScore %d\n", target);
	}
	else {					//If player is O, set function variables to his values
		target = MINIMAX_OPPONENT_SQUARE;
		winningScore = MINIMAX_OPPONENT_WINNING_SCORE;
		//		printf("Enter Compute Score %d\n", target);
	}
	//checks top left to bottom right diagonal
	if ((board->squares[0][0] == target && board->squares[1][1] == target && board->squares[2][2] == target) ||
		// checks top right to bottom left diagonal
		(board->squares[0][2] == target && board->squares[1][1] == target && board->squares[2][0] == target)) {
		//	printf("Diagonal Score is %d", winningScore);
		//	printBoard(board);
		return winningScore;
	}								//Computes if Row 0 has a win
	else if ((board->squares[0][0] == target && board->squares[0][1] == target && board->squares[0][2] == target) ||
		//Computes if Row 1 has a win
		(board->squares[1][0] == target && board->squares[1][1] == target && board->squares[1][2] == target) ||
		//Computes if Row 2 has a win
		(board->squares[2][0] == target && board->squares[2][1] == target && board->squares[2][2] == target)) {
		//	printf("Row Score is %d", winningScore);
		//	printBoard(board);
		return winningScore;
	}								//Computes if Column 0 has a win
	else if ((board->squares[0][0] == target && board->squares[1][0] == target && board->squares[2][0] == target) ||
		//Computes if Column 1 has a win
		(board->squares[0][1] == target && board->squares[1][1] == target && board->squares[2][1] == target) ||
		//Computes if Column 2 has a win
		(board->squares[0][2] == target && board->squares[1][2] == target && board->squares[2][2] == target)) {
		//	printf("Column Score is %d", winningScore);
		//	printBoard(board);
		return winningScore;
	}
	else if (board_isFull(board)) {
		winningScore = MINIMAX_DRAW_SCORE;
	}
	else {
		winningScore = MINIMAX_NOT_ENDGAME;
	}

	//	printBoard(board);
	//	score = winningScore;
	//printf("winning %d\n", winningScore);
	return winningScore;
}

// Init the board to all empty squares.
void minimax_initBoard(minimax_board_t* board) {
	for (int m = 0; m <= 2; m++) {
		for (int n = 0; n <= 2; n++) {
			board->squares[m][n] = MINIMAX_EMPTY_SQUARE;
		}
	}
	printBoard(board);
}

void test_Board(minimax_board_t* board, bool player) {  //I think it is a bug in the code not the logic. Interesting bugs are happening
	int test_Row = 5;
	int test_Col = 5;
	uint8_t test_Row2 = 5;
	uint8_t test_Col2 = 5;
	printf("Entered test Board\n");
	printf("Enter: row column: ");
	scanf_s("%d %d", &test_Row, &test_Col);
	if (player)
		board->squares[test_Row][test_Col] = MINIMAX_PLAYER_SQUARE;
	else
		board->squares[test_Row][test_Col] = MINIMAX_OPPONENT_SQUARE;
	printBoard(board);
//	player = !player;
	minimax_computeNextMove(board, !player, &test_Row2, &test_Col2); // Make sure these are changing
	printf("New move should be %u %u\n", test_Row2, test_Col2);
	if (!player)
		board->squares[test_Row2][test_Col2] = MINIMAX_PLAYER_SQUARE;
	else
		board->squares[test_Row2][test_Col2] = MINIMAX_OPPONENT_SQUARE;
	printBoard(board);
}




#include <stdio.h>
#include <stdint.h>
#include <stdbool.h>
#include "miniMax.h"


int main()
{
	printf("Main has begun\n");
	minimax_board_t board1;  // Board 1 is the main example in the web-tutorial that I use on the web-site.
	board1.squares[0][0] = MINIMAX_OPPONENT_SQUARE;
	board1.squares[0][1] = MINIMAX_EMPTY_SQUARE;
	board1.squares[0][2] = MINIMAX_PLAYER_SQUARE;
	board1.squares[1][0] = MINIMAX_PLAYER_SQUARE;
	board1.squares[1][1] = MINIMAX_EMPTY_SQUARE;
	board1.squares[1][2] = MINIMAX_EMPTY_SQUARE;
	board1.squares[2][0] = MINIMAX_PLAYER_SQUARE;
	board1.squares[2][1] = MINIMAX_OPPONENT_SQUARE;
	board1.squares[2][2] = MINIMAX_OPPONENT_SQUARE;

	minimax_board_t board2;
	board2.squares[0][0] = MINIMAX_OPPONENT_SQUARE;
	board2.squares[0][1] = MINIMAX_EMPTY_SQUARE;
	board2.squares[0][2] = MINIMAX_PLAYER_SQUARE;
	board2.squares[1][0] = MINIMAX_EMPTY_SQUARE;
	board2.squares[1][1] = MINIMAX_EMPTY_SQUARE;
	board2.squares[1][2] = MINIMAX_EMPTY_SQUARE;
	board2.squares[2][0] = MINIMAX_PLAYER_SQUARE;
	board2.squares[2][1] = MINIMAX_EMPTY_SQUARE;
	board2.squares[2][2] = MINIMAX_OPPONENT_SQUARE;

	minimax_board_t board3;
	board3.squares[0][0] = MINIMAX_OPPONENT_SQUARE;
	board3.squares[0][1] = MINIMAX_EMPTY_SQUARE;
	board3.squares[0][2] = MINIMAX_EMPTY_SQUARE;
	board3.squares[1][0] = MINIMAX_OPPONENT_SQUARE;
	board3.squares[1][1] = MINIMAX_EMPTY_SQUARE;
	board3.squares[1][2] = MINIMAX_EMPTY_SQUARE;
	board3.squares[2][0] = MINIMAX_PLAYER_SQUARE;
	board3.squares[2][1] = MINIMAX_EMPTY_SQUARE;
	board3.squares[2][2] = MINIMAX_PLAYER_SQUARE;

	minimax_board_t board4;
	board4.squares[0][0] = MINIMAX_OPPONENT_SQUARE;
	board4.squares[0][1] = MINIMAX_EMPTY_SQUARE;
	board4.squares[0][2] = MINIMAX_EMPTY_SQUARE;
	board4.squares[1][0] = MINIMAX_EMPTY_SQUARE;
	board4.squares[1][1] = MINIMAX_EMPTY_SQUARE;
	board4.squares[1][2] = MINIMAX_EMPTY_SQUARE;
	board4.squares[2][0] = MINIMAX_PLAYER_SQUARE;
	board4.squares[2][1] = MINIMAX_EMPTY_SQUARE;
	board4.squares[2][2] = MINIMAX_PLAYER_SQUARE;

	minimax_board_t board5;
	board5.squares[0][0] = MINIMAX_PLAYER_SQUARE;
	board5.squares[0][1] = MINIMAX_PLAYER_SQUARE;
	board5.squares[0][2] = MINIMAX_EMPTY_SQUARE;
	board5.squares[1][0] = MINIMAX_EMPTY_SQUARE;
	board5.squares[1][1] = MINIMAX_OPPONENT_SQUARE;
	board5.squares[1][2] = MINIMAX_EMPTY_SQUARE;
	board5.squares[2][0] = MINIMAX_EMPTY_SQUARE;
	board5.squares[2][1] = MINIMAX_EMPTY_SQUARE;
	board5.squares[2][2] = MINIMAX_EMPTY_SQUARE;

	uint8_t row, column;
	bool player = true;


	/*	int16_t testScore = minimax_computeBoardScore(&board1, player);
	printf("Board score %d\n", testScore);
	printBoard(&board1);
	if (minimax_isGameOver(testScore))
	printf("Game over %d\n", testScore);*/
	minimax_computeNextMove(&board1, true, &row, &column);
	printf("next move for board1: (%d, %d)\n\r", row, column);
	printBoard(&board1);
	minimax_computeNextMove(&board2, true, &row, &column);
	printf("next move for board2: (%d, %d)\n\r", row, column);
	printBoard(&board2);
	minimax_computeNextMove(&board3, true, &row, &column);
	printf("next move for board3: (%d, %d)\n\r", row, column);
	printBoard(&board3);
	minimax_computeNextMove(&board4, false, &row, &column);
	printf("next move for board4: (%d, %d)\n\r", row, column);
	printBoard(&board4);
//	minimax_computeNextMove(&board5, false, &row, &column);
//	printf("next move for board5: (%d, %d)\n\r", row, column);
//	printBoard(&board5);

	minimax_board_t boardt;
	player = false;
	while (1) {
		printf("Entered While\n");
	test_Board(&boardt, player);
	}

	/*	THIS IS THE CORRECT ANSWER
	*	Board 1: (1,1)
	*	Board 2: (1,1)
	*	Board 3: (0,1)
	*	Board 4: (0,1)
	*	Board 5: (0,2)
	*/
	getchar();
}
