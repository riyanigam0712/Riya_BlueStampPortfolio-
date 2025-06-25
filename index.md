# Smart Glasses

These smart glasses will be able use object recognition to TTS(text to speech) to help a user in different ways in the real world. --- Draft project goal is changing

| **Engineer** | **School** | **Area of Interest** | **Grade** |
|:--:|:--:|:--:|:--:|
| Riya N | Irvington High School | Electrical Engineering | Incoming Senior

<!---**Replace the BlueStamp logo below with an image of yourself and your completed project. Follow the guide [here](https://tomcam.github.io/least-github-pages/adding-images-github-pages-site.html) if you need help.**-->

![Headstone Image](RiyaN.jpeg)
  
# Final Milestone

<!--- <iframe width="560" height="315" src="https://www.youtube.com/embed/F7M7imOVGug" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe> -->

<!---For your final milestone, explain the outcome of your project. Key details to include are:
- What you've accomplished since your previous milestone
- What your biggest challenges and triumphs were at BSE
- A summary of key topics you learned about
- What you hope to learn in the future after everything you've learned at BSE -->



# Second Milestone - Object Recognition and Text-to-Speech Output

<iframe width="560" height="315" src="https://www.youtube.com/embed/tQPfRk3OY-E?si=J6-i2c21H-1Gcepx" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
In this milestone, I set up object recognition using a prebuilt MobileNet V2 library using Tensorflow and configured text-to-speech so the system can audibly announce its results.

1. Update the Raspberry Pi
First, open the terminal and run the following commands to update your Raspberry Pi and install essential tools:
```HTML 
sudo apt update
sudo apt upgrade -y
sudo apt install -y python3-pip - You might run in to some errors here, try removing the sudo command. Or try activating the environment first. 
sudo apt install --upgrade -y python3-setuptools
```

2. Set Up a Virtual Environment
A virtual environment (venv) is used to isolate project dependencies. This allows you to run different projects with conflicting package versions on the same system.
```HTML 
sudo apt install python3.11-venv
python -m venv env --system-site-packages
source env/bin/activate
source /home/riya/Documents/env/bin/activate - This will be different for you, check your path for your environment  
```

3. Install Raspi-Blinka
Raspi-Blinka is a Python library that bridges the gap between CircuitPython and standard Python, making it easier to use CircuitPython libraries on the Raspberry Pi. Although it’s not strictly required, it can be very helpful later on. 
Note: I had a lot of problems installing this library with the Wget command so I had go to the github and install every commmand manually.
```HTML 
cd ~
sudo pip3 install --upgrade adafruit-python-shell
Wget    https://raw.githubusercontent.com/adafruit/Raspberry-Pi-Installer-Scripts/master/raspi-blinka.py
sudo python3 raspi-blinka.py
```

Camera Testing -  Check if your camera is working properly by running:
 ```HTML 
libcamera-hello -t 0
```
if that does not work:
 ```HTML 
libcamera-hello --list-cameras : to check if camera is there
sudo apt upgrade libcamera-dev libcamera-apps
```
Speech Output - Festival is a text-to-speech (TTS) engine that will enable spoken output for recognized objects:
 ```HTML 
- sudo apt install -y festival
```

 Another command needed for Tensorflow  
 ```HTML 
sudo apt install -y python3-numpy python3-pillow python3-pygame
```
Install rpi-vision - This library can identify these objects on the link below: https://raw.githubusercontent.com/pytorch/hub/master/imagenet_classes.txt
 ```HTML 
cd ~
source env/bin/activate
git clone --depth 1 https://github.com/adafruit/rpi-vision.git if this gitbub does not work replace with https://github.com/anwesha-g/tensorflow_rpi_objdet
cd rpi-vision
pip3 install -e .
```

Install TensorFlow 2.x - use the command all at once
 ```HTML 
RELEASE=https://github.com/PINTO0309/Tensorflow-bin/releases/download/v2.15.0.post1/tensorflow-2.15.0.post1-cp311-none-linux_aarch64.whl
CPVER=$(python --version | grep -Eo '3\.[0-9]{1,2}' | tr -d '.')
pip install $(echo "$RELEASE" | sed -e "s/cp[0-9]\{3\}/CP$CPVER/g")
```
then use these commands in the terminal to start the program
 ```HTML 
cd rpi-vision
python3 tests/pitft_labeled_output.py --tflite
```
From this point, you can use either OBS or TigerVNC to view a camera feed that displays both the frame rate and the Raspberry Pi’s temperature. If you hold an object in front of the Pi Camera (common examples that worked for me were a mouse, a computer keyboard, and a cellular phone )it should be recognized by the system. You can also refer to the list I shared earlier for other objects it might detect. Overall, it works, but it can be a bit janky at times.

An extra thing for Speech Output that I did: 
Orginally I was using headphones with the headphone jack. If you want to test the speech output. Plug in your headphones or use bluetooth headphones and use:
 ```HTML 
echo "This is a test" | festival --tts
```
For me the volume was really low when I was testing with the command above. I had to use a another command called alsamixer. Put the word alsamixer in the terminal. A menu will come up to adjust the volume. 

In my demo video you might have seen that I used a USB speaker instead of headphones. This was becuase I wanted to display to everyone the speech output. To do this plug the Bluetooth speaker into the Raspberry Pi and use the command:
 ```HTML
sudo raspi-config
```
This command is helpful for many things in general like debugging and you will see a huge menu with many features. For our purpose, go to audio and switch the output from headphone jack to the USB speaker. Then you can run the test and check it out. Now when you put objects in front of the camera the audio will come out of the speaker. You can still adjust audio volume with alsamixer. 
# First Milestone


<iframe width="560" height="315" src="https://www.youtube.com/embed/bZwton4v0_g?si=MIdvZNEX-xEZbMlO" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

My project that is the smart glasses I am using a rasberry pi and it's designated camera to perform object regontiiton and then attaching on to the glasses. For my milestone I Set up and connected to the Raspberry Pi to take a picture.

This entailed: 

Inserting the Micro-SD card into the USB reader and plug it into your computer

Do the imaging for the sd card which means duplicate their configured Raspberry Pi setups, back up their systems, or quickly deploy the same OS and software on multiple devices

Enable (SSH (Secure Shell) which lets you remotely control your Raspberry Pi from another computer without needing a monitor, keyboard, or mouse connected to the Pi) 

Run terminal commands from my PC

Edit code via VS Code

Transfer files between Pi and my computer

Insert the SD card into the Raspberry Pi., Put Raspberry Pi into the case

Monitor Setup via OBS and Connect the Raspberry Pi to your PC using the HDMI capture card

enable SSH and VNC and then change port number for SSH

VS Code and open it, Install the Remote - SSH extension
```python
from picamera2 import Picamera2, Preview
import time
import cv2
picam2 = Picamera2()
camera_config = picam2.create_still_configuration(main={"size": (1920, 1080)},
lores={"size": (640, 480)}, display="lores")
picam2.configure(camera_config)
#picam2.start_preview(Preview.QTGL) #Comment this out if not using desktop interface
picam2.start()
time.sleep(2)
im = picam2.capture_array()
im = cv2.cvtColor(im, cv2.COLOR_BGR2RGB)
cv2.imwrite('file.png', im)void setup() {
```
Connect the camera and use python code above to take a picture. 

I encountered a lot of challenges for my first milestone which included the VS code side bar not working, and the imaging not working on my computer. 
My next steps ...


# Schematics 
<!---Here's where you'll put images of your schematics. [Tinkercad](https://www.tinkercad.com/blog/official-guide-to-tinkercad-circuits) and [Fritzing](https://fritzing.org/learning/) are both great resoruces to create professional schematic diagrams, though BSE recommends Tinkercad becuase it can be done easily and for free in the browser. -->

<!---# Code
Here's where you'll put your code. The syntax below places it into a block of code. Follow the guide [here]([url](https://www.markdownguide.org/extended-syntax/)) to learn how to customize it to your project needs. 

```c++
void setup() {
  // put your setup code here, to run once:
  Serial.begin(9600);
  Serial.println("Hello World!");
}

void loop() {
  // put your main code here, to run repeatedly:

}
```-->

# Bill of Materials
<!---Here's where you'll list the parts in your project. To add more rows, just copy and paste the example rows below.
Don't forget to place the link of where to buy each component inside the quotation marks in the corresponding row after href =. Follow the guide [here]([url](https://www.markdownguide.org/extended-syntax/)) to learn how to customize this to your project needs. -->

| **Part** | **Note** | **Price** | **Link** |
|:--:|:--:|:--:|:--:|
| Raspberry Pi 4 | Allows the interaction between the software and hardware for the camera and audio system | $58.99 | <a href="https://www.amazon.com/dp/B0CMZST24Y?ref=cm_sw_r_cso_cp_apin_dp_Y63HAE5CHH0DGMEAV253_1&ref_=cm_sw_r_cso_cp_apin_dp_Y63HAE5CHH0DGMEAV253_1&social_share=cm_sw_r_cso_cp_apin_dp_Y63HAE5CHH0DGMEAV253_1&starsLeft=1"> Link </a> |
| Camera Module | Taking videos for the object recognition | $14.49 | <a href="https://www.amazon.com/dp/B01ER2SKFS?ref=cm_sw_r_cso_cp_apin_dp_1CTZAM6W55YVQMMQNVET&ref_=cm_sw_r_cso_cp_apin_dp_1CTZAM6W55YVQMMQNVET&social_share=cm_sw_r_cso_cp_apin_dp_1CTZAM6W55YVQMMQNVET&starsLeft=1"> Link </a> |
| Ear Phones | Allows user to hear the audio in ear | $10.95 | <a href="[https://www.amazon.com/dp/B07YZ6LLDH?ref=cm_sw_r_cso_cp_apin_dp_04A8H7YD6WEPZYG52YZK&ref_=cm_sw_r_cso_cp_apin_dp_04A8H7YD6WEPZYG52YZK&social_share=cm_sw_r_cso_cp_apin_dp_04A8H7YD6WEPZYG52YZK&starsLeft=1](https://www.amazon.com/Antool-Earbuds-Headphones-Earphones-Compatible/dp/B0D17TFVTF?th=1)"> Link </a> |
| Glasses | Allows user to wear the device | $5.99 | <a href="https://www.amazon.com/dp/B0BSF4PL2Q?ref=cm_sw_r_cso_cp_apin_dp_9G1KS2KBDABJZ2GR6485&ref_=cm_sw_r_cso_cp_apin_dp_9G1KS2KBDABJZ2GR6485&social_share=cm_sw_r_cso_cp_apin_dp_9G1KS2KBDABJZ2GR6485&starsLeft=1"> Link </a> |
| Wires | Enables everything to be connected to allow information to flow | $6.98 | <a href="https://www.amazon.com/dp/B01EV70C78?ref=cm_sw_r_cso_cp_apin_dp_CCVPR26GTMPF9EXHBMFR&ref_=cm_sw_r_cso_cp_apin_dp_CCVPR26GTMPF9EXHBMFR&social_share=cm_sw_r_cso_cp_apin_dp_CCVPR26GTMPF9EXHBMFR&starsLeft=1"> Link </a> |


# Other Resources/Examples
- [Helpful Slides for the Raspberry Pi setup](https://trashytuber.github.io/YimingJiaBlueStamp/](https://docs.google.com/presentation/d/1YA3rk9UH5X98TfvAGCCi-I7g4gzihHQl/edit?slide=id.p1#slide=id.p1))
- [Raspberry Pi + Teachable Machine = Teachable Pi](https://learn.adafruit.com/teachable-machine-raspberry-pi-tensorflow-camera/use-raspberry-pi-camera/)
- [Running TensorFlow Lite Object Recognition on the Raspberry Pi 4 or Pi 5](https://learn.adafruit.com/running-tensorflow-lite-on-the-raspberry-pi-4)
- [Teachable Machine](https://trashytuber.github.io/YimingJiaBlueStamp/](https://docs.google.com/presentation/d/1YA3rk9UH5X98TfvAGCCi-I7g4gzihHQl/edit?slide=id.p1#slide=id.p1)](https://teachablemachine.withgoogle.com/))

<!---To watch the BSE tutorial on how to create a portfolio, click here.-->

# Starter Project

<iframe width="560" height="315" src="https://www.youtube.com/embed/_SmJnlM9aK0?si=IGL8JqboFfbL3MUj" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

The starter project I worked on was the Retro Arcade Console. It was a soldering-heavy project with many interesting steps and components.

First, I soldered the USB socket (to serve as the power source) and the dot-matrix display (for the screen). After that, I attached other essential components, including the capacitors, the buzzer, and the power switch.

Next, I added the keys, timers, and a battery option to provide an alternative power source. The console came preloaded with many classic games, such as Tetris and Space Shooter, making it a fun and rewarding project despite its small size and simple design.

![Headstone Image](IMG_9601.jpeg)
