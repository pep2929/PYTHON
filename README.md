# PYTHON


from random import seed, random
import sys

dim = 10

def display(grid):
    print('  ', '-' * (2 * dim + 3))
    for row in grid:
        print('   |', *row, '|')
    print('  ', '-' * (2 * dim + 3))

def longest_path(i, j, grid):
    # Adjust indices to 0-based indexing
    i -= 1
    j -= 1

    # Check if starting cell contains '*'
    if grid[i][j] != '*':
        return 0, None  # No path if starting cell is not '*'

    path_length = 0
    path_in_grid = [[' ' for _ in range(dim)] for _ in range(dim)]
    direction = 'NE'  # Start moving NE

    while True:
        # Mark current position if not already marked
        if path_in_grid[i][j] == ' ':
            path_in_grid[i][j] = '↗' if direction == 'NE' else '↙'
            path_length += 1

        moved = False
        # Move in the current direction as far as possible
        while True:
            if direction == 'NE':
                next_i, next_j = i - 1, j + 1
            else:  # direction == 'SW'
                next_i, next_j = i + 1, j - 1

            if 0 <= next_i < dim and 0 <= next_j < dim and grid[next_i][next_j] == '*':
                i, j = next_i, next_j
                if path_in_grid[i][j] == ' ':
                    path_in_grid[i][j] = '↗' if direction == 'NE' else '↙'
                    path_length += 1
                    moved = True
                else:
                    break  # Already visited
            else:
                break  # Can't move further in this direction

        if not moved:
            # Attempt to move SE once to change direction
            next_i, next_j = i + 1, j + 1  # Move SE
            if 0 <= next_i < dim and 0 <= next_j < dim and grid[next_i][next_j] == '*':
                i, j = next_i, next_j
                # Mark the SE move
                if path_in_grid[i][j] == ' ':
                    path_in_grid[i][j] = '↘'
                    path_length += 1
                # Switch direction
                direction = 'SW' if direction == 'NE' else 'NE'
            else:
                break  # Can't move SE or cell is empty; path ends here

    return path_length, path_in_grid

# Input and grid generation
try:
    for_seed, i, j, density = input('Input an integer, two integers between '
                                    f'1 and {dim},\n      '
                                    'and a number between 0 and 1: '
                                    ).split()
    for_seed, i, j, density = int(for_seed), int(i), int(j), float(density)
    if not (1 <= i <= dim and 1 <= j <= dim and 0 <= density <= 1):
        raise ValueError
except ValueError:
    print('Incorrect input, giving up.')
    sys.exit()

seed(for_seed)
grid = [['*' if random() < density else ' ' for _ in range(dim)]
        for _ in range(dim)
       ]

print('Here is the grid that has been generated:')
display(grid)

path_length, path_in_grid = longest_path(i, j, grid)
if path_length == 0:
    print(f'There is no special path starting from ({i}, {j}) in the grid!')
else:
    print(f'The longest special path starting from ({i}, {j}) has a length of {path_length}.')
    print('Here it is:')
    display(path_in_grid)
