********************************* AI slips *****************************************

slip2a /slip12a

import calendar

def generate_calendar(month, year):
    try:
        cal = calendar.TextCalendar()
        month_calendar = cal.formatmonth(year, month)
        print(month_calendar)
    except Exception as e:
        print(f"Error: {e}")
try:
    month = int(input("Enter the month (1-12): "))
    year = int(input("Enter the year: "))
    
    if 1 <= month <= 12:
        generate_calendar(month, year)
    else:
        print("Invalid month. Please enter a value between 1 and 12.")
except ValueError:
    print("Invalid input. Please enter numeric values for month and year.")
--------------------------------------------------------------------------------------------------------
slip2b

graph = {
    1: [2, 3, 4],
    2: [5, 6],
    3: [7],
    4: [],
    5: [],
    6: [],
    7: []
}
def depth_first_search(graph, start, goal):
    stack = [start]
    visited = set()
    path = []
    while stack:
        node = stack.pop()
        if node not in visited:
            visited.add(node)
            path.append(node)
            if node == goal:
                return path
            for neighbor in reversed(graph[node]):
                if neighbor not in visited:
                    stack.append(neighbor)
    return None
initial_node = 1
goal_node = 7
path = depth_first_search(graph, initial_node, goal_node)
if path:
    print("Path from node {} to node {}:".format(initial_node, goal_node), path)
else:
    print("No path found from node {} to node {}.".format(initial_node, goal_node))

****************************************************************************************************************************

slip3a/slip21a

import string

def remove_punctuation(input_string):
    punctuation = string.punctuation
    cleaned_string = ''.join(char for char in input_string if char not in punctuation)
    return cleaned_string

user_input = input("Enter a string: ")
result = remove_punctuation(user_input)
print("String without punctuation:", result)

-------------------------------------------------------------------------------------------------------------------
slip3b

graph = {
    1: [2, 3],
    2: [4, 5],
    3: [6, 7],
    4: [],
    5: [],
    6: [],
    7: []
}

def depth_first_search(graph, start, goal):
    stack = [start]
    visited = set()
    path = []
    while stack:
        node = stack.pop()
        if node not in visited:
            visited.add(node)
            path.append(node)
            if node == goal:
                return path
            for neighbor in reversed(graph[node]):
                if neighbor not in visited:
                    stack.append(neighbor)
    return None

initial_node = 2
goal_node = 7

path = depth_first_search(graph, initial_node, goal_node)

if path:
    print(f"Path from node {initial_node} to node {goal_node}: {path}")
else:
    print(f"No path found from node {initial_node} to node {goal_node}.")

******************************************************************************************************************************

slip7a/8b

def print_board(board):
    for row in board:
        print(" | ".join(row))
        print("-" * 5)

def check_winner(board, player):
    for row in board:
        if all(cell == player for cell in row):
            return True
    for col in range(3):
        if all(board[row][col] == player for row in range(3)):
            return True
    if all(board[i][i] == player for i in range(3)) or all(board[i][2 - i] == player for i in range(3)):
        return True
    return False

def is_full(board):
    return all(cell != ' ' for row in board for cell in row)

def play_game():
    board = [[' ' for _ in range(3)] for _ in range(3)]
    current_player = 'X'
    print("Welcome to Tic-Tac-Toe!")
    print_board(board)
    while True:
        try:
            move = input(f"Player {current_player}, enter your move (row and column: 0 1): ")
            row, col = map(int, move.split())
            if board[row][col] != ' ':
                print("This cell is already taken! Try again.")
                continue
        except (ValueError, IndexError):
            print("Invalid input! Enter row and column as two numbers separated by a space (e.g., 0 1).")
            continue
        board[row][col] = current_player
        print_board(board)
        if check_winner(board, current_player):
            print(f"Player {current_player} wins!")
            break
        if is_full(board):
            print("It's a draw!")
            break
        current_player = 'O' if current_player == 'X' else 'X'
play_game()

----------------------------------------------------------------------------------------------------------
slip7b/slip10b /slip22b

import random

responses = {
    "hello": ["Hi there!", "Hello!", "Hey! How can I help you?"],
    "how are you": ["I'm just a bot, but I'm doing great! How about you?", "I'm here to assist you. How are you?"],
    "bye": ["Goodbye!", "See you later!", "Take care!"],
    "name": ["I am ChatBot, your virtual assistant.", "You can call me ChatBot!"],
    "help": ["Sure, I'm here to help. What do you need assistance with?", "How can I assist you today?"],
    "default": ["I'm not sure I understand. Can you rephrase?", "Sorry, I didn't get that.", "Could you please elaborate?"]
}

def get_response(user_input):
    user_input = user_input.lower()
    for key in responses:
        if key in user_input:
            return random.choice(responses[key])
    return random.choice(responses["default"])

def chatbot():
    print("ChatBot: Hello! I'm ChatBot. Type 'bye' to exit.")
    while True:
        user_input = input("You: ")
        if user_input.lower() == "bye":
            print("ChatBot: Goodbye! Have a great day!")
            break
        response = get_response(user_input)
        print(f"ChatBot: {response}")

chatbot()

****************************************************************************************************************************
slip8a

def count_case_letters(input_string):
    upper_case_count = 0
    lower_case_count = 0
    for char in input_string:
        if char.isupper():
            upper_case_count += 1
        elif char.islower():
            lower_case_count += 1
    return upper_case_count, lower_case_count

user_input = input("Enter a string: ")

upper_count, lower_count = count_case_letters(user_input)

print(f"Number of uppercase alphabets: {upper_count}")
print(f"Number of lowercase alphabets: {lower_count}")

----------------------------------------------------------------------------------------------------------------------------------
slip8b =>>> refer 7a

*********************************************************************************************************************************
slip 10a

from itertools import permutations

def solve_cryptarithmetic():
    digits = range(10)
    letters = 'TWOFRU'
    for perm in permutations(digits, len(letters)):
        mapping = dict(zip(letters, perm))
        T, W, O, F, R, U = mapping['T'], mapping['W'], mapping['O'], mapping['F'], mapping['R'], mapping['U']
        if T == 0 or F == 0:
            continue
        TWO = 100 * T + 10 * W + O
        FOUR = 1000 * F + 100 * O + 10 * U + R
        if TWO + TWO == FOUR:
            print(f"SOLUTION FOUND: TWO = {TWO}, FOUR = {FOUR}")
            print(f"Mapping: {mapping}")
            return
    print("No solution found.")

solve_cryptarithmetic()

--------------------------------------------------------------------------------------------------------------------------------
slip10b  ==> refer slip7b

********************************************************************************************************************************
slip12a ==> refer slip2a

-------------------------------------------------------------------------------------------------------------------------------
slip12b /slip13b

N = 4
def print_solution(board):
    for row in board:
        print(" ".join("Q" if x == 1 else "." for x in row))
    print("\n")

def is_safe(board, row, col):
    for i in range(row):
        if board[i][col] == 1:
            return False
    for i, j in zip(range(row, -1, -1), range(col, -1, -1)):
        if board[i][j] == 1:
            return False
    for i, j in zip(range(row, -1, -1), range(col, N)):
        if board[i][j] == 1:
            return False
    return True
    
def solve_n_queens(board, row):
    if row >= N:
        print_solution(board)
        return True
    found_solution = False
    for col in range(N):
        if is_safe(board, row, col):
            board[row][col] = 1
            if solve_n_queens(board, row + 1):
                found_solution = True
            board[row][col] = 0
    return found_solution
    
def n_queens():
    board = [[0] * N for _ in range(N)]
    if not solve_n_queens(board, 0):
        print("No solution exists.")
        
n_queens()

**************************************************************************************************************************
slip13b ===> slip12b

*************************************************************************************************************************
slip14a

def sort_sentence(sentence):
    words = sentence.split()
    sorted_words = sorted(words, key=lambda x: x.lower())  
    sorted_sentence = " ".join(sorted_words)
    return sorted_sentence
sentence = input("Enter a sentence: ")

sorted_sentence = sort_sentence(sentence)
print("Sorted sentence:", sorted_sentence)

--------------------------------------------------------------------------
slip14b

def print_board(board):
    for row in board:
        print(" ".join(["Q" if col == 1 else "." for col in row]))
    print("\n")
    
def is_safe(board, row, col, N):
    for i in range(row):
        if board[i][col] == 1:
            return False
    for i, j in zip(range(row, -1, -1), range(col, -1, -1)):
        if board[i][j] == 1:
            return False
    for i, j in zip(range(row, -1, -1), range(col, N)):
        if board[i][j] == 1:
            return False
    return True
    
def solve_n_queens(board, row, N):
    if row == N:
        print_board(board)
        return True
    res = False
    for col in range(N):
        if is_safe(board, row, col, N):
            board[row][col] = 1
            res = solve_n_queens(board, row + 1, N) or res
            board[row][col] = 0
    return res
    
def n_queens(N):
    board = [[0 for _ in range(N)] for _ in range(N)]
    if not solve_n_queens(board, 0, N):
        print("Solution does not exist.")
if __name__ == "__main__":
    N = int(input("Enter the value of N (size of the chessboard): ")) 
    n_queens(N)

*******************************************************************************************************************************
slip15a

class MonkeyBananaProblem:
    def __init__(self):
        self.monkey_position = "ground"
        self.box_position = "ground"
        self.banana_position = "ceiling"
        self.box_under_banana = False
        self.monkey_at_box = False
        self.banana_reached = False
        
    def move_monkey_to_box(self):
        if self.monkey_position == "ground" and self.box_position == "ground":
            self.monkey_position = "box"
            self.monkey_at_box = True
            print("Monkey moved to the box.")
        else:
            print("Monkey is already on the box or box is not on the ground.")

    def move_box_under_banana(self):
        if self.monkey_position == "box":
            self.box_position = "under_banana"
            self.box_under_banana = True
            print("Box moved under the bananas.")
        else:
            print("Monkey is not on the box, can't move the box.")
            
    def monkey_climb(self):
        if self.monkey_at_box and self.box_under_banana:
            self.monkey_position = "ceiling"
            self.banana_reached = True
            print("Monkey climbed and reached the bananas.")
        else:
            print("Monkey can't climb, either the box is not under the bananas or monkey is not on the box.")

    def check_if_banana_reached(self):
        if self.banana_reached:
            print("The monkey has successfully reached the bananas!")
        else:
            print("The monkey has not reached the bananas yet.")
            
    def solve(self):
        print("Starting the Monkey and Banana problem...\n")
        self.move_monkey_to_box()  
        self.move_box_under_banana()  
        self.monkey_climb() 
        self.check_if_banana_reached()

if __name__ == "__main__":
    problem = MonkeyBananaProblem()
    problem.solve()

--------------------------------------------------------------------------------------------------------------------------------
slip15b

class IterativeDeepeningDFS:
    def __init__(self, graph, start_node, goal_node):
        self.graph = graph  # Graph as an adjacency list
        self.start_node = start_node
        self.goal_node = goal_node
    
    def dfs(self, node, goal, depth, visited):
        # If the goal is found or we reach the depth limit
        if node == goal:
            return [node]
        if depth == 0:
            return None
        
        visited.add(node)
        for neighbor in self.graph.get(node, []):
            if neighbor not in visited:
                path = self.dfs(neighbor, goal, depth - 1, visited)
                if path:
                    return [node] + path
        visited.remove(node)
        return None

    def iddfs(self):
        depth = 0
        while True:
            visited = set()
            print(f"Searching at depth {depth}")
            path = self.dfs(self.start_node, self.goal_node, depth, visited)
            if path:
                return path  # Found the goal
            depth += 1

graph = {
    'A': ['B', 'C'],
    'B': ['A', 'D', 'E'],
    'C': ['A', 'F'],
    'D': ['B'],
    'E': ['B', 'G'],
    'F': ['C'],
    'G': ['E']
}

start_node = 'A'
goal_node = 'G'

iddfs_solver = IterativeDeepeningDFS(graph, start_node, goal_node)

result = iddfs_solver.iddfs()

if result:
    print(f"Path to goal {goal_node}: {' -> '.join(result)}")
else:
    print(f"Goal node {goal_node} not found.")

*******************************************************************************************************************************
slip16a/slip23a

def tower_of_hanoi(n, source, target, auxiliary):
    if n == 1:
        print(f"Move disk 1 from {source} to {target}")
        return

    tower_of_hanoi(n-1, source, auxiliary, target)

    print(f"Move disk {n} from {source} to {target}")

    tower_of_hanoi(n-1, auxiliary, target, source)

if __name__ == "__main__":
    n = int(input("Enter the number of disks: "))  # Number of disks
    tower_of_hanoi(n, 'A', 'C', 'B') 

------------------------------------------------------------------------------------------------------------------------------
slip16b ==> refer slip8b

*****************************************************************************************************************************

slip21a/slip3a
-----------------------------------------------------------------------------------------------------------------------------
slip21b

from itertools import permutations

def solve_cryptarithmetic():

    letters = ['G', 'O', 'T', 'U']
    
    for perm in permutations(range(10), len(letters)):
        letter_to_digit = dict(zip(letters, perm))
        
        G = letter_to_digit['G']
        O = letter_to_digit['O']
        T = letter_to_digit['T']
        U = letter_to_digit['U']
        
        GO = G * 10 + O
        TO = T * 10 + O
        OUT = U * 100 + O * 10 + T
        
        if GO + TO == OUT:
            print(f"Solution found: G = {G}, O = {O}, T = {T}, U = {U}")
            print(f"{GO} + {TO} = {OUT}")
            return  
    
    print("No solution found.")

solve_cryptarithmetic()


*********************************************************************************************************************************

Slip22b ==> refer 7b

********************************************************************************************************************************

slip23a ==> refer slip16a

--------------------------------------------------------------------------------------------------------------------------------
slip23b 

import itertools

def is_valid_solution(send, more, money):
    return send + more == money

def solve_cryptarithmetic():
    letters = 'SENDMOREMONEY'

    unique_letters = set(letters)

    if len(unique_letters) != 8:
        print("Invalid number of unique letters.")
        return

    for perm in itertools.permutations(range(10), len(unique_letters)):

        letter_to_digit = dict(zip(unique_letters, perm))

        send = int("".join(str(letter_to_digit[char]) for char in 'SEND'))
        more = int("".join(str(letter_to_digit[char]) for char in 'MORE'))
        money = int("".join(str(letter_to_digit[char]) for char in 'MONEY'))

        if is_valid_solution(send, more, money):
            print(f"SEND = {send}")
            print(f"MORE = {more}")
            print(f"MONEY = {money}")
            print(f"Solution: {letter_to_digit}")
            return

    print("No solution found.")

solve_cryptarithmetic()

*******************************************************************************************************************************
slip24a ==> refer slip14a

-------------------------------------------------------------------------------------------------------------------------------
slip 24b 

import itertools

def is_valid_solution(cross, roads, danger):
    return cross + roads == danger

def solve_cryptarithmetic():
    unique_letters = ['C', 'R', 'O', 'S', 'A', 'D', 'N', 'G', 'E']


    for perm in itertools.permutations(range(10), len(unique_letters)):
        
        if perm[0] == 0 or perm[1] == 0 or perm[2] == 0:
            continue  

        letter_to_digit = dict(zip(unique_letters, perm))

        cross = int("".join(str(letter_to_digit[char]) for char in 'CROSS'))
        roads = int("".join(str(letter_to_digit[char]) for char in 'ROADS'))
        danger = int("".join(str(letter_to_digit[char]) for char in 'DANGER'))

        if is_valid_solution(cross, roads, danger):
            print(f"CROSS = {cross}")
            print(f"ROADS = {roads}")
            print(f"DANGER = {danger}")
            print(f"Solution: {letter_to_digit}")
            return

    print("No solution found.")

solve_cryptarithmetic()

****************************************************** END ********************************************************


