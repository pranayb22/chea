*********************************AI slips*****************************************

slip2a

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

==========================================================================================================================
slip3a

import string

def remove_punctuation(input_string):
    # Define the punctuation characters
    punctuation = string.punctuation
    # Remove punctuation from the string
    cleaned_string = ''.join(char for char in input_string if char not in punctuation)
    return cleaned_string

user_input = input("Enter a string: ")
result = remove_punctuation(user_input)
print("String without punctuation:", result)

-------------------------------------------------------------------------------------------------------------------
slip3b




