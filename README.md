nrf24pihub
==========

A simple solution to set up a raspberry pi hub for nrf24l01 based wireless sensors/ micro controller projects

It makes use of pynrf (by Joao Paulo Barraca)
python code for spidev
and GPIO drivers in python

Details on setting it up can be read at http://blog.riyas.org

1. Getting the tools

 # get the necessary packages  
 sudo apt-get install python-dev  
 #needed to install the python modules  
 sudo apt-get install build-essential 
 #install git for easy code access
 sudo apt-get install git-core 

2. Installing the modules


git clone https://github.com/riyas-org/nrf24pihub

cd nrf24pihub

cd python-spi

sudo python setup.py install

cd ../RPi.GPIO-0.5.6

sudo python setup.py install

cd ..


Start the application

sudo python raspberryfriend.py

===========================================================
Pyton code


#!/usr/bin/python
# raspberry pi nrf24l01 hub
# more details at http://blog.riyas.org
# Credits to python port of nrf24l01, Joao Paulo Barrac & maniacbugs original c library

from nrf24 import NRF24
import time
from time import gmtime, strftime

pipes = [[0xf0, 0xf0, 0xf0, 0xf0, 0xe1], [0xf0, 0xf0, 0xf0, 0xf0, 0xd2]]

radio = NRF24()
radio.begin(0, 0,25,18) #set gpio 25 as CE pin
radio.setRetries(15,15)
radio.setPayloadSize(32)
radio.setChannel(0x4c)
radio.setDataRate(NRF24.BR_250KBPS)
radio.setPALevel(NRF24.PA_MAX)
radio.setAutoAck(1)
radio.openWritingPipe(pipes[0])
radio.openReadingPipe(1, pipes[1])

radio.startListening()
radio.stopListening()

radio.printDetails()
radio.startListening()

while True:
    pipe = [0]
    while not radio.available(pipe, True):
        time.sleep(1000/1000000.0)
    recv_buffer = []
    radio.read(recv_buffer)
    out = ''.join(chr(i) for i in recv_buffer)
    print out
    
    

