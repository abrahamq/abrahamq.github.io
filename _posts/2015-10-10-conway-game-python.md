---
layout: single
title: "Conway's Game of Life"
date: 2015-10-10
classes: wide
---

My Freshman year I started writing a python script to play Conway's game of life.
The game was created by British mathematician John Conway as a simple way to
simulate life. The playing field is a grid of squares, where each square either
holds a live cell or a dead one. The basic rules (taken from the wiki article)
are as follows:

- Any live cell with fewer than two live neighbours dies, as if caused by under-population.
- Any live cell with two or three live neighbours lives on to the next generation.
- Any live cell with more than three live neighbours dies, as if by overcrowding.
- Any dead cell with exactly three live neighbours becomes a live cell, as if by reproduction.

If you want more information about the game itself and its significance, I'll be
posting another blog on the topic soon. I started this project by writing a
quick python script that could output the game of life board as a nicely
formatted array of 1s (living cells) and 0s (dead cells). After that, I turned
the script into a class and made it possible to create Game of Life objects and
to do lots more stuff with it. Here's the current version of the script: The
main function that is included will create a GOL object that is 10 cells by 10
cells. It will then add points at (0,0) and (1,1). Note: the grid works like
pixels on the screen, it starts at (0,0) at the top left of the board. The
script will output a 10 by 10 grid to the console. You are welcome to make any
modifications and share the code as you wish, just give me credit.

```python
def main():
    game = GOL(10)
    game.addPoint(0,0)
    game.addPoint(1,1)
    game.run(1)
#   while True: #does the same as run
#     game.update()
#     game.output(game.array)
#     game.time.sleep(1)

##    game.run()
    pass

class GOL:
    import time
    import copy


    def __init__(self, width): #The constructor for a GOL object. You only have to pass it width of the field
        self.width = width
        self.height = width
        self.arraylength = width*width #The game of life field is all represented as one array, that gets truncated into the blocks you see when you print
        self.array = [0]*self.arraylength #creates an array full of zeros that is the same length as arraylength
        self.arrayNew = self.copy.copy(self.array) #uses the copy method of the copy class to copy the array into arrayNew
        self.delay = 1 # This is how much time passes between
        #hardcoded for now, creates a glider
        self.array[self.index(3,3)]=1
        self.array[self.index(4,3)]=1
        self.array[self.index(5,3)]=1
        self.array[self.index(5,2)]=1
        self.array[self.index(4,1)]=1

    def index(self, x,y):
        fx = x%self.width
        fy = y%self.height
        return fy*self.width +fx

    def toX(self,index):
        return index%self.width

    def toY(self,index):
        return (index - self.toX(index))/self.width

    def toXY(self, index): #returns x, y coordinate as a tuple (x,y)
        return (self.toX(index), self.toY(index))

    def addPoint(self, x,y): #adds a cell at the specified (x,y) location
        self.array[self.index(x,y)] = 1

    def changePoint(self, x, y): #changes the state of a cell at a specified (x,y) point
        if self.array[self.index(x,y)] == 1:
            self.array[self.index(x,y)] = 0
        else:
            self.array[self.index(x,y)] = 1


    def update(self):
        for i in xrange(self.arraylength):
            fx = self.toX(i)
            fy = self.toY(i)
            if(self.numNeighbors(fx,fy)<2):
                self.arrayNew[i] = 0
            if(self.numNeighbors(fx,fy)>3):
                self.arrayNew[i] = 0
            if(self.numNeighbors(fx,fy)==3 ):
                self.arrayNew[i] = 1
            if(self.numNeighbors(fx,fy) == 2 and self.live(fx,fy)):
                self.arrayNew[i] = 1
        self.array = self.copy.copy(self.arrayNew)

    def live(self, x, y): #returns true if a cell is alive
        if (self.array[self.index(x,y)] == 1):
            return True
        else:
            return False
        pass

    def numNeighbors(self, x, y): #Goes through the 8 possible neighbor positions and finds out how many of them are alive
        res = 0
        if (self.live(x-1,y)):
            res+= 1
        if (self.live(x+1,y)):
            res+=1
        if (self.live(x, y+1)):
            res+=1
        if (self.live(x,y-1)):
            res+=1
        if (self.live (x+1,y+1)):
            res+=1
        if (self.live (x-1,y-1)):
            res+=1
        if (self.live(x+1, y-1)):
            res+=1
        if (self.live(x-1, y+1)):
            res+=1
        return res

    def output(self,foo):
        for i in xrange(self.width):
            print(foo[self.width*i:(i+1)*self.width-1]) #Prints out the array in rows of length width
        print("________________")

    def run(self, waitTime): #call this thing to have the game run. Pass a waitTime, which is the number of seconds between printouts
        self.output(self.array)
        self.update()
        self.time.sleep(waitTime)
        self.run(waitTime)

    def runOnce(self, waitTime): #just like run, but only does it once
        self.output(self.array)
        self.update()
        self.time.sleep(waitTime)

if __name__ == '__main__':
    main()
```
