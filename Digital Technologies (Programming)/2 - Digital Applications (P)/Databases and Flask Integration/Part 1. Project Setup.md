# Part 1. Project Setup

Ok, so there are a few things to do here, let's list them first, and then knock them down:
1. Project Folder and files
2. Database Creation
3. Template Inheritance Setup
4. Starting app.py file
## 1. Project Folder and Files
Create a new folder in your file system, and open it in VS Code.
Let's make sure you have everything you need in your project folder! Here's my essentials to begin work. Ignore the DB file in there for now, we're going to make that in step 2!

![[folder_setup.png|354]]

To create the virtual environment (the .venv folder), do Ctrl + Shift + P and locate the command:
![[create_venv.png]]
## 2. Database Creation
Jump into DB Browser, create a new database file inside your Project Folder
Some notes:
- I have just used lowercase names for everything, so try to follow that, it will make any code you borrow from my solution just a little more straighforward.
### User table
![[table_user.png]]

### Machine table
![[table_machine.png]]

### Job table
**Don't forget to assign the Foreign Keys!**
![[table_job.png]]

### Models.py file
Before we go any further, let's fill out our `models.py` file using our friend SQLACodegen.
Make sure that the final argument you're sending matches what you have named your database file! You can see in the screenshot above, and the command below, mine is called `job_logger.db`

I've also got you installing Flask and Flask-Sqlalchemy at the same time!

```bash
pip install flask-sqlacodegen flask flask-sqlalchemy
flask-sqlacodegen --flask --outfile models.py sqlite:///job_logger.db
```

Just for reference, this is my models.py file result:
```python
# coding: utf-8
from flask_sqlalchemy import SQLAlchemy

db = SQLAlchemy()

class Job(db.Model):
    __tablename__ = 'job'

    id = db.Column(db.Integer, primary_key=True, unique=True)
    user_id = db.Column(db.ForeignKey('user.id'), nullable=False)
    machine_id = db.Column(db.ForeignKey('machine.id'), nullable=False)
    description = db.Column(db.Text)
    material = db.Column(db.Text)
    date_created = db.Column(db.Text)
    date_required = db.Column(db.Text)
    complete = db.Column(db.Boolean)

    machine = db.relationship('Machine', primaryjoin='Job.machine_id == Machine.id', backref='jobs')
    user = db.relationship('User', primaryjoin='Job.user_id == User.id', backref='jobs')

class Machine(db.Model):
    __tablename__ = 'machine'

    id = db.Column(db.Integer, primary_key=True, unique=True)
    name = db.Column(db.Text, nullable=False)
    type = db.Column(db.Text)
    model = db.Column(db.Text)
    dimensions = db.Column(db.Text)
    status = db.Column(db.Text)

class User(db.Model):
    __tablename__ = 'user'

    id = db.Column(db.Integer, primary_key=True, unique=True)
    name = db.Column(db.Text, nullable=False)
    student_id = db.Column(db.Text)
    email = db.Column(db.Text)

```


## 3. Template Inheritance Setup
During our project breakdown session, we worked out that there might be as many as eight individual webpages. We've looked briefly at Template Inheritance before, and in a project of this scale, it is DEFINITELY going to be worth implementing.

Now the navigation bar example that I'm providing here is from [W3 Schools](https://www.w3schools.com/howto/howto_js_topnav.asp), so no credit to me! I actually removed a little bit of functionality, if you're curious, check out the original through that link!
### base.html
```html
<!DOCTYPE html>
<html>

<head>
    <link rel="stylesheet" href="{{ url_for('static', filename='styles.css') }}">
    <title>Website Name - {% block title %}{% endblock %}</title>
</head>

<body>

    <div class="topnav">
        <a href="#home">Home</a>
        <a href="#new_job">New Job</a>
        <a href="#new_user">New User</a>
        <a href="#about">About</a>
    </div>

    <div style="padding-left:16px">
        {% block content %}{% endblock %}
    </div>


    {% block scripts %}

    {% endblock %}
</body>

</html>
```

## index.html (and starting point for all others!)
```html
{% extends "base.html" %}

{% block title %} Home {% endblock %}


{% block content %}
<h1>RoJoR</h1>
<h2>Welcome to the Robotics Job Registry</h2>
{% endblock %}
```

## styles.css
```css
body {
    margin: 0;
    font-family: Arial, Helvetica, sans-serif;
}

.topnav {
    overflow: hidden;
    background-color: #333;
}

.topnav a {
    float: left;
    color: #f2f2f2;
    text-align: center;
    padding: 14px 16px;
    text-decoration: none;
    font-size: 17px;
}
```


For context, let's see what out index.html file looks like!
![[starting_ui.png]]

## 4. Starting app.py file
```python
from flask import Flask, render_template
import os
from models import db, User, Machine, Job

folder_path = os.path.dirname(os.path.abspath(__file__))

app = Flask(__name__)
app.config["SQLALCHEMY_DATABASE_URI"] = "sqlite:///" + folder_path + "/job_logger.db"
db.init_app(app)

@app.route("/")
def index():
    return render_template("index.html")

if __name__ == '__main__':
    with app.app_context():
        db.create_all()
    app.run(debug = True)
```

You can see here, we're just displaying the index page so far, and connecting our database essentials together!