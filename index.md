# Smart Glasses
pair of glasses that do object recognition 
You should comment out all portions of your portfolio that you have not completed yet, as well as any instructions:
```HTML 
<!--- This is an HTML comment in Markdown -->
<!--- Anything between these symbols will not render on the published site -->
```

| **Engineer** | **School** | **Area of Interest** | **Grade** |
|:--:|:--:|:--:|:--:|
| Riya N | Irvington High School | Electrical Engineering | Incoming Senior

**Replace the BlueStamp logo below with an image of yourself and your completed project. Follow the guide [here](https://tomcam.github.io/least-github-pages/adding-images-github-pages-site.html) if you need help.**

![Headstone Image](RiyaN.jpeg)
  
# Final Milestone

<!--- <iframe width="560" height="315" src="https://www.youtube.com/embed/F7M7imOVGug" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe> -->

<!---For your final milestone, explain the outcome of your project. Key details to include are:
- What you've accomplished since your previous milestone
- What your biggest challenges and triumphs were at BSE
- A summary of key topics you learned about
- What you hope to learn in the future after everything you've learned at BSE -->



# Second Milestone


<iframe width="560" height="315" src="https://www.youtube.com/embed/tQPfRk3OY-E?si=J6-i2c21H-1Gcepx" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
My Second Milestone was to get the object recognition to work with a premade library from Mobile new V2 and make sure the text to speech would produce an audio of what was recognized. 

First I had to in the terminal:  
**Update the Raspberry Pi
sudo apt update
sudo apt upgrade -y
sudo apt install -y python3-pip
sudo apt install --upgrade -y python3-setuptools

Then I had to 
Setup Virtual Environment - venv is to protect your system, and to have multiple incompatible projects running on the same system without any issues. Each project for example requires a different package version, so you have a venv for each project. You can experiment with different packages without venv. 
sudo apt install python3.11-venv
python -m venv env --system-site-packages
source env/bin/activate
source /home/riya/Documents/env/bin/activate

Upgrade Script - Raspi-Blinka is a Python library that bridges the gap between CircuitPython and standard Python on Single Board Computers (SBCs) like the Raspberry Pi. It allows you to use CircuitPython libraries within a standard Python environment. It not completley needed but make thing much easier later on. I had a lot of problems installing this library with the Wget command so I had go to the github nd install everything manually. 
cd ~
sudo pip3 install --upgrade adafruit-python-shell
Wget    https://raw.githubusercontent.com/adafruit/Raspberry-Pi-Installer-Scripts/master/raspi-blinka.py
sudo python3 raspi-blinka.py

Camera Testing -  WHen you put this command in the terminal, the cmaera live feed should activate. 
libcamera-hello -t 0

Speech Output - Fesival is a libary that will enable the text to speech conversion
- sudo apt install -y festival

Install rpi-vision - This libary has
https://raw.githubusercontent.com/pytorch/hub/master/imagenet_classes.txt
TensorFlow 2.x
cd rpi-vision
python3 tests/pitft_labeled_output.py --tflite

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
Here's where you'll put images of your schematics. [Tinkercad](https://www.tinkercad.com/blog/official-guide-to-tinkercad-circuits) and [Fritzing](https://fritzing.org/learning/) are both great resoruces to create professional schematic diagrams, though BSE recommends Tinkercad becuase it can be done easily and for free in the browser. 

# Code
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
```

# Bill of Materials
Here's where you'll list the parts in your project. To add more rows, just copy and paste the example rows below.
Don't forget to place the link of where to buy each component inside the quotation marks in the corresponding row after href =. Follow the guide [here]([url](https://www.markdownguide.org/extended-syntax/)) to learn how to customize this to your project needs. 

| **Part** | **Note** | **Price** | **Link** |
|:--:|:--:|:--:|:--:|
| Raspberry Pi 4 | Allows the interaction between the software and hardware for the camera and audio system | $58.99 | <a href="https://www.amazon.com/dp/B0CMZST24Y?ref=cm_sw_r_cso_cp_apin_dp_Y63HAE5CHH0DGMEAV253_1&ref_=cm_sw_r_cso_cp_apin_dp_Y63HAE5CHH0DGMEAV253_1&social_share=cm_sw_r_cso_cp_apin_dp_Y63HAE5CHH0DGMEAV253_1&starsLeft=1"> Link </a> |
| Camera Module | Taking videos for the object recognition | $14.49 | <a href="https://www.amazon.com/dp/B01ER2SKFS?ref=cm_sw_r_cso_cp_apin_dp_1CTZAM6W55YVQMMQNVET&ref_=cm_sw_r_cso_cp_apin_dp_1CTZAM6W55YVQMMQNVET&social_share=cm_sw_r_cso_cp_apin_dp_1CTZAM6W55YVQMMQNVET&starsLeft=1"> Link </a> |
| Ear Phones | Allows user to hear the audio in ear | $10.95 | <a href="[https://www.amazon.com/dp/B07YZ6LLDH?ref=cm_sw_r_cso_cp_apin_dp_04A8H7YD6WEPZYG52YZK&ref_=cm_sw_r_cso_cp_apin_dp_04A8H7YD6WEPZYG52YZK&social_share=cm_sw_r_cso_cp_apin_dp_04A8H7YD6WEPZYG52YZK&starsLeft=1](https://www.amazon.com/Antool-Earbuds-Headphones-Earphones-Compatible/dp/B0D17TFVTF?th=1)"> Link </a> |
| Glasses | Allows user to wear the device | $5.99 | <a href="https://www.amazon.com/dp/B0BSF4PL2Q?ref=cm_sw_r_cso_cp_apin_dp_9G1KS2KBDABJZ2GR6485&ref_=cm_sw_r_cso_cp_apin_dp_9G1KS2KBDABJZ2GR6485&social_share=cm_sw_r_cso_cp_apin_dp_9G1KS2KBDABJZ2GR6485&starsLeft=1"> Link </a> |
| Wires | Enables everything to be connected to allow information to flow | $6.98 | <a href="https://www.amazon.com/dp/B01EV70C78?ref=cm_sw_r_cso_cp_apin_dp_CCVPR26GTMPF9EXHBMFR&ref_=cm_sw_r_cso_cp_apin_dp_CCVPR26GTMPF9EXHBMFR&social_share=cm_sw_r_cso_cp_apin_dp_CCVPR26GTMPF9EXHBMFR&starsLeft=1"> Link </a> |


# Other Resources/Examples
- [Helpful Slides for the Raspberry Pi](https://trashytuber.github.io/YimingJiaBlueStamp/](https://docs.google.com/presentation/d/1YA3rk9UH5X98TfvAGCCi-I7g4gzihHQl/edit?slide=id.p1#slide=id.p1))
- [Raspberry Pi + Teachable Machine = Teachable Pi](https://learn.adafruit.com/teachable-machine-raspberry-pi-tensorflow-camera/use-raspberry-pi-camera/)
- [Running TensorFlow Lite Object Recognition on the Raspberry Pi 4 or Pi 5](https://learn.adafruit.com/running-tensorflow-lite-on-the-raspberry-pi-4)
- [Teachable Machine](https://trashytuber.github.io/YimingJiaBlueStamp/](https://docs.google.com/presentation/d/1YA3rk9UH5X98TfvAGCCi-I7g4gzihHQl/edit?slide=id.p1#slide=id.p1)](https://teachablemachine.withgoogle.com/))

To watch the BSE tutorial on how to create a portfolio, click here.

# Starter Project

<iframe width="560" height="315" src="https://www.youtube.com/embed/_SmJnlM9aK0?si=IGL8JqboFfbL3MUj" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

The starter project I did was the Retro Arcade Console. It was a soldering heavy project that had many interesting steps and components. First I attached the USB socket (Serves as the power source) and the Dot matrix (the screen) with soldering. Then I attached many other important components like the capacitors, buzzer and power switch. After, I attached the keys and the timers and also added a battery option for power. The console has many classic games like tetris and spaceshooter and it was pretty fun for such a small console and simple project. 

![Headstone Image](IMG_9601.jpeg)
