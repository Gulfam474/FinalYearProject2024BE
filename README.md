# FinalYearProject2024BE
''' BE final year, technology used - python, media pipe, pyautogui, OS and OpenCV python libraries  
project working - performing Mouse operation like scroll, click, double click and many more using hand recognition system using mediapipe,pyautogui and opencv
brief working- using live video capturing frames performing hand recognition by using meadiapipe hands class(, mediapipe is a google build library for face detection and hand detection, licensed only acces.
                it has a hans() class in solution module
                1st step is to import the mediapipe library by using import mediapipe as mp statement 
                2nd step is assign 1 varible to mediapipe hands class by hands = mp.Solutions.hands.Hands()
                3rd step - Hands class have process() method that assign hand landmark location and give this image as output it assign 21 lanmark to image and detect multiple hands at a time
                4th step - Hands class also have HandLandmark list which have all the landmark present in list formate which has x,y,z co-ordinates where (x,y) is coordinate and z is depth.
                5th step - HandLandmark class also has all hand finger name like thumb_tip,index_tip,pinky_tip which has it location coordinate present in a variables)
,openCV (,opencv is python library stands for open source computer vision library, openCV is open source library used for image processing,video capturing and analysis, including object detection, face                                      detection and many more, in this project opencv used for VideoCapturing live feed from camera, in cv2(another name for openCV) the VideoCapture() fuction is present it take argument 0 or 1 ,0 for inbuild                   camera 1 for external camera and return a video object which have method like read() ,isOpned(),released(). read() return 1 boolean value true or false as it detect frame caprure is successful or not and                   return also frame of videos. isOpened() return boolean value it indicates that video source is runing or not. release() all the frame of video
                cvtColor convert BGR to RGB  it take 1st argument as frame and 2nd argument as at what type you want to convert like RGB and return type convert image.
                cv2.imshow() used for visualizations purpose it take 1st argument as a title and 2nd argument as a frame which return by VideoCapture()	object open method
                cv2.waitKey() used for press key for exiting the process like 1 key press on keyboard.
                cv2.destroyAllWindows() used for close all tab of frame.
            Function	                Description
            cv2.VideoCapture()	      Captures video from a webcam or file.
            cv2.cvtColor()	          Converts the color format (e.g., BGR to RGB).
            cv2.imshow()	            Displays an image or video frame.
            cv2.waitKey()            	Waits for a key press to proceed or exit.
            cv2.destroyAllWindows()	  Closes all OpenCV windows.
)



'''

# -*- coding: utf-8 -*-
"""
Created on Wed Jan  8 22:02:54 2025

@author: HP
"""

import cv2
import pyautogui
import mediapipe as mp
import time

# Initialize Hand tracking
mp_hands = mp.solutions.hands
hands = mp_hands.Hands()

# Set up the camera
cap = cv2.VideoCapture(0)

# Set the screen size explicitly
screen_width, screen_height = pyautogui.size()

# Initialize variables for mouse control
clicking_left = False
clicking_right = False

# Cooldown settings for multitasking view
cooldown_duration = 5  # in seconds
last_detection_time = time.time()

# Initialize variables for scrolling
thumb_ring_touching = False
ring_tip_previous_y = 0
scroll_speed = 100  # Initial scroll speed
alpha = 0.5  # Smoothing factor

while True:
    ret, frame = cap.read()
    if not ret:
        break

    # Convert the frame to RGB
    frame_rgb = cv2.cvtColor(frame, cv2.COLOR_BGR2RGB)

    # Process the frame with mediapipe
    results = hands.process(frame_rgb)

    if results.multi_hand_landmarks:
        for hand_landmarks in results.multi_hand_landmarks:
            # Adjusted X-coordinate to invert the direction
            x, y = int((1 - hand_landmarks.landmark[mp_hands.HandLandmark.MIDDLE_FINGER_TIP].x) * screen_width), \
                   int(hand_landmarks.landmark[mp_hands.HandLandmark.MIDDLE_FINGER_TIP].y * screen_height)

            # Move the cursor
            pyautogui.moveTo(x, y)

            # Check for left-clicking gesture (index finger touching middle finger)
            index_tip = hand_landmarks.landmark[mp_hands.HandLandmark.INDEX_FINGER_TIP]
            middle_tip = hand_landmarks.landmark[mp_hands.HandLandmark.MIDDLE_FINGER_TIP]

            distance_index_middle = ((index_tip.x - middle_tip.x)**2 + (index_tip.y - middle_tip.y)**2)**0.5
            index_and_middle_touching = distance_index_middle < 0.05  # Adjust this threshold as needed

            if index_and_middle_touching:
                if not clicking_left:
                    pyautogui.click()  # Simulate a left-click
                    clicking_left = True
            else:
                clicking_left = False

            # Check for right-clicking gesture (index finger and thumb touching)
            thumb_tip = hand_landmarks.landmark[mp_hands.HandLandmark.THUMB_TIP]
            distance_index_thumb = ((index_tip.x - thumb_tip.x)**2 + (index_tip.y - thumb_tip.y)**2)**0.5
            index_and_thumb_touching = distance_index_thumb < 0.05  # Adjust this threshold as needed

            if index_and_thumb_touching:
                if not clicking_right:
                    pyautogui.rightClick()  # Simulate a right-click
                    clicking_right = True
            else:
                clicking_right = False

            # Check if the hand is in a fist position for multitasking view
            if thumb_tip.y < index_tip.y:
                # Check cooldown
                if time.time() - last_detection_time > cooldown_duration:
                    # Simulate Win+Tab to open multitasking view
                    pyautogui.hotkey('winleft', 'tab')
                    last_detection_time = time.time()  # Update last detection time

            # Scrolling gesture
            thumb_tip = hand_landmarks.landmark[mp_hands.HandLandmark.THUMB_TIP]
            ring_tip = hand_landmarks.landmark[mp_hands.HandLandmark.RING_FINGER_TIP]

            # Calculate smoothed y-coordinate
            ring_tip_y = alpha * ring_tip_previous_y + (1 - alpha) * ring_tip.y

            # Adjusted X-coordinate to invert the direction
            x, y = int((1 - ring_tip.x) * screen_width), int(ring_tip_y * screen_height)

            # Check for scrolling gesture
            distance_thumb_ring = ((thumb_tip.x - ring_tip.x) ** 2 + (thumb_tip.y - ring_tip.y) ** 2) ** 0.5
            thumb_ring_touching = distance_thumb_ring < 0.05  # Adjust this threshold as needed

            # Get the current position of the cursor
            cursor_x, cursor_y = pyautogui.position()

            # Scroll up if thumb is touching ring finger and cursor is on the upside
            if thumb_ring_touching and cursor_y < screen_height / 2 and ring_tip.y < ring_tip_previous_y:
                pyautogui.scroll(scroll_speed)
            # Scroll down if thumb is touching ring finger and cursor is on the downside
            elif thumb_ring_touching and cursor_y >= screen_height / 2 and ring_tip.y > ring_tip_previous_y:
                pyautogui.scroll(-scroll_speed)

            # Save the current y-coordinate for the next iteration
            ring_tip_previous_y = ring_tip_y

    # Display the frame
    cv2.imshow('Hand Gesture Control', frame)

    if cv2.waitKey(1) & 0xFF == 27:  # Press 'Esc' to exit
        break

# Release the camera and close all windows
cap.release()
cv2.destroyAllWindows()


                
