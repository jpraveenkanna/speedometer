# speedometer
RAs rom



import RPi.GPIO as GPIO
from time import sleep
import time, math


import RPi.GPIO as GPIO  
from time import sleep    
GPIO.setmode(GPIO.BCM)      
GPIO.setup(25, GPIO.IN)
GPIO.setup(21,GPIO.OUT)

dist_meas = 0.00
km_per_hour = 0
rpm = 0
elapse = 0
sensor = 29
pulse = 0
start_timer = time.time()
def init_GPIO():            
   GPIO.setmode(GPIO.BOARD)
   GPIO.setwarnings(False)
   GPIO.setup(sensor,GPIO.IN,GPIO.PUD_UP)

def calculate_elapse(channel):            
   global pulse, start_timer, elapse
   pulse+=1                        
   elapse = time.time() - start_timer
   start_timer = time.time()            

def calculate_speed(r_cm):
   global pulse,elapse,rpm,dist_km,dist_meas,km_per_sec,km_per_hour
   if elapse !=0:                     
      rpm = 1/elapse * 60  
	  return rpm
      return km_per_hour
	  
# Define a threaded callback function to run in another thread when events are detected  
def my_callback(channel): 
	
    if GPIO.input(25):     # if port 25 == 1  
        print ("1")
        GPIO.output(21,1)
        sleep(0.009)
    else:                  # if port 25 != 1  
        print ("0")
        GPIO.output(21,0) 
	GPIO.add_event_detect(25, GPIO.BOTH, callback=my_callback)     
	raw_input("Press Enter when ready\n>")    
	try:  
		print ("When pressed, you'll see: Rising Edge detected on 25")  
		print ("When released, you'll see: Falling Edge detected on 25")    
		print ("Time's up. Finished!")  
	finally:                   
		GPIO.cleanup()

def init_interrupt():
   GPIO.add_event_detect(sensor, GPIO.FALLING, callback = calculate_elapse, bouncetime = 20)
if __name__ == '__main__':
   init_GPIO()
while True:
      init_interrupt()
      calculate_speed(24) 
	  my_callback()
      print('rpm:{0:.0f} pulse:{3}'.format(rpm,km_per_hour,dist_meas,pulse))
	  if rpm>399:
		sleep(0.04)
	  else:
		sleep(0.07)


  


