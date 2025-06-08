# TJ CTF

June 6th - June 8th.

Hosted by the [Thomas Jefferson High School for Science and Technology](https://ctf.tjctf.org/), I originally was not going to compete in this CTF because I was camping without reception for most of it, but thought this was going to be a nice easy CTF and wanted to check it out. Those highschoolers did a really good job creating interesting problems. So I wish I had of been able to participate the entirety of it.

I competed alone and got 461st of 723 teams who scored at least a point.
![CTF_placement](https://github.com/user-attachments/assets/8e54a683-ce4a-4809-a9d1-38a41b185248)

- Binary search: [Guess my Number](#Guess-My-Number)
- Plotting points: [Mouse Trails](#Mouse-Trails)
- AST_Dump: [Serpent](#Serpent)
- WASM RE: [Garfield Monday Lasagna](#Garfield-Monday-Lasagna)

## Guess My Number
![GuessMyNumber](https://github.com/user-attachments/assets/7a18eade-720e-4699-9e8c-b8b771dbeba7)

As soon as I saw this I knew we could use a binary search to guess the correct number. The basic idea is that in a sorted list you can quickly (O(log(N)) find an element, by halving the search range each time. 

Given the site will tell you if your guess is too high or too low, we know that we can halve the number of possibilities by guessing the mid point. For example, the range is 1-1,000, our first guess is 500. Being told that this was too low, we know the range is between 500-1,000. By recrusively continuing this pattern (guessing the midpoint of the range), we eventually guess the correct number. 

![exampleBinarySearch](https://github.com/user-attachments/assets/bd4c00bc-c409-4c4d-bac2-db260a626012)

**tjctf{g0od_j0b_u_gu33sed_correct_998}**


## Mouse Trails
![MouseTrails](https://github.com/user-attachments/assets/92e557d7-efde-44c3-96ca-88a1ac96087d)

For this problem, we are given a text file with thousands of coordinate pairs (ie. 557,709) each on a line of their own. Since these pairs relate the where the mouse was, we know by plotting the points we can get a picture of what was on screen. To do this, I used python and `matplotlib.pyplot`. For ease of the writeup I cleaned up and commented the code below. 

First, we need to read the coordinate pairs from the file into our program's memory. 
```python
def read_coordinates(filename): # takes in filename
    x_coords = [] # array of x coordinates
    y_coords = [] # array of y coordinates
    with open(filename, 'r') as file: # open file
        for line in file: # loop through each line
            try:
                x, y = map(float, line.strip().split(',')) # split the x and y by a comma and strip any spaces
                x_coords.append(x) # append x to the x array
                y_coords.append(y) # append y to the y array
            except ValueError:
                print(f"Skipping invalid line: {line.strip()}") # catch any lines that do not have a coordinate pair
    return x_coords, y_coords # return resulting arrays
```
Next we need to plot each coordinate pair. Because I was unsure if the points needed to be connected, I programmed it to be able to print both a scatter plot and a line graph.
```python
import matplotlib.pyplot as plt # import plot 
def plot_coordinates(x, y, plot_type='scatter'): # takes in x and y arrays and optionally the plot type
    plt.figure(figsize=(8, 6))  # Arbitrary plot size
    if plot_type == 'scatter': 
        plt.scatter(x, y, marker='o', label='Points') # creates a scatter plot
    elif plot_type == 'line':
        plt.plot(x, y, marker='o', linestyle='-', label='Line') # creates a line plot
    else:
        raise ValueError("Invalid plot_type. Choose 'scatter' or 'line'.") # error catching
    plt.show() # display the plot 
```

Finally, the main function called each of these functions and displayed the plots.
```python
if __name__ == "__main__":
    x_coords, y_coords = read_coordinates('mouse_movements.txt') # read in coordinates from txt file
    
    if x_coords and y_coords:
        plot_coordinates(x_coords, y_coords, plot_type='scatter') # create and show scatter plot
        plot_coordinates(x_coords, y_coords, plot_type='line') # create and show line plot
    else:
        print("No valid coordinates found to plot.") # error catching

```

The line plot was garbage, but the scatter plot clearly showed writing. 
![ScatterPlot](https://github.com/user-attachments/assets/eba422c6-b256-4ed2-b3a6-b539ddd95d28)

After saving it to a file and opening it with ImageMagick (since I'm on Kali Linux), I flipped the image and found the flag.
![ScatterPlotFlipped](https://github.com/user-attachments/assets/6b3611ba-9e94-487c-b804-c0ffd6c2bd25)

**tjctf{we_love_cartesian_plane}**

## Serpent
![Serpent](https://github.com/user-attachments/assets/7dda53bc-b300-4e3d-8221-d1ad0a41a8ac)

We are given a `.pickle` file for this challenge. Before even looking up what a pickle file was, it ran `file` on the file to see that it was just `data`. 
![FileCMDResults](https://github.com/user-attachments/assets/591e70a0-de1f-45d8-bfb6-2c84e1a764be)

I decided to open it in vim just to see if it was readable, and several flags immediately jumped out to me, so I ran `strings` and foun a plethora of possible flags as well as several words like "distractions" or "fake flags." 
![stringsCMDResults](https://github.com/user-attachments/assets/0d00d883-35b1-4284-b9fb-ebfdbdf41444)

At this point I decided to look up `.pickle` file to find that it is a Python-specific file format for serialization. I figured `ast_dump` could have meaning as well, so I found that it stood for abstract syntax tree, and `ast` was a python library for working with it. 

An abstract syntax tree (AST) is a data structure that represents the structure of code. These are widely used in compilers or program analysis. By the hint to go to the deepest level, I knew we wanted to see an outupt in its tree format and find the deepest leaf node. Instead I found a way to print the data with indents and followed that as deep as I could. 

```python
import pickle
import ast

with open("ast_dump.pickle", "rb") as f: # open the file
    loaded_data = pickle.load(f) # deserial the data

print(ast.dump(ast.parse(loaded_data), indent=4)) # print the data as an ast_dump parsed to nicely have indents
```

I found a Function called "deepest layer" and it only had one associated flag. Trying this flag solved the challenge. 
![AstDump](https://github.com/user-attachments/assets/d8962caf-ff67-4c11-a8e5-86b61d749d30)

**tjctf{f0ggy_d4ys}**

## Garfield Monday Lasagna
![GarfieldMondayLasagna](https://github.com/user-attachments/assets/a5f2cb76-9285-40a9-9b3b-defa1441588d)
