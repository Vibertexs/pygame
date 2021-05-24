# -*- coding: utf-8 -*-
"""
Created on Sat May 15 22:39:37 2021

@author: Bilal
"""

import pygame #Python lIbrary making it easier to visualise programs
import math #Library that will perform our complex math
import numpy as np #Numpy is a library used for number calculations
from matplotlib import pyplot as plt #Matplotlib is used to make graphs
pygame.init() #Intializes pygame for use
#720p resolution
ScreenX = 1280 #Width of the window
ScreenY = 720 #Height of the window
#Initalize all the variables
run = True #To start the program when this runs
time = 0 #Default time
power = 0 #Default power
angle = 0 #Default angle
shoot = False #To signal that the projectile is not fired
flight_number = 1 #Counter of how many flights have passed
gravity = 9.80665 #Exact Value of acceleration due to gravity
#Color Codes for easy refrence
red = (230,57,70) #Color code for imperial red
blue = (20,33,61) #COlor Code for dark navy blue
yellow = (255,201,34)
#Start the clock
clock = pygame.time.Clock()
#Make the window and name it prjectile motion simulation
window = pygame.display.set_mode((ScreenX,ScreenY))
pygame.display.set_caption('Projectile Motion Simulation')
#Set up the font
font = pygame.font.Font('freesansbold.ttf',32) #Text font: freesansbold size 32
#Power text setup
textPower = font.render('', True, red) #Intially will display blank
powerRect = textPower.get_rect() #Creates the rectangle that our text will go in
powerRect.center = (10, 20) #The box's center will be located at 10,20
#Angle text setup
textAngle = font.render('', True, red) #Intially will display blank
angleRect = textPower.get_rect() #Creates the rectangle that our text will go in
angleRect.center = (10, 50) #The box's center will be located at 10,50
#Flight Time text setup
textFlightTime = font.render('', True, red) #Intially will display blank
flightTimeRect = textPower.get_rect() #Creates the rectangle that our text will go in
flightTimeRect.center = (10, 80) #The box's center will be located at 10,80
#Class for the projectile to instantiated and easy access to its properties
class projectileObject(object):
    #Init initializes all the properties relating to this class
    def __init__(self,x,y,radius,color): #Takes 4 arguments because self does not count
        self.x = x #X Position
        self.y = y #Y Position
        self.radius = radius #The Radius
        self.color = color #Its color
    #When called upon this function will draw the ball
    def draw(self, window):
        #Draws a circle on the window with the color white, using its properties that will be passed through
        pygame.draw.circle(window, (self.color), (self.x,self.y), self.radius) 
    #Static method typically does work for the class
    @staticmethod
    def projectilePath(startx, starty, velocity, angle, time):
        #Calculate each axis velocity
        velx = math.cos(angle) * velocity #Finding the horizontal velocity by using cos
        vely = math.sin(angle) * velocity #Finding the horizontal velocity by using sin
        #Calculate the change using the formula Distance = Velocity*Time
        changeX = velx * time #Here drag is negligble
        changeY = (vely * time) + (((-1)*gravity* (time ** 2))) #Calculate Gravity by finding the time and subratracting by the gravity*time^2
        newx = round(changeX + startx) #New X position estimate
        newy = round(starty - changeY) #New Y Position estimate
        return (newx, newy) #Return the new positions
#This function will redraw the window whenever called upon
def refreshScreen(): 
    #Fills the background to blue
    window.fill((blue))
    #Displays each text seperatly on the window
    window.blit(textPower, powerRect) #Power text
    window.blit(textAngle, angleRect) #Angle Test
    window.blit(textFlightTime, flightTimeRect) #Flight time text
    #Function we difined in the class we use here
    projectile.draw(window) #Draws the projectile
    pygame.display.update() #Updats the screen
#Used to find the angle
def findAngle(position):
    #We Use Arctan to find the angle between 2 points, the argument that is passed is the mouse pointer
    return abs(math.atan2(position[1]-projectile.y, position[0]-projectile.x)) #Returned in radians
#Used to find the power
def findPower(pos):
    #Distance Formula; We use the distance formula between the projectile and the mouse cursor: Linear Relationship
    return (math.sqrt((pos[1]-projectile.y)**2 + (pos[0]-projectile.x)**2)/4.5) #Divided by 4.5 because there is too much power
#Displays all the stats in the console
def display_stats(velocity, angle, flightTime):
    print("Inital Velocity:", velocity, "m/s") #Prints the velocity
    print("Launch Angle:", angle, "°") #Prints the angle
    print("Flight Time:", flightTime, "s") #Prints the predicted flight time
    maximumHeightTime = velocity*math.sin(math.radians(angle))/gravity # Calculates the time it needs to reach maximum height
    print("Time at Maximum Height:",maximumHeightTime, "s")  #Prints the maximum height
    maxHeight = (velocity*math.sin(angle))**2/(2*gravity)  #Calculate how high the height is
    print("Max Height:", maxHeight, "m") #Prints the max height
    print("\n\n\n------------------------------------------------------------------")#Makes it easier to view multiple graphs
#Graph the flight path in console
def display_FlightPath(angle,velocity, flight_number):
    print("\n\n\n") #Makes it easier to view multiple graphs
    print("Flight Number:", flight_number) #Prints the flight number at the top
    u = velocity #Changeing it to u to make it easier to use in the formula
    a=angle #In terms of showing the equation better
    x=np.arange(0,15,0.01) #Makes an array of 0 15 with atep size of 0.01:[0,0.01,0.02,0.02...15.0]
    #Formula = tan(θ)*x-9.8/2*xVel^2*cos^2(θ) * x^2
    y = (math.tan(angle)*(x)-((9.8*x**2)/((2)*(u**2)*(math.cos(a)**2)))) #Used to display the porabla
    #Graph the plots
    fig, ax = plt.subplots() #Used to display multiple plots
    ax.plot(x, y) #Graphs the x and y
    plt.ylim([0,8]) #Set the y limit to 0,8
    plt.xlim([0,10]) #Set the x limit to 0,10
    plt.xlabel("Distance meters") #X label
    plt.ylabel("Height meters") #Y Label
    plt.show() #Show the graph in console
#Prevents the ball from going out of the x boundiries
def outOfBounds(x): #We only need to check for the X
    if x>ScreenX: #Checks if the x is greater than the boundry, meaning if its too far left
        projectile.x = ScreenX-projectile.radius #Set it 10 pixels to the left
    elif x<0: #Check if its too far right
        projectile.x = projectile.radius #Set it 10 pixel right
#Ask for the radius input
try:
    #try to ask
    rad = int(input("Enter the radius as an integer:"))
except:
    #if they faid, default radius is 10
    print("You failed to enter an integer value, the radius is now 10")
    rad = 10
#Instantiates an instance of the projectile class
projectile = projectileObject((ScreenX/2),(ScreenY-1-rad),rad,(yellow))
#The loop that runs repeatedly
while run:
    #How fast the clock ticks
    clock.tick(100) #The faster clock tick the faster the simulation
    pos =pygame.mouse.get_pos() #Gets the mouse position
    if shoot: #Checks if the projectile is shot
        if projectile.y < ScreenY - projectile.radius: #checks if its in the air
            time += 0.08181637000000002 #To make it real world time
            po = projectile.projectilePath(x, y, velocity, angle, time) #We use this to get easier access to the projectile properties
            projectile.x = po[0] #This is the x
            projectile.y = po[1] #This is the y
            outOfBounds(projectile.x) #Checks for out of bounds
        else:
            shoot = False #Set the shoot to false meaning the projectile is at rest and is not launched
            time = 0 #Rest the time
            projectile.y = (ScreenY-projectile.radius-1) #Set the projectile on the ground
    #Calculate important measurements needed for calculations
    p = (findPower(pos)/15) #Find the power however the display is 15 less for better visual scale
    a  = (math.degrees(findAngle(pygame.mouse.get_pos()))) #FInd the angle in degrees
    f = (abs(2*p*math.sin(((findAngle(pygame.mouse.get_pos()))))/gravity)) #Find the flight time
    #Set the texts and rendering
    textP = ("Initial Velocity:"+(str(int(p))+"m/s")) #To give an effect of us being zoomed in we divide by 15
    textA = ("Angle:"+str(int(a))+"°") #Text is set equal to the angle
    textF = ("Flight Time:"+str(f)+"s") #Text is set to the flight time
    textPower = font.render(textP, True, red) #Render the velocity text
    textAngle = font.render(textA, True, red) #Render the angle text
    textFlightTime = font.render(textF, True, red) #Render the flight time text
    refreshScreen() #refresh the screen
    #Checks all the events that are registered
    for event in pygame.event.get():
        #If game is exited then quit this loop by setting run to false
        if event.type == pygame.QUIT:
            run = False
        #If mouse button is clicked then shoot
        if event.type == pygame.MOUSEBUTTONDOWN:   
            #Set it to shoot if not shot
            if not shoot:
                x = projectile.x #Save the old x
                y = projectile.y #Save the old y
                #Changeing the angle for the graph
                if a>90:
                    a= 180-a
                #Set shoot to true
                shoot = True
                #The velocity that we find using our function
                velocity = findPower(pos)
                #Finding angle using our function
                angle = findAngle(pos)
                #Displays the graphs                
                display_FlightPath(math.radians(a), p,flight_number)
                #Display the stats in the console
                display_stats(p,a,f)
                #Increase flight number
                flight_number+=1   
pygame.quit() #Terminates the pygame code
quit() #Terminates the python code

