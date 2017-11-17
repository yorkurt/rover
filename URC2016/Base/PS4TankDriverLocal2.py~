import sys
sys.path.insert(0, "../lib")
import time
import pygame
import serial
import socket
from pygame.locals import *
from udpTX import *

# R = 0
# F = 1

class InputManager:

    L_PWM_SPEED = 0
    L_PWM_STRING = 0
    L_DIR = 'B'
    R_PWM_SPEED = 0
    R_PWM_STRING = 0
    R_DIR = 'B'

    #IP = "192.168.7.2"  	#UDP BBB
    #IP = "127.0.0.1"    	#UDP LOCAL
    IP = "10.109.119.13"	#UDP HSMM
    #PORT = 5005
    PORT = 15005
    #PORT = 22    
    TX = udpTX(IP,PORT)
    sock = socket.socket(socket.AF_INET,
			socket.SOCK_DGRAM)

    def __init__(self):
        print 'joystick:',pygame.joystick.get_count()
        self.init_joystick()
        self.isRunning = True

    def init_joystick(self):

        joystick = pygame.joystick.Joystick(0)# num = ID
        joystick.init()
        self.joystick = joystick
        self.joystick_name = joystick.get_name()
        self.numAxis = self.joystick.get_numaxes()
        self.numBalls = self.joystick.get_numballs()
        self.numButtons = self.joystick.get_numbuttons()
        self.numHats = self.joystick.get_numhats()
        print 'name    :',self.joystick_name
        print 'axis    :',self.numAxis
        print 'balls   :',self.numBalls
        print 'buttons :',self.numButtons
        print 'hats    :',self.numHats
        self.prevAxis = list()
        for i in range(self.numAxis):
            self.prevAxis.append(0)
        self.prevBalls = list()
        for i in range(self.numBalls):
            self.prevBalls.append(0)
        self.prevButtonsUp = list()
        for i in range(self.numButtons):
            self.prevButtonsUp.append(0)
        self.prevButtonsDown = list()
        for i in range(self.numButtons):
            self.prevButtonsDown.append(0)
        self.prevHats = list()
        for i in range(self.numHats):
            self.prevHats.append(0)
        
    def doStuff(self):
	self.localStr = ""
        for event in pygame.event.get():
            eventType = event.type
            # print 'event : ' + str(eventType)
            if eventType == JOYAXISMOTION:
                for i in range(self.numAxis):
                    	axis = self.joystick.get_axis(i)



			if i == 1:
				self.L_PWM_STRING = str(int(self.L_PWM_SPEED))
				self.L_PWM_SPEED = axis * -100
				if self.L_PWM_SPEED > 0:
					#self.L_DIR = 'F'
					#self.L_DIR = 99
					self.L_DIR = "99"
					#self.localStr = "w"
				if self.L_PWM_SPEED < 0:
					self.L_PWM_SPEED = self.L_PWM_SPEED * -1
					#self.L_DIR = 'R'
					#self.L_DIR = 0
					self.L_DIR = 0
					#self.localStr = "a"
				if self.L_PWM_SPEED == 100:
					self.L_PWM_STRING = "99"
				if self.L_PWM_SPEED == 0:
					self.L_PWM_STRING = "00"
					self.L_DIR = 0	
			if i == 5:
				self.R_PWM_SPEED = axis * -100
				self.R_PWM_STRING = str(int(self.R_PWM_SPEED))
				if self.R_PWM_SPEED > 0:
					#self.R_DIR = 'F'
					self.R_DIR = 99
					#self.localStr = "d"
				if self.R_PWM_SPEED < 0:
					self.R_PWM_SPEED = self.R_PWM_SPEED * -1
					self.R_PWM_STRING = str(int(self.R_PWM_SPEED))
					#self.R_DIR = 'R'
					self.R_DIR = 0
					#self.localStr = "s"
				if self.R_PWM_SPEED == 100:
					self.R_PWM_STRING = "99"
				if self.R_PWM_SPEED == 0:
					self.R_PWM_STRING = "00"
					self.R_DIR = 0

            if eventType == JOYBALLMOTION:
                for i in range(self.numBalls):
                    ball = self.joystick.get_ball(i)
                    if self.prevBalls[i] != ball:
                        self.prevBalls[i] = ball
                        #self.sendCommand('ball:'+str(i)+':'+str(ball)+'\n')
            if eventType == JOYHATMOTION:
                for i in range(self.numHats):
                    hat = self.joystick.get_hat(i)
                    if self.prevHats[i] != hat:
                        self.prevHats[i] = hat
                        #self.sendCommand('hat:'+str(i)+':'+str(hat)+'\n')
            if eventType == JOYBUTTONUP:
                for i in range(self.numButtons):
                    button = self.joystick.get_button(i)
                    if self.prevButtonsUp[i] != button:
                        self.prevButtonsUp[i] = button
                        #self.sendCommand('button_up:'+str(i)+':'+str(button)+'\n')
		self.localStr = "h"
            if eventType == JOYBUTTONDOWN:
                for i in range(self.numButtons):
                    button = self.joystick.get_button(i)
                    if self.prevButtonsUp[i] != button:
                        self.prevButtonsUp[i] = button        
	                #self.sendCommand('button_down:'+str(i)+':'+str(button)+'\n')
	    #outstring = self.L_PWM_STRING + self.L_DIR+","+ self.R_PWM_STRING+ self.R_DIR
	    outstring = self.L_PWM_STRING+","+str(self.L_DIR)+","+self.R_PWM_STRING+","+str(self.R_DIR)
	    #outstring = self.localStr
	    print outstring
            #if outstring != "":
	    self.TX.tx(outstring)
def main():
    sock = socket.socket(socket.AF_INET, #Internet
                         socket.SOCK_DGRAM) #UDP
    delay = 0.001
    pygame.init()
    input_manager = InputManager()
    while input_manager.isRunning:
    	input_manager.doStuff()
        time.sleep(delay)

if __name__ == '__main__':
    main()
