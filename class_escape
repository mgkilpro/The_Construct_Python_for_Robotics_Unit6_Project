from robot_control_class import RobotControl                                    #robot_control_class is in "The Construct Robotics for Python Unit 6"
import time                                                                     #for "motor" method
import math                                                                     #for calculating limit of distance from wall

class escape:

    def __init__(self):
        self.robotcontrol = RobotControl()
    
    def left(self):
        print("Turning LEFT")
        self.robotcontrol.rotate(-90)                                           #turn robot left pi/2


    def right(self):
        print("Turning RIGHT")
        self.robotcontrol.rotate(90)                                            #turn robot right pi/2

    
    def motor(self):                                                            #make move robot straight while 1sec
      speed = 0.25
      time = 1
      
      print("[COMMAND] : motor")
      self.robotcontrol.move_straight_time("forward", speed, time)
      print("[COMMAND] : motor end")

    def drive(self):                                                            #make move robot without crashing to wall

      centerLaser = 360                                                         #front angle of laser sensor
      sideLaser2 = 150                                                          #side angle of laser sesor -> [left : -150 * pi / 720] & [right : 150 * pi / 720]
      sideLaser1 = 0                                                            #side angle of laser sesor -> [left : -pi/2] & [right : pi/2]

      radius = 0.7                                                              #radius from robot : limit distance of front & side1
      driver = radius / math.cos(math.pi * sideLaser2 / 720)                    #limit distance of side2
      print("Driver Distanse %.4f" %(driver))

      framing = 10                                                              #angle of correcting direction of the robot
      infinity = 10                                                             #using for detecting escape completeley

      while True:
        
        laserValue = self.robotcontrol.get_laser_full()
        print("-------------------------------------------------------------")
        print("[CNETER LASER] %.4f / [LEFT2 LASER] %.4f / [RIGHT2 LASER] %.4f" %(laserValue[centerLaser], laserValue[719-sideLaser2], laserValue[sideLaser2]))
        print("LEFT1 LASER] %.4f / [RIGHT1 LASER] %.4f" %(laserValue[719-sideLaser1], laserValue[sideLaser1]))
        print("-------------------------------------------------------------")

        if (laserValue[centerLaser] < radius) or (infinity <= laserValue[sideLaser1] and infinity <= laserValue[719 - sideLaser1]):    #center laser distance is shorter than limit or escape completely

          print("[CENTER LASER] Wall Detected")
          self.robotcontrol.stop_robot()
          break

        elif laserValue[sideLaser2] < driver:                                   #right2 laser detected wall

          print("[RIGHT2 LASER] Wall Detected")
          self.robotcontrol.rotate(-framing)                                    #correcting direction of the robot
          self.motor()                                                          #go straight little : [speed = 0.25 , time : 1sec]

        elif laserValue[719-sideLaser2] < driver:

          print("[LEFT2 LASER] Wall Detected")
          self.robotcontrol.rotate(framing)

        elif laserValue[sideLaser1] < radius:                                   #right1 laser detected wall : direction of robot is worse than above case

          print("[RIGHT1 LASER] Wall Detected")
          self.robotcontrol.rotate(-framing*2)                                  #correcting direction of the robot : two times larger than above case
          self.motor()

        elif laserValue[719-sideLaser1] < radius:

          print("[LEFT1 LASER] Wall Detected")
          self.robotcontrol.rotate(framing*2)

        self.robotcontrol.move_straight()

# action START
action = escape()
action.drive()
action.right()
action.drive()
action.right()
action.drive()
action.left()
action.drive()

print("Escape Completed => Program Finished")
