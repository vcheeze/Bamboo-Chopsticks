# Bamboo-Chopsticks
from random import choice
add_library('minim')
minim=Minim(this)

class Hand:
    def __init__(self,numFin=1,player='A'):
        self.numFin=numFin
        self.value=False
        self.img1=loadImage('1'+player+'.png')
        self.img2=loadImage('2'+player+'.png')
        self.img3=loadImage('3'+player+'.png')
        self.img4=loadImage('4'+player+'.png')


    def display(self,x,y,s):
        self.x=x
        self.y=y
        self.s=s        
        if self.numFin==1:
            image(self.img1,self.x,self.y,self.s,self.s)
        elif self.numFin==2:
            image(self.img2,self.x,self.y,self.s,self.s)
        elif self.numFin==3:
            image(self.img3,self.x,self.y,self.s,self.s)
        elif self.numFin==4:
            image(self.img4,self.x,self.y,self.s,self.s)



class Player:
    def __init__(self,player):
        self.hand1=Hand(player=player)
        self.hand2=Hand(player=player)
        self.hands=[self.hand1,self.hand2]

    def displayBig(self):
        self.hand1.display(100,400,200)
        self.hand2.display(500,400,200)
        
    def displaySmall(self):
        self.hand1.display(300,50,75)
        self.hand2.display(425,50,75)


class Game:
    def __init__(self):
        self.Player1=Player(player='A')
        self.Player2=Player(player='B')
        self.turn=0
        self.count=0
        self.pauseTurn=[0,0]        
        self.img=loadImage('Button.png')
        self.background=loadImage('Bamboos.jpg')
        self.switchOwn=False
        self.combo=[]
        self.total1=self.Player1.hand1.numFin+self.Player1.hand2.numFin
        self.total2=self.Player2.hand1.numFin+self.Player2.hand2.numFin
        self.bgSound=minim.loadFile('The Righteous Gang.mp3')
        self.playSound(self.bgSound,'loop')
        self.clickSound=minim.loadFile('button.wav')
        self.again={ENTER:False}
        self.p1=False
        self.p2=False
        
    def playSound(self,sound,type):
        if type == 'once':
            sound.rewind()
            sound.play()
        else:
            sound.rewind()
            sound.loop()    
        
    def display(self):
        image(self.background,0,0,800,600)
        image(self.img,700,500,50,50)   
        for hand in self.Player1.hands:
            if hand.numFin >= 5:
                hand.numFin=0
        for hand in self.Player2.hands:
            if hand.numFin >= 5:
                hand.numFin=0
                            
        if self.pauseTurn[0] > 0:
            self.pauseTurn[0]-=1
            if self.pauseTurn[0]==0:
                self.turn=self.pauseTurn[1] 
         
        if self.turn==-100:
            if self.p1==True:
                self.pauseTurn[0]=120
                self.bgSound.close()
                self.background=loadImage("player1wins.jpg")
                image(self.background,0,0,800,600)
                textSize(40)
                text('Play Again? Press Enter Key.',140,180)
                fill(255)
            if self.p2==True:
                self.pauseTurn[0]=120
                self.bgSound.close()
                self.background=loadImage("player2wins.jpg")
                image(self.background,0,0,800,600)
                textSize(40)
                text('Play Again? Press Enter Key.',140,180)
                fill(255)
                
        if self.turn==0:
            self.Player1.displayBig()
            self.Player2.displaySmall()                    
            if self.switchOwn:
                if self.total1 == 2:    
                    if (self.Player1.hand1.numFin == 2 and self.Player1.hand2.numFin == 0) or (self.Player1.hand1.numFin == 0 and self.Player1.hand2.numFin == 2):
                        self.Player1.hand1.numFin=1
                        self.Player1.hand2.numFin=1
                        self.Turn()
                        self.switchOwn=False
                    elif self.Player1.hand1.numFin == 1 and self.Player1.hand2.numFin == 1:
                        self.Player1.hand1.numFin=2
                        self.Player1.hand2.numFin=0
                        self.Turn()
                        self.switchOwn=False
                elif self.total1 == 3:
                    if (self.Player1.hand1.numFin == 2 and self.Player1.hand2.numFin == 1) or (self.Player1.hand1.numFin == 1 and self.Player1.hand2.numFin == 2):
                        self.Player1.hand1.numFin=3
                        self.Player1.hand2.numFin=0
                        self.Turn()
                        self.switchOwn=False
                    elif (self.Player1.hand1.numFin == 3 and self.Player1.hand2.numFin == 0) or (self.Player1.hand1.numFin == 0 and self.Player1.hand2.numFin == 3):
                        self.Player1.hand1.numFin=2
                        self.Player1.hand2.numFin=1
                        self.Turn()
                        self.switchOwn=False
                elif self.total1 == 4:
                    if (self.Player1.hand1.numFin == 3 and self.Player1.hand2.numFin == 1) or (self.Player1.hand1.numFin == 1 and self.Player1.hand2.numFin == 3):
                        self.combo=[(2,2),(4,0)]
                        swap=choice(self.combo)
                        self.Player1.hand1.numFin=swap[0]
                        self.Player1.hand2.numFin=swap[1]
                        self.Turn()
                        self.switchOwn=False
                    elif (self.Player1.hand1.numFin == 4 and self.Player1.hand2.numFin == 0) or (self.Player1.hand1.numFin == 0 and self.Player1.hand2.numFin == 4):
                        self.combo=[(3,1),(2,2)]
                        swap=choice(self.combo)
                        self.Player1.hand1.numFin=swap[0]
                        self.Player1.hand2.numFin=swap[1]
                        self.Turn()
                        self.switchOwn=False
                    elif self.Player1.hand1.numFin == 2 and self.Player1.hand2.numFin == 2:
                        self.combo=[(3,1),(4,0)]
                        swap=choice(self.combo)
                        self.Player1.hand1.numFin=swap[0]
                        self.Player1.hand2.numFin=swap[1]
                        self.Turn()
                        self.switchOwn=False
                elif self.total1 == 5:
                    if (self.Player1.hand1.numFin == 4 and self.Player1.hand2.numFin == 1) or (self.Player1.hand1.numFin == 1 and self.Player1.hand2.numFin == 4):
                        self.Player1.hand1.numFin=2
                        self.Player1.hand2.numFin=3
                        self.Turn()
                        self.switchOwn=False
                    elif (self.Player1.hand1.numFin == 3 and self.Player1.hand2.numFin == 2) or (self.Player1.hand1.numFin == 2 and self.Player1.hand2.numFin == 3):
                        self.Player1.hand1.numFin=4
                        self.Player1.hand2.numFin=1
                        self.Turn()
                        self.switchOwn=False
                elif self.total1 == 6:
                    if (self.Player1.hand1.numFin == 4 and self.Player1.hand2.numFin == 2) or (self.Player1.hand1.numFin == 2 and self.Player1.hand2.numFin == 4):
                        self.combo=[(1,0),(3,3)]
                        swap=choice(self.combo)
                        self.Player1.hand1.numFin=swap[0]
                        self.Player1.hand2.numFin=swap[1]
                        self.Turn()
                        self.switchOwn=False
                    elif self.Player1.hand1.numFin == 3 and self.Player1.hand2.numFin == 3:
                        self.combo=[(1,0),(2,4)]
                        swap=choice(self.combo)
                        self.Player1.hand1.numFin=swap[0]
                        self.Player1.hand2.numFin=swap[1]
                        self.Turn()
                        self.switchOwn=False
                elif self.total1 == 7:
                    if (self.Player1.hand1.numFin == 4 and self.Player1.hand2.numFin == 3) or (self.Player1.hand1.numFin == 3 and self.Player1.hand2.numFin == 4):
                        self.Player1.hand1.numFin=2
                        self.Player1.hand2.numFin=0
                        self.Turn()
                        self.switchOwn=False
                elif self.total1 == 8:
                    if self.Player1.hand1.numFin == 4 and self.Player1.hand2.numFin == 4:
                        self.Player1.hand1.numFin=3
                        self.Player1.hand2.numFin=0
                        self.Turn()
                        self.switchOwn=False
        elif self.turn>0:
            self.Player1.displaySmall()
            self.Player2.displayBig()
            if self.switchOwn:
                if self.total2 == 2:    
                    if (self.Player2.hand1.numFin == 2 and self.Player2.hand2.numFin == 0) or (self.Player2.hand1.numFin == 0 and self.Player2.hand2.numFin == 2):
                        self.Player2.hand1.numFin=1
                        self.Player2.hand2.numFin=1
                        self.Turn()
                        self.switchOwn=False
                    elif self.Player2.hand1.numFin == 1 and self.Player2.hand2.numFin == 1:
                        self.Player2.hand1.numFin=2
                        self.Player2.hand2.numFin=0
                        self.Turn()
                        self.switchOwn=False
                elif self.total2 == 3:
                    if (self.Player2.hand1.numFin == 2 and self.Player2.hand2.numFin == 1) or (self.Player2.hand1.numFin == 1 and self.Player2.hand2.numFin == 2):
                        self.Player2.hand1.numFin=3
                        self.Player2.hand2.numFin=0
                        self.Turn()
                        self.switchOwn=False
                    elif (self.Player2.hand1.numFin == 3 and self.Player2.hand2.numFin == 0) or (self.Player2.hand1.numFin == 0 and self.Player2.hand2.numFin == 3):
                        self.Player2.hand1.numFin=2
                        self.Player2.hand2.numFin=1
                        self.Turn()
                        self.switchOwn=False
                elif self.total2 == 4:
                    if (self.Player2.hand1.numFin == 3 and self.Player2.hand2.numFin == 1) or (self.Player2.hand1.numFin == 1 and self.Player2.hand2.numFin == 3):
                        self.combo=[(2,2),(4,0)]
                        swap=choice(self.combo)
                        self.Player2.hand1.numFin=swap[0]
                        self.Player2.hand2.numFin=swap[1]
                        self.Turn()
                        self.switchOwn=False
                    elif (self.Player2.hand1.numFin == 4 and self.Player2.hand2.numFin == 0) or (self.Player2.hand1.numFin == 0 and self.Player2.hand2.numFin == 4):
                        self.combo=[(3,1),(2,2)]
                        swap=choice(self.combo)
                        self.Player2.hand1.numFin=swap[0]
                        self.Player2.hand2.numFin=swap[1]
                        self.Turn()
                        self.switchOwn=False
                    elif self.Player2.hand1.numFin == 2 and self.Player2.hand2.numFin == 2:
                        self.combo=[(3,1),(4,0)]
                        swap=choice(self.combo)
                        self.Player2.hand1.numFin=swap[0]
                        self.Player2.hand2.numFin=swap[1]
                        self.Turn()
                        self.switchOwn=False
                elif self.total2 == 5:
                    if (self.Player2.hand1.numFin == 4 and self.Player2.hand2.numFin == 1) or (self.Player2.hand1.numFin == 1 and self.Player2.hand2.numFin == 4):
                        self.Player2.hand1.numFin=2
                        self.Player2.hand2.numFin=3
                        self.Turn()
                        self.switchOwn=False
                    elif (self.Player2.hand1.numFin == 3 and self.Player2.hand2.numFin == 2) or (self.Player2.hand1.numFin == 2 and self.Player2.hand2.numFin == 3):
                        self.Player2.hand1.numFin=4
                        self.Player2.hand2.numFin=1
                        self.Turn()
                        self.switchOwn=False
                elif self.total2 == 6:
                    if (self.Player2.hand1.numFin == 4 and self.Player2.hand2.numFin == 2) or (self.Player2.hand1.numFin == 2 and self.Player2.hand2.numFin == 4):
                        self.combo=[(1,0),(3,3)]
                        swap=choice(self.combo)
                        self.Player2.hand1.numFin=swap[0]
                        self.Player2.hand2.numFin=swap[1]
                        self.Turn()
                        self.switchOwn=False
                    elif self.Player2.hand1.numFin == 3 and self.Player2.hand2.numFin == 3:
                        self.combo=[(1,0),(2,4)]
                        swap=choice(self.combo)
                        self.Player2.hand1.numFin=swap[0]
                        self.Player2.hand2.numFin=swap[1]
                        self.Turn()
                        self.switchOwn=False
                elif self.total2 == 7:
                    if (self.Player2.hand1.numFin == 4 and self.Player2.hand2.numFin == 3) or (self.Player2.hand1.numFin == 3 and self.Player2.hand2.numFin == 4):
                        self.Player2.hand1.numFin=2
                        self.Player2.hand2.numFin=0
                        self.Turn()
                        self.switchOwn=False
                elif self.total2 == 8:
                    if self.Player2.hand1.numFin == 4 and self.Player2.hand2.numFin == 4:
                        self.Player2.hand1.numFin=3
                        self.Player2.hand2.numFin=0
                        self.Turn()
                        self.switchOwn=False
        

    def Turn(self):
# Determine the turn of the players
        self.count+=1
        if self.count < 2:
            self.turn=0
        elif 2 <= self.count < 3:
            self.pauseTurn=[120,1]
        elif self.count==4:
            self.count=0
            self.pauseTurn=[120,0] 
        self.total1=self.Player1.hand1.numFin+self.Player1.hand2.numFin
        self.total2=self.Player2.hand1.numFin+self.Player2.hand2.numFin
        print self.count, self.turn
        print self.Player1.hand1.numFin, self.Player1.hand2.numFin, self.total1
        print self.Player2.hand1.numFin, self.Player2.hand2.numFin, self.total2
        return self.turn
 
       
    def update(self):
        if self.turn==0:
            for hand in game.Player1.hands:
                for choice in game.Player2.hands:
                    if hand.value == True and choice.value == True:
                        choice.numFin=hand.numFin+choice.numFin
                        hand.value=False
                        choice.value=False
                        return
        else:
            for hand in game.Player2.hands:
                for choice in game.Player1.hands:
                    if hand.value == True and choice.value == True:
                        choice.numFin=hand.numFin+choice.numFin
                        hand.value=False
                        choice.value=False
                        return            


    def Click(self):     
        if self.turn == 0:
            if self.Player1.hand1.value == False and self.Player1.hand2.value == False:
                if 700 < mouseX < 750 and 500 < mouseY < 550:
                    self.switchOwn=True
                    self.Turn()
                for hand in self.Player1.hands:
                    if hand.x < mouseX < hand.x + hand.s and hand.y < mouseY < hand.y + hand.s:
                        hand.value=True
                        self.Turn()
            else:
                for hand in self.Player2.hands:
                    if hand.x < mouseX < hand.x + hand.s and hand.y < mouseY < hand.y + hand.s:
                        hand.value=True
                        self.update()
                        self.Turn()

        elif self.turn == 1:
            if self.Player2.hand1.value == False and self.Player2.hand2.value == False:
                if 700 < mouseX < 750 and 500 < mouseY < 550:
                    self.switchOwn=True
                    self.Turn()
                for hand in self.Player2.hands:
                    if hand.x < mouseX < hand.x + hand.s and hand.y < mouseY < hand.y + hand.s:
                        hand.value=True
                        self.Turn()
            else:
                for hand in self.Player1.hands:
                    if hand.x < mouseX < hand.x + hand.s and hand.y < mouseY < hand.y + hand.s:
                        hand.value=True
                        self.update()
                        self.Turn()
                 
                                                                               
    def CheckWinner(self):
        if self.Player1.hands[0].numFin not in range(1,5) and self.Player1.hands[1].numFin not in range(1,5):
            self.switchOwn=False
            self.p2=True
            self.turn=-100
        if self.Player2.hands[0].numFin not in range(1,5) and self.Player2.hands[1].numFin not in range(1,5):
            self.switchOwn=False
            self.p1=True
            self.turn=-100
            
            
    def Again(self):
        if self.again[ENTER]:
            self.p1=False
            self.p2=False

game=Game()

def setup():
    size(800,600)
    background(255)
    stroke(255)
    
def draw():
    background(255)
    game.display()    
    
def mouseClicked():
    if game.pauseTurn[0] == 0:
        game.Click()
        game.playSound(game.clickSound,'once')
    game.CheckWinner()
        
def keyPressed():
    game.again[ENTER]=True
    game.Again()
    if game.p1==False and game.p2==False:
        game.__init__()
 
def keyReleased():
    game.again[keyCode]=False
