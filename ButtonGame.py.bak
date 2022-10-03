from tkinter import Tk, Button, Menu, filedialog
import random
import json


class Game(Tk):
    def __init__(self):
        super().__init__()

        # grid size and 'shadow' color definitions
        self.gridWidth = 15
        self.gridHeight = 15
        self.whiteABG = '#CCCCCC'
        self.blackABG = '#3D3D3D'
        # List for storing the button names and other variables defined
        self.button = []
        self.shapeFlag = 'plusSign'
        self.filename = "ButtonGame"

        # Creating the buttons and storing them in a list of lists
        for height in range(self.gridHeight):
            row = []
            for width in range(self.gridWidth):
                temp_button = Button(self, text='    ', command=lambda y=height, x=width: self.press(y, x), bg='white',
                                     activebackground=self.whiteABG)
                row.append(temp_button)
            self.button.append(row)

        # Create Menu bar
        self.option_add('*tearOff', False)
        self.menu_bar = Menu(self)
        self.config(menu=self.menu_bar)
        # Create Menu tabs
        self.file = Menu(self.menu_bar)
        self.shape = Menu(self.menu_bar)
        self.menu_bar.add_cascade(menu=self.file, label='File')
        self.menu_bar.add_cascade(menu=self.shape, label='Shape')
        # Create Dropdown Options
        # File Options
        self.file.add_command(label='New', command=self.new_game)
        self.file.add_command(label='Save', command=self.save_game)
        self.file.add_command(label='Open', command=self.open_game)
        # Shape options
        self.shape.add_command(label='3row', command=lambda: self.shape_switch('3row'))
        self.shape.add_command(label='plusSign', command=lambda: self.shape_switch('plusSign'))
        self.shape.add_command(label='tower', command=lambda: self.shape_switch('tower'))

    def new_game(self):
        # Resets the colors and shapeFlag for a new game
        self.randColor()
        self.shapeFlag = 'plusSign'

    def save_game(self):
        # Allows user to save the current color distribution to a file that can be opened
        save_list = []
        for y in range(self.gridHeight):
            for x in range(self.gridWidth):
                if self.button[y][x]['bg'] == 'black':
                    save_list.append([y, x])

        save_dictionary = {
            "blackList": save_list
        }

        file = filedialog.asksaveasfile()
        with open(file.name, 'w') as outfile:
            json.dump(save_dictionary, outfile)

    def open_game(self):
        # Allows the user to open a .json and sets the colors to the given distribution
        # **Needs to check for game size, filetype, etc**
        file = filedialog.askopenfile()
        with open(file.name) as infile:
            open_dictionary = json.load(infile)

        for y in range(self.gridHeight):
            for x in range(self.gridWidth):
                self.button[y][x]['bg'] = 'white'

        for i in open_dictionary["blackList"]:
            self.button[i[0]][i[1]]['bg'] = 'black'

    def shadow(self, y, x):
        # Creates a 'shadow' over the cells corresponding to the current shape where the cursor is located
        if self.shapeFlag == 'plusSign':
            for i in self.plusSign(y, x):
                self.button[i[0]][i[1]]['state'] = 'active'
        elif self.shapeFlag == 'tower':
            for i in self.tower(y, x):
                self.button[i[0]][i[1]]['state'] = 'active'
        elif self.shapeFlag == '3row':
            for i in self.threeRow(y, x):
                self.button[i[0]][i[1]]['state'] = 'active'

    def unshadow(self, y, x):
        # Removes the shadow effect once the cursor is moved from the cell
        if self.shapeFlag == 'plusSign':
            for i in self.plusSign(y, x):
                self.button[i[0]][i[1]]['state'] = 'normal'
        elif self.shapeFlag == 'tower':
            for i in self.tower(y, x):
                self.button[i[0]][i[1]]['state'] = 'normal'
        elif self.shapeFlag == '3row':
            for i in self.threeRow(y, x):
                self.button[i[0]][i[1]]['state'] = 'normal'

    def shape_switch(self, shape):
        # Changes the shapeFlag
        self.shapeFlag = shape

    def reverse_color(self, cell):
        # Given a list of cells, this turns black cells white and white cells black
        for i in cell:

            if self.button[i[0]][i[1]]['bg'] == 'white':
                self.button[i[0]][i[1]].configure(bg='black')
                self.button[i[0]][i[1]].configure(activebackground=self.blackABG)
            else:
                self.button[i[0]][i[1]].configure(bg='white')
                self.button[i[0]][i[1]].configure(activebackground=self.whiteABG)

            self.button[i[0]][i[1]]['state'] = 'normal'

    def randColor(self):
        # Changes all cell to black or white randomly
        for y in self.button:
            for x in y:
                if random.randint(0, 1) == 1:
                    x.configure(bg='black')
                    x.configure(activebackground=self.blackABG)
                else:
                    x.configure(bg='white')
                    x.configure(activebackground=self.whiteABG)

    def plusSign(self, y, x):
        # Returns a list of cells based on the given cell in a plus sign pattern
        cell_list = [[y, x]]

        # Button to the left
        if x != 0:
            cell_list.append([y, x - 1])
        # Button to the right
        if x + 1 != self.gridWidth:
            cell_list.append([y, x + 1])
        # Button above
        if y != 0:
            cell_list.append([y - 1, x])
        # Button below
        if y + 1 != self.gridHeight:
            cell_list.append([y + 1, x])

        return cell_list

    def tower(self, y, x):
        # Returns a list of cells based on the given cell in a vertical 5 cell pattern
        i = 0
        j = 5
        if y == 0:
            j = 3
        elif y == 1:
            j = 4
        elif y == self.gridHeight - 2:
            i = 1
        elif y == self.gridHeight - 1:
            i = 2

        cell_list = []
        # Flip 5 buttons in a vertical row
        for k in range(i, j):
            cell_list.append([y + 2 - k, x])

        return cell_list

    def threeRow(self, y, x):
        # Returns a list of cells based on the given cell in a row of 3 cells
        cell_list = [[y, x]]

        if x != 0:
            cell_list.append([y, x - 1])

        if x + 1 != self.gridWidth:
            cell_list.append([y, x + 1])

        return cell_list

    def press(self, y, x):
        # Given a cell it reverses the color with the appropriate pattern and changes the shapeFlag
        if self.shapeFlag == '3row':
            self.reverse_color(self.threeRow(y, x))
            self.shapeFlag = 'plusSign'
        elif self.shapeFlag == 'plusSign':
            self.reverse_color(self.plusSign(y, x))
            self.shapeFlag = 'tower'
        elif self.shapeFlag == 'tower':
            self.reverse_color(self.tower(y, x))
            self.shapeFlag = '3row'

    def build(self):
        # Adding the buttons to the window grid
        for height in range(self.gridHeight):
            for width in range(self.gridWidth):
                self.button[height][width].grid(column=width, row=height, stick='nsew')
                self.button[height][width].bind('<Enter>', lambda event, y=height, x=width: self.shadow(y, x))
                self.button[height][width].bind('<Leave>', lambda event, y=height, x=width: self.unshadow(y, x))
                self.columnconfigure(width, weight=1)
            self.rowconfigure(height, weight=1)

        self.randColor()


if __name__ == "__main__":
    game = Game()
    game.build()

    game.mainloop()
