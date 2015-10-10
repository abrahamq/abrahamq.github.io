# Conway's game of life in PyGame

So after I made Conway's game of life in python, I decided that a graphical
representation would be really nice. I wrote a pretty quick and dirty script in
pygame to do this. You can click on dead cells to make them come alive and
click on live ones to kill them. You can also pause the game by hitting any key
during play. This thing is very much still under development. One known bug is
that when you first pause and click on the board, the game will move forward
one iteration. It will not move again until unpaused. I'm sure there's some
simple flow control issue I'm not seeing (post in the comments if you have any
suggestions). This uses my previous gameoflifeclasses script from the previous
post. You should save that script as a .py file, then save this script as a .py
file in the same directory.

![initial][initial] This is what you'll likely see when you start up the script:
The default starting configuration is a glider. You can change this by changing
lines under the #hardcoded comment in the __init__ constructor of the GOL
class.

Please do whatever you want with the code, just give me credit when you do.
Also please point out bugs.


Possible new features: 
* A memory system so that you can scroll through previous iterations of the game board
* A slider control for how fast the game updates
* A (semi) infinite board where you can zoom into or out of different regions (and that gets smaller as more of the board is explored by cells.

    
# Here is the code:

```python
 import pygame, sys   
 from pygame.locals import * 
 import Gameoflifeclasses  
    
 white = (255, 255,255)  
 black = (0, 0, 0)  
 height = 100 #number of pixels across  
 width = 100 
 widthBlock = 10 #number of pixels of width per block  
 heightBlock = 10 
 yBoxes = height/heightBlock #number of blocks down   
 xBoxes = width/widthBlock #number of blocks across  
    
 def posOfBoxes(width, widthBlock,height, heightBlock): #creates an array of tuples (x,y) that have the pixel wise positions of the boxes  
   corners = []   
   print(xBoxes)  
   for i in xrange(0, (yBoxes)*(xBoxes)):  
     corners.append(((widthBlock*(i%xBoxes)), (widthBlock*(i/yBoxes))))  
   return corners  
    
 def indexOfBoxes(xPixel,yPixel): #Returns the x and y position of a box (as a tuple) when given pixel coordinates   
   x = xPixel/10 
   y = yPixel/10 
   return (x,y)  
   pass 
     
 def main():  
   pygame.init()  
   maindisplay = pygame.display.set_mode((width,height))  
   maindisplay.fill(white)  
   linesurf = pygame.Surface((width, height))  
   i = 0 
   for i in xrange(0, width/widthBlock): #these create horizontal and vertical lines  
     linePos = widthBlock+widthBlock*i   
     pygame.draw.line(linesurf, black, (linePos,0), (linePos,height), 1)  
   for x in xrange(0,height/heightBlock):  
     linePos = heightBlock+heightBlock*x  
     pygame.draw.line(linesurf, black, (0,linePos), (height,linePos), 1)  
   corners = posOfBoxes(width, widthBlock,height, heightBlock)  
   print(corners)  
   game = Gameoflifeclasses.GOL(xBoxes) #This initiates the gaem of life classes   
   paused = False 
      
   while True:  
     ev = pygame.event.get()  
     for event in ev:   
       if event.type == pygame.MOUSEBUTTONUP and paused == True:  
         pos = pygame.mouse.get_pos()  
         xPixel, yPixel = pos #unpacking the mouse position  
         xIndex, yIndex = indexOfBoxes(xPixel,yPixel) #unpacking the tuple returned by indexOfBoxes  
         game.changePoint(xIndex,yIndex)  
         for point in xrange(0,len(corners)):  
           boxxy = pygame.Rect(corners[point], (widthBlock-1, heightBlock-1))  
           if game.array[point] == 1:   
             pygame.draw.rect(maindisplay, black, boxxy, 0)  
           else:   
             pygame.draw.rect(maindisplay, white, boxxy, 0)  
         pygame.display.flip()  
       if event.type == KEYDOWN:  
         if paused:   
           paused = False 
         else:   
           paused = True 
       if event.type == QUIT:  
         pygame.quit()  
         sys.exit()  
            
     if not paused:   
       for point in xrange(0,len(corners)):  
         boxxy = pygame.Rect(corners[point], (widthBlock-1, heightBlock-1))  
         if game.array[point] == 1:   
           pygame.draw.rect(maindisplay, black, boxxy, 0)  
         else:   
           pygame.draw.rect(maindisplay, white, boxxy, 0)  
       pygame.time.delay(100)  
       game.update()  
       pygame.display.flip()  
        
    
    
        
 if __name__ == '__main__':  
   main()  
   pass
```



[initial]: ../images/conwayPyGame/initial.jpg "Initial image"
