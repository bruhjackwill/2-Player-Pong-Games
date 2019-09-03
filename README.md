# 2-Player-Pong-Game
This game was made by Giles Booth. 

The new code was a group effort with simple additions to the existing code. 

This was a school project for Computer Science.

Hope you like our project.

Original code for the two player pong game:
https://github.com/blogmywiki/pongo

![20190827_101954 (1)](https://user-images.githubusercontent.com/54100604/63755579-409b4a00-c8b7-11e9-8639-8935bc3fbf10.jpg)

The modified code by William White.

Idea by Ibrahim Abdou aswell as the formating and typing of this github page.

Jack Tucker for the extension ideas, sensors and extra microbit when testing.

#Modified code for Microbit A:

import radio

import random

import speech

from microbit import *

from music import play, POWER_UP, JUMP_DOWN, NYAN, FUNERAL


a_bat = 2

b_bat = 2

bat_map = {0: 4, 1: 3, 2: 2, 3: 1, 4: 0}

ball_x = 2

ball_y = 2

b_ball_x = 2

b_ball_y = 2

directions = [1, -1]

x_direction = random.choice(directions)

y_direction = random.choice(directions)

b_x_direction = random.choice(directions)

b_y_direction = random.choice(directions)

delay = 600

counter = 0

a_points = 0

b_points = 0

winning_score = 5

twoballs = False

game_over = False



def b_move_ball():

    global ball_x, ball_y, x_direction, y_direction, counter
    
    global a_bat, b_bat, a_points, b_points, delay
    
    global b_ball_x, b_ball_y, b_x_direction, b_y_direction, twoballs
    
    display.set_pixel(b_ball_x, b_ball_y, 0)
    
    b_ball_x = b_ball_x + b_x_direction
    
    b_ball_y = b_ball_y + b_y_direction
    
    if b_ball_x < 0:
    
        b_ball_x = 0
        
        b_x_direction = 1
        
    if b_ball_x > 4:
    
        b_ball_x = 4
        
        b_x_direction = -1
        
    if b_ball_y == 0:
    
        if b_ball_x == b_bat:
        
            b_ball_y = 0
            
            b_y_direction = 1
            
            delay -= 50
            
        else:
        
            play(POWER_UP, wait=False)
            
            a_points += 1
            
            b_ball_y = 0
            
            b_y_direction = 1
            
            radio.send("a" + str(a_points))
            
    if b_ball_y == 4:
    
        if b_ball_x == a_bat:
        
            b_ball_y = 4
            
            b_y_direction = -1
            
            delay -= 50
            
        else:
        
            play(JUMP_DOWN, wait=False)
            
            b_points += 1
            
            b_ball_y = 4
            
            b_y_direction = -1
            
            radio.send("b" + str(b_points))
            
            
    counter = 0
    radio.send("d" + str(b_ball_x))
    radio.send("f" + str(b_ball_y))



def move_ball():

    global ball_x, ball_y, x_direction, y_direction, counter
    
    global b_ball_x, b_ball_y, b_x_direction, b_y_direction, twoballs
    
    global a_bat, b_bat, a_points, b_points, delay
    
    display.set_pixel(ball_x, ball_y, 0)
    
    ball_x = ball_x + x_direction
    
    ball_y = ball_y + y_direction
    
    if ball_x < 0:
    
        ball_x = 0
        
        x_direction = 1
        
    if ball_x > 4:
    
        ball_x = 4
        
        x_direction = -1
        
    if ball_y == 0:
    
        if ball_x == b_bat:
        
            ball_y = 0
            
            y_direction = 1
            
            delay = delay - 50
            
        else:
        
            play(POWER_UP, wait=False)
            
            a_points += 1
            
            ball_y = 0
            
            y_direction = 1
            
            radio.send("a" + str(a_points))
            
    if ball_y == 4:
    
        if ball_x == a_bat:
        
            ball_y = 4
            
            y_direction = -1
            
            delay -= 50
            
        else:
        
            play(JUMP_DOWN, wait=False)
            
            b_points += 1
            
            ball_y = 4
            
            y_direction = -1
            
            radio.send("b" + str(b_points))
            
    counter = 0
    
    radio.send("x" + str(ball_x))
    
    radio.send("y" + str(ball_y))
    


radio.on()

radio.config(channel=69)

radio.config(power=4)


while not game_over:

    if a_points > 1 or b_points > 1:
    
        radio.send("t")
        
        twoballs = True
        
    counter += 1
    
    display.set_pixel(a_bat, 4, 6)
    
    display.set_pixel(b_bat, 0, 6)
    
    display.set_pixel(ball_x, ball_y, 9)
    
    
    if twoballs is True:
    
        display.set_pixel(b_ball_x, b_ball_y, 9)
        
    if button_a.was_pressed():
    
        display.set_pixel(a_bat, 4, 0)
        
        a_bat = a_bat - 1
        
        if a_bat < 0:
        
            a_bat = 0
            
        radio.send("p" + str(a_bat))
        
    if button_b.was_pressed():
    
        display.set_pixel(a_bat, 4, 0)
        
        a_bat = a_bat + 1
        
        if a_bat > 4:
        
            a_bat = 4
            
        radio.send("p" + str(a_bat))
        
    incoming = radio.receive()
    
    if incoming:
    
        display.set_pixel(b_bat, 0, 0)
        
        b_bat = bat_map[int(incoming)]
        
    if counter == delay:
    
        move_ball()
        
        if twoballs is True:
        
            b_move_ball()
            
    if a_points == winning_score or b_points == winning_score:
    
        game_over = True
        
if a_points > b_points:

    play(NYAN, wait=False)
    
    display.scroll("A wins!")
    
else:

    play(FUNERAL, wait=False)
    
    display.scroll("B wins!")
    
display.scroll("Press reset to play again")



#Code for Microbit B:


import speech

import radio

from microbit import *

from music import play, POWER_UP, JUMP_DOWN, NYAN, FUNERAL

a_bat = 2

b_bat = 2

bat_map = {0: 4, 1: 3, 2: 2, 3: 1, 4: 0}

ball_x = 2

ball_y = 2

b_ball_x = 2

b_ball_y = 2

a_points = 0

b_points = 0

winning_score = 5

game_over = False

twoballs = False

radio.on()

radio.config(channel=69)

radio.config(power=4)



def parse_message():

    global a_bat, incoming, bat_map, ball_x, ball_y, a_points, b_points
    
    global twoballs, b_ball_x, b_ball_y
    
    msg_type = incoming[:1]
    
    msg = incoming[1:]
    
    if msg_type == "t":
    
        twoballs = True
        
    if msg_type == "p":
    
        display.set_pixel(a_bat, 0, 0)
        
        their_bat = int(msg)
        
        a_bat = bat_map[their_bat]
        
    if msg_type == "x":
    
        display.set_pixel(ball_x, ball_y, 0)
        
        ball_x = bat_map[int(msg)]
        
    if msg_type == "y":
    
        display.set_pixel(ball_x, ball_y, 0)
        
        ball_y = bat_map[int(msg)]
        
    if twoballs is True:
    
        if msg_type == "d":
        
            display.set_pixel(b_ball_x, b_ball_y, 0)
            
            b_ball_x = bat_map[int(msg)]
            
        if msg_type == "f":
        
            display.set_pixel(b_ball_x, b_ball_y, 0)
            
            b_ball_y = bat_map[int(msg)]
            
    if msg_type == "a":
    
        a_points = int(msg)
        
        play(JUMP_DOWN, wait=False)
        
    if msg_type == "b":
    
        b_points = int(msg)
        
        play(POWER_UP, wait=False)


while not game_over:

    display.set_pixel(b_bat, 4, 6)
    
    display.set_pixel(a_bat, 0, 6)
    
    display.set_pixel(ball_x, ball_y, 9)
    
    if twoballs is True:
    
        display.set_pixel(b_ball_x, b_ball_y, 9)
        
    if button_a.was_pressed():
    
        display.set_pixel(b_bat, 4, 0)
        
        b_bat = b_bat - 1
        
        if b_bat < 0:
        
            b_bat = 0
            
        radio.send(str(b_bat))
        
    if button_b.was_pressed():
    
        display.set_pixel(b_bat, 4, 0)
        
        b_bat = b_bat + 1
        
        if b_bat > 4:
        
            b_bat = 4
            
        radio.send(str(b_bat))
        
    incoming = radio.receive()
    
    if incoming:
    
        parse_message()
        
    if a_points == winning_score or b_points == winning_score:
    
        game_over = True
        
if a_points < b_points:

    play(NYAN, wait=False)
    
    display.scroll("B wins!")
    
else:

    play(FUNERAL, wait=False)
    
    display.scroll("A wins!")


