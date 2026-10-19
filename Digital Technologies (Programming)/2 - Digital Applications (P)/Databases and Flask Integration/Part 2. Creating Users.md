# Part 2. Creating Users

We're going to need to allow people to create a new user for themselves in order to use the system, so let's do that first!

A couple notes before we begin:
- An ideal version of this would involve complete user authentication with a password. This is a great idea, but it intorudces a bit of complexity that I don't think we have the in-class time to cover right now. It would require
    - Password Hashing (Storing passwords safely and securely)
    - Session Management
    - Log in and Log out functions
    - Possibly implementing the whole Flask-Login package (very useful, but a bit to understand)
- We're just going to limit it to this kind of workflow:
    - Users are created
    - When creating a job, you pick a user from a drop-down box to assign it to that user.

## 1. Setting Up
Let's do a couple things to begin with!
- Create the HTML file for the template
- Create the route and function
- Create a navigation link to the new route in the base.html

### Create the HTML file
This one's easy!
Make a new html file inside your `templates` folder, call it `create_user.html`
Fill it with this blank template inheritance code
```html
{% extends "base.html" %}

{% block title %} Create New User {% endblock %}

{% block content %}
<h1>Create New User</h1>
{% endblock %}
```

### Create the route and function
We're just going to set up a new function for ourselves, we'll fill in all the details shortly.
I'm going tto make sure that this route has both the GET and POST methods enabled, since it's going to be used for a **form**

```python
@app.route("/create_user", methods=["GET", "POST"])
def create_user():
    return render_template("create_user.html")
```

Make sure you add this function *directly below your index function*

### Add the link the Base.html's navigation bar
This is well worth doing now to make your life easier while testing! Head over to your `base.html` and update your navigation bar to look like this:

```html
<div class="topnav">
	<a href="{{url_for('index')}}">Home</a>
	<a href="#new_job">New Job</a>
	<a href="{{url_for('create_user')}}">New User</a>
</div>
```

Now, from any page of your website you should be able to access the Home/Index page and the create_user page!
![[create_user_route_setup.gif]]
You should probably run your app and make sure you can easily navigate to the create_user page before you go any further.

## 2. Creating the Form (HTML)


## 3. Handling the Form (Python)