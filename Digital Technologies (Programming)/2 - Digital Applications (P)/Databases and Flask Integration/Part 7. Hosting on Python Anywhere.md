# Part 7. Hosting on Python Anywhere

In this guide we are going to be hosting on websites online using Python Anywhere so that we can use our website online from any device!
## 1. Signing up for Python Anywhere
Before we start, we need to create an account on Python Anywhere. They have free limited accounts, so sign up [here](https://www.pythonanywhere.com/registration/register/beginner/), the limited account restricts us to have the URL of our site be "our username".pythonanywhere.com but if we create our username with this in mind it isn't an issue. For this project set your username to be "your_student_number" (eg. if your student number was 1234567 your username would be 1234567). You will be required to confirm your email so use your student email.
## 2. Uploading Your Project Files
- In your file explorer on your computer open the folder that contains your project. Then select all the files and folders **except** for .venv and \_\_pycache\_\_ and zip them. 
![[Files to Zip.png]]
>[!Note] Any entries in your database will be carried over, if you want to remove entries in your database then do that before zipping your project.
- Open the Python Anywhere Dashboard and click on Browse Files:
![[Python Anywhere Dashboard.png|593]]
The Browse Files page will look like this:
![[Browse Files.png|596]]
- Click on "Upload a file" and upload your zip. 
- Then click on "Open Bash console here", it will open a new bash console, type the command 
```bash
unzip "your-zip-file.zip"
``` 
>[!Note] replace your-zip-file with whatever you named your project zip file:

- Press [Enter] to run the command and you should see the files in your project being unzipped:
![[Bash Output.png|379]]
## 3. Setting Up the Web App
- Go to the Web tab on Python Anywhere and click on "Add a new web app":
![[Add New Web App.png|606]]
- Click "Next", since we are using the free version we don't have to worry about the domain name since it will just match our username.
- On the next page it will ask you to "Select a Python Web framework", select Manual Configuration, **DO NOT SELECT FLASK**.
- Next select your python version, you can check the version you are using in Visual Studio Code by using the command `python --version`. Alternatively, you can just select python version 3.13.
- This should set up your web app base
## 4. Linking the Source Code
- On the Web tab, scroll until you see the "Code" section:
![[Code Section of Web page.png|640]]
Right now it shows the path to our source code is empty.
- Click on "Enter the path to your web app source code" and type in the path to your uploaded files, this will likely be "/home/yourusername" but you can double check by going to the Files tab, opening the folder with your app.py file and copying the path from the top left
![[Copy Path to Source Code.png|627]]
- Your source code path should now be set:
![[Source Code Set.png|612]]
## 5. Configure WSGI
- In the "Code" section of the Web tab click on the WSGI configuration file to open it, it will look something like this (I have scrolled down a bit in the file for this screenshot):
![[WSGI File Unedited.png|614]]
- Delete all of the file contents and replace it with the following Python code, make sure that the path is the same path to your source code that you used in section 4.
```python 
import sys, os

path = '/home/RoJoRJobLoggingSiteTest/' #Edit this to be the path to your source code
if path not in sys.path:
    sys.path.append(path)

from app import app as application
```
- The WSGI file will just contain this:
![[WSGI File Edited Proper.png|668]]
- Save the file
## 6. Setup Virtual Environment
- Go to the Consoles tab and open a Bash console
- In the Bash console use the command `mkvirtualenv -p python3.13 my-venv-name` (replace my-venv-name with whatever you want your virtual environment, and if you used a version other than 3.13 then replace 3.13 with the version you used )
![[Virtual Env Command.png|442]]
- Go to the Files tab, you will now see a directory called .virtualenvs/ 
- Open this folder, and open the newly created venv folder
- Copy the path to this folder in the top left as seen below:
![[Copy venv path.png|608]]
- Go to the Web tab
- Scroll down to the Venv section
![[Venv Section.png|584]]
- Click on "Enter path to a virtualenv, if desired" and paste the path to the Venv folder you created
- Click on "Start a console in this virutalenv"
![[Start a console in this virtualenv.png]]
- In this console, pip install all the packages you needed for the project. For our purposes, unless you added more dependencies, this will likely just be:
```bash
pip install flask-sqlacodegen flask flask-sqlalchemy
```
>[!SUCCESS] Your website should now be set up, at the top of the Web tab you can click on the link to your site to open it.
