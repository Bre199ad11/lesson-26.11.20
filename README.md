#include <iostream>
#include <cstdlib>
#include <ctime>
#include <conio.h>
using namespace std;
const int field_size = 4;
int field[field_size][field_size];
int score;

enum direction {
	north,
	east,
	south,
	west
};


void print_field() {
	cout << endl;
	for (int i = 0; i < field_size; i++) {
		for (int j = 0; j < field_size; j++) {
			cout << field[i][j] << "\t";

		}
		cout << endl;
	}
	cout << endl << "Score: " << score << endl;
}

void init_field() {
	int i, j;
	i = rand() % field_size;
	j = rand() % field_size;
	field[i][j] = 4;
	i = rand() % field_size;
	j = rand() % field_size;
	field[i][j] = 2;
	i = rand() % field_size;
	j = rand() % field_size;
	field[i][j] = 4;
}

bool has_free_cells() {
	for (int i = 0; i < field_size; i++) {
		for (int j = 0; j < field_size; j++) {
			if (field[i][j] == 0) {
				return true;
			}
		}
	}
	return false;
}


direction read_direction() {
	input:
	int c=_getch();
	switch (c)
	{
	case 'w':
		return north;
	case 's':
		return south;
	case 'a':
		return west;
	case 'd':
		return east;
	default:
		goto input; //переход к метке input
	}
}

void shift(direction direction) {
	switch (direction) {
	case north:
		for (int j = 0; j < field_size; j++) {
			for (int i = 0; i < field_size - 1; i++) {
				if (field[i][j] == field[i + 1][j]) {
					field[i][j] *= 2;
					field[i + 1][j] = 0;
					score += field[i][j];

				}
				bool shift = false;
				do {
					for (int i = 0; i < field_size - 1; i++) {
						shift = false;
						if (field[i][j] == 0 && field[i + 1][j] != 0) {
							field[i][j] = field[i + 1][j];
							field[i + 1][j] = 0;
							shift = true;

						}
					}
				} while (shift);
			}
		}
	case south:
		for (int j = 0; j < field_size; j++) {
			for (int i = 0; i < field_size - 1; i++) {
				if (field[i][j] == field[i - 1][j]) {
					field[i][j] *= 2;
					field[i - 1][j] = 0;
					score += field[i][j];

				}
				bool shift = false;
				do {
					for (int i = 0; i < field_size - 1; i++) {
						if (field[i][j] == 0 && field[i - 1][j] != 0) {
							field[i][j] = field[i - 1][j];
							field[i - 1][j] = 0;
							shift = true;

						}
					}
				} while (shift);
			}
		}
	case east:
		for (int j = 0; j < field_size; j++) {
			for (int i = 0; i < field_size - 1; i++) {
				if (field[j][i] == field[j + 1][i]) {
					field[j][i] *= 2;
					field[j + 1][i] = 0;
					score += field[j][i];

				}
				bool shift = false;
				do {
					for (int i = 0; i < field_size - 1; i++) {
						if (field[j][i] == 0 && field[j + 1][i] != 0) {
							field[j][i] = field[j + 1][i];
							field[j + 1][i] = 0;
							shift = true;

						}
					}
				} while (shift);
			}
		}
	case west:
		for (int j = 0; j < field_size; j++) {
			for (int i = 0; i < field_size - 1; i++) {
				if (field[j][i] == field[j - 1][j]) {
					field[j][i] *= 2;
					field[j - 1][i] = 0;
					score += field[j][i];

				}
				bool shift = false;
				do {
					for (int i = 0; i < field_size - 1; i++) {
						if (field[j][i] == 0 && field[j - 1][i] != 0) {
							field[j][i] = field[j - 1][i];
							field[j - 1][i] = 0;
							shift = true;

						}
					}
				} while (shift);
			}
		}
	}
}

void add_new_element() {
	int i, j;
	do {
		i = rand() % field_size;
		j = rand() % field_size;

	} while (field[i][j]);
	field[i][j] = (rand()%4) ? 2:4;
}
int main() {
	srand(time(0));
	init_field();
	print_field();
	while (has_free_cells()) {
		direction dir = read_direction();
		shift(dir);
		add_new_element();
		print_field();
	}
}
