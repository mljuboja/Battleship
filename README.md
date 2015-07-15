# battleship.c
The function source file of the text-based version of Battleship in C programming language.

/*********************************************
* Project: Battleship                        *
* Author: Marina Ljuboja                     *
*                                            *
* Description: User plays the computer in a  *
* game of battleship. Each player has a 10 x *
* 10 board with 5 ships. The first to sink   *
* all of the opponents ships wins. Each move *
* and stats will be collected in a separate  *
* file.                                      *
*                                            *
**********************************************/

#include "battleship.h"

/***********************************************************/
/*Displays welcome message as well as the rules of the game*/
/***********************************************************/
void welcome_message (void)
{
	printf("*********************************************\n\n");
	printf("********** Welcome to Battleship! ***********\n\n");
	printf("*********************************************\n\n");
	printf("******** You will play the computer *********\n\n");
	printf("******** to see who sinks all of the ********\n\n");
	printf("******** opponent's ships first. ************\n\n");
	printf("*********************************************\n\n");
	system("pause");
	system("cls");
}


/*****************************************************************/
/*Initializes the 10 x 10 board. Will be called for both players.*/
/*****************************************************************/
void intialize_gameboard (char gameboard[MAX_ROWS][MAX_COLS], int rows, int cols)
{
	int index_rows = 0, index_cols = 0;

	for (index_rows = 0; index_rows < rows; index_rows++)
	{
		for (index_cols = 0; index_cols < cols; index_cols++)
		{
			gameboard[index_rows][index_cols] = '~';
		}
	}
}


/****************************************/
/*Prints the gameboard                  */
/****************************************/
void print_gameboard (char gameboard[MAX_ROWS][MAX_COLS], int rows, int cols, int show)
{
	int index_cols = 0, index_rows = 0;

	printf("  ");

	for (index_cols = 0; index_cols < cols; index_cols++)
	{
		printf("%d ", index_cols);
	}

	printf("\n");

	if (show)
	{
		for (index_rows = 0; index_rows < rows; index_rows++)
		{
			printf("%d ", index_rows);

			for (index_cols = 0; index_cols < cols; index_cols++)
			{
				printf("%c ", gameboard[index_rows][index_cols]);
			}

			printf("\n\n");
		}
	}

	else
	{
		for (index_rows = 0; index_rows < rows; index_rows++)
		{
			printf("%d ", index_rows);

			for (index_cols = 0; index_cols < cols; index_cols++)
			{
				if ((gameboard[index_rows][index_cols] == 'c') || (gameboard[index_rows][index_cols] == 'b') || 
					(gameboard[index_rows][index_cols] == 'r') || (gameboard[index_rows][index_cols] == 's') || 
					(gameboard[index_rows][index_cols] == 'd'))
				{
					printf("~ ");
				}
				else
				{
					printf("%c ", gameboard[index_rows][index_cols]);
				}
			}
			printf("\n\n");
		}
	}
}


/************************************************/
/*Asks player 1 how they want to place the ships*/
/************************************************/
void p1_ship_placement (char gameboard[MAX_ROWS][MAX_COLS])
{
	int placement_type = 0;

	do
	{
		printf("\nHow would you like your ships placed?\n\n");
		printf("1. Manually.\n");
		printf("2. Randomly.\n\n");
		scanf("%d", &placement_type);
		system("cls");
	
		if (placement_type == 1)
		{
			manually_place_ships_on_board(gameboard);
		}

		else if (placement_type == 2)
		{
			randomly_place_ships_on_board(gameboard, 1, 1);
		}

		else
		{
			printf("\nWrong input. Choose from 1 or 2\n\n");
		}
	} while ((placement_type < 1) || (placement_type > 2));
}


/********************************************************************/
/*If player 1 chooses to manually place the ships then this function*/ 
/*will be called                                                    */
/********************************************************************/
void manually_place_ships_on_board(char gameboard[MAX_ROWS][MAX_COLS])
{
	char ship_direction = '\0';

	int i = 0, j = 0, x1 = 0, y1 = 0, valid = 0;

	/*The carrier (5) will be placed first..*/
	system("pause");
	system("cls");
	printf("First let's place the carrier.\n");
	printf("Type 'a' for across and 'v' for vertical.\n");

	while ((ship_direction != 'a') && (ship_direction != 'v'))
	{
		scanf(" %c", &ship_direction);
	
		if (ship_direction == 'a')
		{
			do 
			{
				valid = 0;
				printf("\nEnter the coordinates.\n");
				scanf("%d%d", &x1, &y1);

				for (j = y1; (j < (y1 + CARRIER) && (valid == 0)); j++)
				{
					if ((x1 < 0) || (y1 < 0) || (x1 > 9) || (y1 > 9) || ((x1 + CARRIER) > 9))
					{
						printf("The ship is off the board. Try again.\n\n");
						valid = 1;
					}
					else
					{
						gameboard[x1][j] = 'c'; 
					}
				}
			 } while ((x1 < 0) || (y1 < 0) || (x1 > 9) || (y1 > 9) || ((x1 + CARRIER) > 9) || (valid == 1));
		}

		else if (ship_direction == 'v')
		{
			do 
			{
				valid = 0;
				printf("\nEnter the coordinates.\n");
				scanf("%d%d", &x1, &y1);

				for (i = x1; i < (x1 + CARRIER) && (valid == 0); i++)
				{
					if((x1 < 0) || (y1 < 0) || (x1 > 9) || (y1 > 9) || ((y1 + CARRIER) > 9))
					{
						printf("The ship is off the board. Try again.\n\n");
						valid = 1;
					}
					else
					{
						gameboard[i][y1] = 'c';
					}
				}

			 } while ((x1 < 0) || (y1 < 0) || (x1 > 9) || (y1 > 9) || ((y1 + CARRIER) > 9) || (valid == 1));
		}

		else
		{
			printf("\nInvalid character. Choose from 'a' and 'v' only.\n\n");
		}
	}
	system("pause");
	system("cls");
	print_gameboard (gameboard, 10, 10, 1);

	ship_direction = '\0'; /* reinitializing the ship direction*/
	valid = 0;

	/*...then the battleship (4)...*/
	system("pause");
	system("cls");
	printf("Now onto the battleship.\n");
	printf("Type 'a' for across and 'v' for vertical.\n");

	while ((ship_direction != 'a') && (ship_direction != 'v'))
	{
		scanf(" %c", &ship_direction);
	
		if (ship_direction == 'a')
		{
			do 
			{
				valid = 0;
				printf("\nEnter the coordinates.\n");
				scanf("%d%d", &x1, &y1);

				for (j = y1; (j < (y1 + BATTLESHIP) && (valid == 0)); j++)
				{
					if (gameboard[x1][j] == 'c')
					{
						printf("The ship is stacked on another ship. Try again.\n\n");
						valid = 1;
					}

					else if ((x1 < 0) || (y1 < 0) || (x1 > 9) || (y1 > 9) || ((x1 + BATTLESHIP) > 9))
					{
						printf("The ship is off the board. Try again.\n\n");
						valid = 1;
					}

					else
					{
						gameboard[x1][j] = 'b';
					}
				}

			 } while ((x1 < 0) || (y1 < 0) || (x1 > 9) || (y1 > 9) || ((x1 + BATTLESHIP) > 9) || (valid == 1));

		}

		else if (ship_direction == 'v')
		{
			do 
			{
				valid = 0;
				printf("\nEnter the coordinates.\n");
				scanf("%d%d", &x1, &y1);

				for (i = x1; (i < (x1 + BATTLESHIP)) && (valid == 0); i++)
				{
					if((x1 < 0) || (y1 < 0) || (x1 > 9) || (y1 > 9) || ((y1 + BATTLESHIP) > 9))
					{
						printf("The ship is off the board. Try again.\n\n");
						valid = 1;
					}

					else if (gameboard[i][y1] == 'c')
					{
						printf("The ship is stacked on another ship. Try again.\n\n");
						valid = 1;
					}
					else 
					{
						gameboard[i][y1] = 'b';
					}
				}

			} while ((x1 < 0) || (y1 < 0) || (x1 > 9) || (y1 > 9) || ((y1 + BATTLESHIP) > 9) || (valid == 1));

		}

		else 
		{
			printf("\nInvalid character. Choose from 'a' and 'v' only.\n\n");
		}
	}

	system("pause");
	system("cls");
	print_gameboard (gameboard, 10, 10, 1);

	ship_direction = '\0';
	valid = 0;

	/*...then the cruiser (3)...*/
	system("pause");
	system("cls");
	printf("\nNow onto the cruiser.\n");
	printf("Type 'a' for across and 'v' for vertical.\n");

	while ((ship_direction != 'a') && (ship_direction != 'v'))
	{
		scanf(" %c", &ship_direction);
	
		if (ship_direction == 'a')
		{
			do 
			{
				valid = 0;
				printf("\nEnter the coordinates.\n");
				scanf("%d%d", &x1, &y1);

				for (j = y1; (j < (y1 + CRUISER)) && (valid == 0); j++)
				{
					if ((x1 < 0) || (y1 < 0) || (x1 > 9) || (y1 > 9) || ((x1 + CRUISER) > 9))
					{
						printf("The ship is off the board. Try again.\n\n");
						valid = 1;
					}

					else if ((gameboard[x1][j] == 'c') || (gameboard[x1][j] == 'b'))
					{
						printf("The ship is stacked on another ship. Try again.\n\n");
						valid = 1;
					}

					else
					{
						gameboard[x1][j] = 'r';
					}
				}

			 } while ((x1 < 0) || (y1 < 0) || (x1 > 9) || (y1 > 9) || ((x1 + CRUISER) > 9) || (valid == 1));

			
		}

		else if (ship_direction == 'v')
		{
			do 
			{
				valid = 0;
				printf("\nEnter the coordinates.\n");
				scanf("%d%d", &x1, &y1);

				for (i = x1; (i < (x1 + CRUISER)) && (valid == 0); i++)
				{
					if((x1 < 0) || (y1 < 0) || (x1 > 9) || (y1 > 9) || ((y1 + CRUISER) > 9))
					{
						printf("The ship is off the board. Try again.\n\n");
						valid = 1;
					}

					else if ((gameboard[i][y1] == 'c') || (gameboard[i][y1] == 'b'))
					{
						printf("The ship is stacked on another ship. Try again.\n\n");
						valid = 1;
					}

					else
					{
						gameboard[i][y1] = 'r';
					}
				}

			} while ((x1 < 0) || (y1 < 0) || (x1 > 9) || (y1 > 9) || ((y1 + CRUISER) > 9) || (valid == 1));

		}

		else 
		{
			printf("\nInvalid character. Choose from 'a' and 'v' only.\n\n");
		}
	}

	system("pause");
	system("cls");
	print_gameboard (gameboard, 10, 10, 1);

	ship_direction = '\0';
	valid = 0;

	/*...then the submarine (3)...*/
	system("pause");
	system("cls");
	printf("\nNow onto the submarine.\n");
	printf("Type 'a' for across and 'v' for vertical.\n");

	while ((ship_direction != 'a') && (ship_direction != 'v'))
	{
		scanf(" %c", &ship_direction);
	
		if (ship_direction == 'a')
		{
			do 
			{
				valid = 0;
				printf("\nEnter the coordinates.\n");
				scanf("%d%d", &x1, &y1);

				for (j = y1; (j < (y1 + SUBMARINE)) && (valid == 0); j++)
				{
					if ((x1 < 0) || (y1 < 0) || (x1 > 9) || (y1 > 9) || ((x1 + SUBMARINE) > 9))
					{
						printf("The ship is off the board. Try again.\n\n");
						valid = 1;
					}

					else if ((gameboard[x1][j] == 'c') || (gameboard[x1][j] == 'b') || (gameboard[x1][j] == 'r'))
					{
						printf("The ship is stacked on another ship. Try again.\n\n");
						valid = 1;
					}

					else
					{
						gameboard[x1][j] = 's';
					}
				}

			 } while ((x1 < 0) || (y1 < 0) || (x1 > 9) || (y1 > 9) || ((x1 + SUBMARINE) > 9) || (valid == 1));

		}

		else if (ship_direction == 'v')
		{
			do 
			{
				valid = 0;
				printf("\nEnter the coordinates.\n");
				scanf("%d%d", &x1, &y1);

				for (i = x1; (i < (x1 + SUBMARINE)) && (valid == 0); i++)
				{
					if((x1 < 0) || (y1 < 0) || (x1 > 9) || (y1 > 9) || ((y1 + SUBMARINE) > 9))
					{
						printf("The ship is off the board. Try again.\n\n");
						valid = 1;
					}

					else if ((gameboard[i][y1] == 'c') || (gameboard[i][y1] == 'b') || (gameboard[i][y1] == 'r'))
					{
						printf("The ship is stacked on another ship. Try again.\n\n");
						valid = 1;
					}

					else
					{
						gameboard[i][y1] = 's';
					}
				}

			} while ((x1 < 0) || (y1 < 0) || (x1 > 9) || (y1 > 9) || ((y1 + SUBMARINE) > 9) || (valid == 1));

		}

		else 
		{
			printf("\nInvalid character. Choose from 'a' and 'v' only.\n\n");
		}
	}

	system("pause");
	system("cls");
	print_gameboard (gameboard, 10, 10, 1);

	ship_direction = '\0';
	valid = 0;

	/*...then the destroyer (2)...*/
	system("pause");
	system("cls");
	printf("\nNow onto the destroyer.\n");
	printf("Type 'a' for across and 'v' for vertical.\n");

	while ((ship_direction != 'a') && (ship_direction != 'v'))
	{
		scanf(" %c", &ship_direction);
	
		if (ship_direction == 'a')
		{
			do 
			{
				valid = 0;
				printf("\nEnter the coordinates.\n");
				scanf("%d%d", &x1, &y1);

				for (j = y1; (j < (y1 + DESTROYER)) && (valid == 0); j++)
				{
					if ((x1 < 0) || (y1 < 0) || (x1 > 9) || (y1 > 9) || ((x1 + DESTROYER) > 9))
					{
						printf("The ship is off the board.\n\n");
						valid = 1;
					}

					else if ((gameboard[x1][j] == 'c') || (gameboard[x1][j] == 'b') || (gameboard[x1][j] == 'r') || (gameboard[x1][j] == 's'))
					{
						printf("The ship is stacked on another ship. Try again.\n\n");
						valid = 1;
					}

					else
					{
						gameboard[x1][j] = 'd';
					}
				}

			 } while ((x1 < 0) || (y1 < 0) || (x1 > 9) || (y1 > 9) || ((x1 + DESTROYER) > 9) || (valid == 1));

			
		}

		else if (ship_direction == 'v')
		{
			do 
			{
				valid = 0;
				printf("\nEnter the coordinates.\n");
				scanf("%d%d", &x1, &y1);

				for (i = x1; (i < (x1 + DESTROYER)) && (valid == 0); i++)
				{
					if((x1 < 0) || (y1 < 0) || (x1 > 9) || (y1 > 9) || ((y1 + DESTROYER) > 9))
					{
						printf("The ship is off the board.\n\n");
						valid = 1;
					}

					else if ((gameboard[i][y1] == 'c') || (gameboard[i][y1] == 'b') || (gameboard[i][y1] == 'r') || (gameboard[i][y1] == 's'))
					{
						printf("The ship is stacked on another ship. Try again.\n\n");
						valid = 1;
					}

					else 
					{
						gameboard[i][y1] = 'd';
					}
				}

			} while ((x1 < 0) || (y1 < 0) || (x1 > 9) || (y1 > 9) || ((y1 + DESTROYER) > 9) || (valid == 1));
		}

		else 
		{
			printf("\nInvalid character. Choose from 'a' and 'v' only.\n\n");
		}
	}

	system("pause");
	system("cls");
}


/*********************************************************************/
/*Randomly places ships on the board. Automatically done for player 2*/
/*********************************************************************/
void randomly_place_ships_on_board(char gameboard[MAX_ROWS][MAX_COLS], int which_player, int show)
{
	char ship_direction = '\0';

	int ship_coordinates[MAX_COLS];

	int i = 0, j = 0, x1 = 0, y1 = 0, valid = 0, k = 0, l = 0;

	srand(time(NULL));

	/*The carrier (5) will be placed first..*/
	ship_direction = (rand() % 2) + 1;

	if (ship_direction == 1)
	{
		do
		{
			valid = 0;
			do 
			{
				x1 = rand() % 10;
				y1 = rand() % 10;

			} while ((x1 < 0) || (y1 < 0) || (x1 > 9) || (y1 > 9) || ((x1 + CARRIER) > 10));
	

			for (j = y1; (j < (y1 + CARRIER)) && (valid == 0); j++)
			{
				if (j > 9)
				{
					for (k = j; k >= y1; --k)
					{
						gameboard[x1][k] = '~';
					}
					valid = 1;
				}
				else
				{
					gameboard[x1][j] = 'c';
				}
			}
		} while (valid == 1);
	}

	else if (ship_direction == 2)
	{
		do
		{
			valid = 0;
			do
			{
				x1 = rand() % 10;
				y1 = rand() % 10;
				
			} while ((x1 < 0) || (y1 < 0) || (x1 > 9) || (y1 > 9) || ((y1 + CARRIER) > 10));

			for (i = x1; (i < (x1 + CARRIER)) && (valid == 0); i++)
			{
				if (i > 9)
				{
					for (l = i; l >= x1; --l)
					{
						gameboard[l][y1] = '~';
					}
					valid = 1;
				}
				else
				{
					gameboard[i][y1] = 'c';
				}
			}

		} while (valid == 1);

	}

	ship_direction = '\0'; 
	x1 = 0; 
	y1 = 0;
	j = 0;
	i = 0;
	k = 0;
	l = 0;
	valid = 0;
	
	/*Battleship:*/
	ship_direction = (rand() % 2) + 1;

	if (ship_direction == 1)
	{
		do 
		{
			valid = 0;
			do 
			{
				x1 = rand() % 10;
				y1 = rand() % 10;

			} while ((x1 < 0) || (y1 < 0) || (x1 > 9) || (y1 > 9) || ((x1 + BATTLESHIP) > 10));

			for (j = y1; (j < (y1 + BATTLESHIP)) && (valid == 0); j++)
			{
				if ((gameboard[x1][j] == 'c') || (j > 9))
				{
					for (k = (j-1); k >= y1; k--)
					{
						gameboard[x1][k] = '~';
					}
					valid = 1;
				}

				else 
				{
					gameboard[x1][j] = 'b';
				}
			}
		} while (valid == 1);
	}

	else if (ship_direction == 2)
	{
		do
		{
			valid = 0;
			do
			{
				x1 = rand() % 10;
				y1 = rand() % 10;

			} while ((x1 < 0) || (y1 < 0) || (x1 > 9) || (y1 > 9) || ((y1 + BATTLESHIP) > 10));

			for (i = x1; (i < (x1 + BATTLESHIP)) && (valid == 0); i++)
			{
 				if ((gameboard[i][y1] == 'c') || (i > 9))
				{
					for (l = (i-1); l >= x1; l--)
					{
						gameboard[l][y1] = '~';
					}
					valid = 1;
	
				}
				else 
				{
					gameboard[i][y1] = 'b';
				}
			}
		} while (valid == 1);
	}

	ship_direction = '/0';
	x1 = 0; 
	y1 = 0;
	j = 0;
	i = 0;
	k = 0;
	l = 0;
	valid = 0;

	/*Cruiser*/
	ship_direction = (rand() % 2) + 1;

	if (ship_direction == 1)
	{
		do 
		{
			valid = 0;
			do 
			{
				x1 = rand() % 10;
				y1 = rand() % 10;

			} while ((x1 < 0) || (y1 < 0) || (x1 > 9) || (y1 > 9) || ((x1 + CRUISER) > 10));

			for (j = y1; (j < (y1 + CRUISER)) && (valid == 0); j++)
			{
				if ((gameboard[x1][j] == 'c') || (gameboard[x1][j] == 'b') || (j > 9))
				{
					for (k = (j-1); k >= y1; k--)
					{
						gameboard[x1][k] = '~';
					}
					valid = 1;
				}

				else 
				{
					gameboard[x1][j] = 'r';
				}
			}
		} while (valid == 1);
	}

	else if (ship_direction == 2)
	{
		do
		{
			valid = 0;
			do 
			{
				x1 = rand() % 10;
				y1 = rand() % 10;

			} while ((x1 < 0) || (y1 < 0) || (x1 > 9) || (y1 > 9) || ((y1 + CRUISER) > 10));

			for (i = x1; (i < (x1 + CRUISER)) && (valid == 0); i++)
			{
				if ((gameboard[i][y1] == 'c') || (gameboard[i][y1] == 'b') || (i > 9))
				{
					for (l = (i-1); l >= x1; l--)
					{
						gameboard[l][y1] = '~';
					}
					valid = 1;
				}
				else 
				{
					gameboard[i][y1] = 'r';
				}
			}
		} while (valid == 1);
	}

	ship_direction = '/0';
	x1 = 0; 
	y1 = 0;
	j = 0;
	i = 0;
	k = 0;
	l = 0;
	valid = 0;

	/*Submarine*/
	ship_direction = (rand() % 2) + 1;

	if (ship_direction == 1)
	{
		do 
		{
			valid = 0;
			do 
			{
				x1 = rand() % 10;
				y1 = rand() % 10;

			} while ((x1 < 0) || (y1 < 0) || (x1 > 9) || (y1 > 9) || ((x1 + SUBMARINE) > 10));

			for (j = y1; (j < (y1 + SUBMARINE)) && (valid == 0); j++)
			{
				if ((gameboard[x1][j] == 'c') || (gameboard[x1][j] == 'b') || (gameboard[x1][j] == 'r') || (j > 9))
				{
					for (k = (j - 1); k >= y1; k--)
					{
						gameboard[x1][k] = '~';
					}
					valid = 1;
				}

				else 
				{
					gameboard[x1][j] = 's';
				}
			}
		} while (valid == 1);
	}

	else if (ship_direction == 2)
	{
		do
		{
			valid = 0;
			do
			{
				x1 = rand() % 10;
				y1 = rand() % 10;
			} while ((x1 < 0) || (y1 < 0) || (x1 > 9) || (y1 > 9) || ((y1 + SUBMARINE) > 10));

			for (i = x1; (i < (x1 + SUBMARINE)) && (valid == 0); i++)
			{
				if ((gameboard[i][y1] == 'c') || (gameboard[i][y1] == 'b') || (gameboard[i][y1] == 'r') || (i > 9))
				{
					for (l = (i - 1); l >= x1; l--)
					{
						gameboard[l][y1] = '~';
					}
					valid = 1;
				}
				else 
				{
					gameboard[i][y1] = 's';
				}
			}
		} while (valid == 1);
	}

	ship_direction = '/0';
	x1 = 0; 
	y1 = 0;
	j = 0;
	i = 0;
	k = 0;
	l = 0;
	valid = 0;

	/*Destroyer*/
	ship_direction = (rand() % 2) + 1;

	if (ship_direction == 1)
	{
		do 
		{
			valid = 0;
			do 
			{
				x1 = rand() % 10;
				y1 = rand() % 10;

			} while ((x1 < 0) || (y1 < 0) || (x1 > 9) || (y1 > 9) || ((x1 + DESTROYER) > 10));

			for (j = y1; (j < (y1 + DESTROYER)) && (valid == 0); j++)
			{
				if ((gameboard[x1][j] == 'c') || (gameboard[x1][j] == 'b') || (gameboard[x1][j] == 'r') || (gameboard[x1][j] == 's') || (j > 9))
				{
					for (k = (j - 1); k >= y1; k--)
					{
						gameboard[x1][k] = '~';
					}
					valid = 1;
				}

				else 
				{
					gameboard[x1][j] = 'd';
				}
			}
		} while (valid == 1);
	}

	else if (ship_direction == 2)
	{
		do
		{
			valid = 0;
			do
			{
				x1 = rand() % 10;
				y1 = rand() % 10;

			} while ((x1 < 0) || (y1 < 0) || (x1 > 9) || (y1 > 9) || ((y1 + DESTROYER) > 10));

			for (i = x1; (i < (x1 + DESTROYER)) && (valid == 0); i++)
			{
				if ((gameboard[i][y1] == 'c') || (gameboard[i][y1] == 'b') || (gameboard[i][y1] == 'r') || (gameboard[i][y1] == 's') || (i > 9))
				{
					for (l = (i - 1); l >= x1; l--)
					{
						gameboard[l][y1] = '~';
					}
					valid = 1;
				}
				else 
				{
					gameboard[i][y1] = 'd';
				}
			}
		} while (valid == 1);
	}
}


/***************************************************/
/* Checks if player 1 hits or misses               */
/***************************************************/
void check_enemy (char gameboard[MAX_ROWS][MAX_COLS], int x1, int y1, player_stats *stats, FILE *outfile)
{
	int valid = 0;

	do
	{
		valid = 0;
		printf("Enter the coordinates of the target.\n\n");
		scanf("%d%d", &x1, &y1);

		if ((gameboard[x1][y1] == 'c') || (gameboard[x1][y1] == 'b') || (gameboard[x1][y1] == 'r') || (gameboard[x1][y1] == 's') || (gameboard[x1][y1] == 'd'))
		{
			gameboard[x1][y1] = '*';
			printf("You hit a ship!\n\n");
			print_gameboard(gameboard, 10, 10, 0);
			stats->total_hits += 1;
			stats->total_shots += 1;
			fprintf(outfile, "\n    Player 1: %d,%d hit.\n", x1, y1);
			valid = 0;
		}

		else if (gameboard[x1][y1] == '~')
		{
			gameboard[x1][y1] = 'm';
			printf("Your shot was a miss.\n\n");
			print_gameboard(gameboard, 10, 10, 0);
			stats->total_misses += 1;
			stats->total_shots += 1;
			fprintf(outfile, "\n    Player 1: %d,%d miss.\n", x1, y1);
			valid = 0;
		}

		else if ((gameboard[x1][y1] == '*') || (gameboard[x1][y1] == 'm'))
		{
			printf("You already chose these coordinates. Try again.\n\n");
			valid = 1;
		}

		else
		{ 
			printf("The coordinates are off the board. Try again.\n\n");
			valid = 1;
		}

	} while (valid == 1);
	
	system("pause");
	system("cls");
}


/***************************************************/
/* Check if enemy hits or misses                   */
/***************************************************/
void check_player (char gameboard[MAX_ROWS][MAX_COLS], int x1, int y1, player_stats *stats, FILE *outfile)
{
	int valid = 0;

	do
	{
		valid = 0;
		x1 = rand() % 10;
		y1 = rand() % 10;

		if ((gameboard[x1][y1] == 'c') || (gameboard[x1][y1] == 'b') || (gameboard[x1][y1] == 'r') || (gameboard[x1][y1] == 's') || (gameboard[x1][y1] == 'd'))
		{
			gameboard[x1][y1] = '*';
			printf("Enemy has made a hit.\n\n");
			print_gameboard(gameboard, 10, 10, 0);
			stats->total_hits +=1;
			stats->total_shots +=1;
			fprintf(outfile, "\n    Player 2: %d,%d hit.\n", x1, y1);
			valid = 0;
		}

		else if (gameboard[x1][y1] == '~')
		{
			gameboard[x1][y1] = 'm';
			printf("Enemy's shot was a miss.\n\n");
			print_gameboard(gameboard, 10, 10, 0);
			stats->total_misses += 1;
			stats->total_shots += 1;
			fprintf(outfile, "\n    Player 2: %d,%d miss.\n", x1, y1);
			valid = 0;
		}

		else if ((gameboard[x1][y1] == '*') || (gameboard[x1][y1] == 'm'))
		{
			valid = 1;
		}

	} while (valid == 1);
	
	system("pause");
	system("cls");
}


/***************************************************/
/* Function checks if the ship is sunk and outputs */
/* to file.                                        */
/***************************************************/
void check_sink (char gameboard[MAX_ROWS][MAX_COLS], int player, player_stats *stats, FILE *outfile, int *sink)
{
	int i = 0, j = 0, c = 0, b = 0, r = 0, s = 0, d = 0;

	for (i = 0; i < 10; i++)
	{
		for (j = 0; j < 10; j++)
		{
			if (gameboard[i][j] == 'c')
			{
				c++;
			}
			else if (gameboard[i][j] == 'b')
			{
				b++;
			}
			else if (gameboard[i][j] == 'r')
			{
				r++;
			}
			else if (gameboard[i][j] == 's')
			{
				s++;
			}
			else if (gameboard[i][j] == 'd')
			{
				d++;
			}
		}
	}

	if (player == 1)
	{
		if ((c == 0) && (sink[0] != 1))
		{
			printf("You sank the enemy's carrier.\n\n");
			sink[0] = 1;
			fprintf(outfile, "\n    Player 1 sank the carrier.\n");
		}

		if ((b == 0) && (sink[1] != 1))
		{
			printf("You sank the enemy's battleship.\n\n");
			sink[1] = 1;
			fprintf(outfile, "\n    Player 1 sank the battleship.\n");
		}

		if ((r == 0) && (sink[2] != 1))
		{
			printf("You sank the enemy's cruiser.\n\n");
			sink[2] = 1;
			fprintf(outfile, "\n    Player 1 sank the cruiser.\n");
		}

		if ((s == 0) && (sink[3] != 1))
		{
			printf("You sank the enemy's submarine.\n\n");
			sink[3] = 1;
			fprintf(outfile, "\n    Player 1 sank the submarine.\n");
		}

		if ((d == 0) && (sink[4] != 1))
		{
			printf("You sank the enemy's destroyer.\n\n");
			sink[4] = 1;
			fprintf(outfile, "\n    Player 1 sank the destroyer.\n");
		}

		system("pause");
		system("cls");
	}

	else
	{
			if ((c == 0) && (sink[0] != 1))
			{
				printf("Enemy sank your carrier.\n\n");
				sink[0] = 1;
				fprintf(outfile, "\n    Player 2 sank the carrier.\n");
			}

			if ((b == 0) && (sink[1] != 1))
			{
				printf("Enemy sank your battleship.\n\n");
				sink[1] = 1;
				fprintf(outfile, "\n    Player 2 sank the battleship.\n");
			}

			if ((r == 0) && (sink[2] != 1))
			{
				printf("Enemy sank your cruiser.\n\n");
				sink[2] = 1;
				fprintf(outfile, "\n    Player 2 sank the cruiser.\n");
			}

			if ((s == 0) && (sink[3] != 1))
			{
				printf("Enemy sank your submarine.\n\n");
				sink[3] = 1;
				fprintf(outfile, "\n    Player 2 sank the submarine.\n");
			}

			if ((d == 0) && (sink[4] != 1))
			{
				printf("Enemy sank your destroyer.\n\n");
				sink[4] = 1;
				fprintf(outfile, "\n    Player 2 sank the destroyer.\n");
			}

		system("pause");
		system("cls");
	}
}
