# FinalYearProject2024BE
BE final year, technology used - python, media pipe, pyautogui, OS and OpenCV python libraries  
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

                
