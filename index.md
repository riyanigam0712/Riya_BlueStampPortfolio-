# Resistor Detector Using Picam

When I was working on an engineering project in high school that involved a lot of electronic components and breadboards, I needed to use four resistors. My teacher told me to check the cabinets at the back of the classroom, but when I opened them—oh my god, it was a complete mess. Everything had spilled out, and resistors were scattered everywhere. Many of them were mislabeled or placed in the wrong packets. It was really stressful trying to find the correct resistors and having to use an online calculator to guess their values based on their color bands.

Originally, my project was focused on building smart glasses, but for modifications, I decided to shift gears. I wanted to create something more practical and helpful—so I started working on a smart resistor detector using a Raspberry Pi camera.


| **Engineer** | **School** | **Area of Interest** | **Grade** |
|:--:|:--:|:--:|:--:|
| Riya N | Irvington High School | Electrical Engineering | Incoming Senior

<!---**Replace the BlueStamp logo below with an image of yourself and your completed project. Follow the guide [here](https://tomcam.github.io/least-github-pages/adding-images-github-pages-site.html) if you need help.**-->

![Headstone Image](RiyaN.jpeg)
  
# Final Milestone

My final Milestone is making the glasses look cool and being able identify different types of resistors.
<iframe width="560" height="315" src="https://www.youtube.com/embed/R_G0wD6l74A?si=lyi4LIUGegrYVNie" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

For my third milestone, I decided to move away from working with the glasses and instead build something more hands-on and meaningful: a resistor detector. Here’s a step-by-step breakdown of the entire process and how I got it working.

1. Project Shift & Initial Research
I initially planned to collect my own dataset of resistor images but quickly realized this was too time-consuming:

I needed hundreds of images per resistor.

I’d have to take them in various environments with proper focus and lighting.

My  phone camera wasn’t ideal for the job.

So instead, I started researching public datasets. I explored Kaggle and Roboflow.

And eventually found a useful dataset from a site from Roboflow.

2. Dataset Structure & Sorting
The Roboflow dataset contained:

Image files of resistors.(jpeg) in 3 different folders, 

Corresponding TXT files with annotations.

Each TXT file had five numbers:

The first number (1–48) represented the resistor’s class label.

This number matched a class name.

However, everything was unsorted. So I wrote a Python script that:

Read the first number from each TXT file.

Mapped it to the corresponding image.

Organized everything into a folder called group_all, with subfolders named 1 to 48, each containing images for that class.

These are the two codes if you use the same dataset.
```python 
import os
import shutil

def sort_images_by_label(base_dir):
    images_dir = os.path.join(base_dir, "images")
    labels_dir = os.path.join(base_dir, "labels")

    for label_file in os.listdir(labels_dir):
        if label_file.endswith(".txt"):
            label_path = os.path.join(labels_dir, label_file)

            # Read the first number from the label file
            with open(label_path, 'r') as f:
                content = f.read().strip()
                if not content:
                    continue
                first_token = content.split()[0]
                if not first_token.isdigit():
                    print(f"Skipped {label_file}: first token is not a digit.")
                    continue

                label_class = first_token
                # Create destination folder under images
                target_dir = os.path.join(images_dir, label_class)
                os.makedirs(target_dir, exist_ok=True)

                # Find corresponding image (assumes .jpg)
                image_name = os.path.splitext(label_file)[0] + ".jpg"
                image_path = os.path.join(images_dir, image_name)

                if os.path.exists(image_path):
                    dest_path = os.path.join(target_dir, image_name)
                    shutil.move(image_path, dest_path)
                    print(f"Moved {image_name} → {target_dir}")
                else:
                    print(f"Image not found for label file: {label_file}")

# These are your data folders relative to where this script is located
base_folders = ['train', 'test', 'valid']

for folder in base_folders:
    if os.path.exists(folder):
        sort_images_by_label(folder)
    else:
        print(f"Folder not found: {folder}")

```
and
 ```python 
import os
import shutil

def group_all_labeled_images(source_sets, output_dir):
    os.makedirs(output_dir, exist_ok=True)

    for base_set in source_sets:
        images_root = os.path.join(base_set, "images")
        if not os.path.exists(images_root):
            print(f"Skipping {base_set}: no images folder found.")
            continue

        for label_class in os.listdir(images_root):
            class_dir = os.path.join(images_root, label_class)
            if not os.path.isdir(class_dir):
                continue  # Skip non-folder files

            target_dir = os.path.join(output_dir, label_class)
            os.makedirs(target_dir, exist_ok=True)

            for file in os.listdir(class_dir):
                if file.lower().endswith((".jpg", ".jpeg", ".png")):
                    src_path = os.path.join(class_dir, file)
                    dest_path = os.path.join(target_dir, file)

                    # Avoid overwriting same-named files
                    if os.path.exists(dest_path):
                        base, ext = os.path.splitext(file)
                        count = 1
                        while os.path.exists(os.path.join(target_dir, f"{base}_{count}{ext}")):
                            count += 1
                        dest_path = os.path.join(target_dir, f"{base}_{count}{ext}")

                    shutil.copy(src_path, dest_path)
                    print(f"Copied {src_path} → {dest_path}")

# Run the grouping after sorting
group_all_labeled_images(["train", "test", "valid"], "grouped_all")
```
I wanted to try Teachable Machine for live detection using a webcam.

Steps I took:

Selected only 10 resistor classes to avoid crashes and long training times.

Dragged sorted images into Teachable Machine.

Trained a model.

Exported it and used SCP (Secure Copy) to transfer the model to my Raspberry Pi.
(there are whole tutorials on this)

Used the Pi Camera to run a live feed.

End result:

The live feed showed predictions with two red numbers: the class and resistor name.

It worked  for the images from the training set.

Real-world detection didn’t work well—especially for resistors at BlueStamp (which were blue), since I hadn’t trained on those.

Here is the code I used to run the live feed.
```python 
WILL add
```
Teachable Machine is better for classification, not object detection. I wanted something that could:

Detect resistors but Draw bounding boxes around them.

Show labels and class names.

That’s when I explored YOLOv5 (You Only Look Once), a powerful object detection framework.

I needed to train YOLOv5 using:

Labeled images

A weights file (e.g., best.pt), which stores the trained model.

Here’s what I did:

Went back to the Roboflow dataset.

Used Google Colab to train the YOLOv5 model:

At first, I tried training on all 48 classes and 14,000+ images for 50 epochs—huge mistake.

Google Colab kept disconnecting and shutting down due to limits.

I even used an auto-clicker to keep the session alive.

To make it work, I:

Reduced to 5 classes.

Trained for just 3–4 epochs.

Edited the data.yaml file - key step

This reduced time and worked better with Colab's limits.

After training, I got:

A best.pt file (weights).

Transferred it to my Raspberry Pi using SCP.

Running YOLOv5 Detection on the Raspberry Pi

To make YOLOv5 work on the Pi:

I modified YOLOv5’s detect.py script.

Lowered the confidence threshold to 0.04 because I wanted it to draw bounding boxes even on uncertain predictions.

Edited the data.yaml file to include the right paths and class names.

Finally, ran the detection code using a custom command.

Result:

The live detection worked!

It drew boxes around resistors and labeled them with class names.(sadly only 5 classes)

Tested it on some images from the training set, and it was accurate and exciting to see!

I now have two working resistor detection pipelines:

Teachable Machine for basic classification (used with a PiCam).

YOLOv5 for real-time object detection with bounding boxes and labels.




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
# First Milestone - Configure Raspberry Pi and Take a picture!


<iframe width="560" height="315" src="https://www.youtube.com/embed/bZwton4v0_g?si=MIdvZNEX-xEZbMlO" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

My project involves creating smart glasses using a Raspberry Pi and its dedicated camera to perform object recognition. The camera will be attached to the glasses. As a milestone, I have set up and connected the camera to the Raspberry Pi and successfully captured a picture.

This entailed: 

Inserting the Micro-SD card into the USB reader and plug it into your computer.

Do the imaging for the sd card. This means duplicate the configured Raspberry Pi setups, back up their systems, or quickly deploy the same OS and software on multiple devices.

Enable (SSH/Secure Shell) which lets you remotely control your Raspberry Pi from another computer without needing a monitor, keyboard, or mouse connected to the Pi) 

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
My next steps are to get object recognition on a live feed with text to speech output. 


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
