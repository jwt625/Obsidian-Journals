Following: https://github.com/AlexanderKoch-Koch/low_cost_robot

# Printing follower arm

2024-08-05T20:44:39-07:00
Also still need to order all the other parts and tools.
And also need another table (ideally). Maybe I could use the existing desk for now.

Follower arm STL files:
- https://github.com/AlexanderKoch-Koch/low_cost_robot/tree/main/hardware/follower/stl

2024-08-05T21:14:02-07:00
Started printing ~ 15 min ago. No support. Chose the PLA strength default and reduced the outer shell from 6 to 4.
![[Pasted image 20240805225956.png]]

2024-08-05T23:00:02-07:00
- the part with the curly tip did not stick around 35% maybe. Removed it
- all other ones are still printing fine

2024-08-05T23:07:41-07:00
Found out I could add support to individual objects.
![[Pasted image 20240805230751.png]]

Still trying to find out where do I cancel objects.
- done. Can't do it on desktop studio and had to get the app...


# Continue the build
2024-10-17T20:13:00-07:00
Damn it's been more than two months since I printed the parts for the arm. Let's follow the instructions and start buying things.

## Printing leader arm
2024-10-17T21:51:35-07:00

`Documents\3D_print\low_cost_robot-main\hardware\leader`
![[Pasted image 20241017215344.png]]
2024-11-03T20:10:57-08:00
Hmm found one part missing.
![[Pasted image 20241103201153.png]]
Oh this part is from the robotis.


## Ordering
2024-10-17T22:01:45-07:00

On https://www.robotis.us/:
![[Pasted image 20241017220149.png]]
- placed
- Total $237.5
![[Pasted image 20241017220429.png]]

Amazon:
![[Pasted image 20241017220457.png]]

Misc:
- probably need some screws and heat set threaded insert
![[Pasted image 20241017221851.png]]




# Assembly
2024-10-29T20:19:14-07:00

https://www.youtube.com/watch?v=RckrXOEoWrk&ab_channel=AlexanderKoch

Fuck, forgot to order the leader arm materials.
Ordered:
![[Pasted image 20241029214032.png]]
![[Pasted image 20241029214408.png]]
2024-10-29T22:41:46-07:00
Rough assembly done. Need to wait for leader arm parts...
Also there is one motor that is not used??
- ok I think it is for the extension arm that is not shown in the youtube video (https://www.youtube.com/watch?v=RckrXOEoWrk&ab_channel=AlexanderKoch)


2024-11-03T22:53:20-08:00
Finished assembling both leader and follower!
![[signal-2024-11-03-225415_002.jpeg]]



# Simulation
2025-05-09T00:22:54-07:00
![[Pasted image 20250509002255.png]]

`/usr/local/bin/python3 -m pip install mujoco-python-viewer`

2025-05-09T00:26:51-07:00

Have to run the sim with:
- `mjpython simulation.py`

holy shit it's working?
![[Pasted image 20250509002707.png]]

Next should try `teleoperate_simulated_robot.py`

Reading:
- https://www.waveshare.com/wiki/Bus_Servo_Adapter_(A)
