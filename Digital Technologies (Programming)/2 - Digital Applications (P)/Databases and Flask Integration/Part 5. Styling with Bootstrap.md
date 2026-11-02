
Now that we have our functionality complete we have to make our website look good. We are going to be using Bootstrap 5 for our styling.

Bootstrap is a front-end development framework that provides a collection of pre-written CSS and JavaScript files to let us style our UI elements without having to write the styling from scratch.

The CSS gives us styled elements and the JavaScript let's us have dynamic elements on our web pages.
>There is also a newly released Bootstrap 6, if you want to explore this feel free but there is currently more documentation and support for Bootstrap 5.

## 1. Importing Bootstrap
Our first step before we can start using Bootstrap to style our websites is importing it. Since all of our pages use the base.html template we only need to import Bootstrap once in our base.html file. 
To import bootstrap paste the following to replace the \<head> section at the top of your html file:
```html
<head>
	<meta  name="viewport"  content="width=device-width, initial-scale=1">
	<link  href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.3/dist/css/bootstrap.min.css"  rel="stylesheet"  integrity="sha384-rbsA2VBKQhggwzxH7pPCaAqO46MgnOM80zW1RWuH61DGLwZJEdK2Kadq2F9CUG65"  crossorigin="anonymous">
	<link  rel="stylesheet"  href="{{ url_for('static', filename='styles.css') }}">
	<title>Job Logging - {% block title %}{% endblock %}</title>
</head>
```
The first line tells our browser how to interpret our viewport (the part of the website we see at one time), which is important for mobile browsers in particular.
The second line is where we actually import the CSS elements of bootstrap.
The rest is the same as what we had before in the \<head> section.

Bootstrap also uses JavaScript so we need to import the JavaScript features of Bootstrap as well. Within the \<body> of your html paste the following line:
```html
<script  src="https://cdn.jsdelivr.net/npm/bootstrap@5.2.3/dist/js/bootstrap.bundle.min.js"></script>
```
> Make sure not to put this line in the {% block scripts %} section. Otherwise the JavaScript will get overridden on the pages that use additional JavaScript (such as our Create Job page)

## 2. Styling our Nav Bar
When we currently shrink our window, we can see that the nav bar gets squished into two lines.

![[Shrinking the Window.gif]]

If we were using our site on a mobile device that has a smaller screen this is what we would see constantly on every page since the nav bar is part of our base template. 
So let's start with styling our navbar.
Currently our nav bar looks like this:
```html
<div class="topnav">
	<a href="#home">Home</a>
	<a href="#new_job">New Job</a>
	<a href="#new_user">New User</a>
	<a href="#about">About</a>
</div>
```
We can replace it with a Bootstrap navbar:
```html
<div  class="navbar navbar-expand-md bg-primary">
	<div  class="container-fluid">
		<a  class="navbar-brand"  href="#">RoJoR</a>
		<div  class="navbar-nav me-auto">
			<a  class="nav-link"  href="{{ url_for('index') }}">Home</a>
			<a  class="nav-link"  href="{{ url_for('new_job') }}">New Job</a>
			<a  class="nav-link"  href="{{ url_for('new_user') }}">New User</a>
			<a  class="nav-link"  href="{{ url_for('about') }}">About</a>
		</div>
	</div>
</div>
```
You can see that every element has the text "class =" inside of it. This is because Bootstrap uses these class names to apply styling to our elements. In our first line for example:
```html 
<nav  class="navbar navbar-expand-md bg-primary">
```
"navbar" tells Bootstrap to format this section of the page as a nav bar. 
The "navbar-expand-md" makes our page links show up horizontally across our nav bar and also handles the responsive design, telling Bootstrap that if the size of our screen is medium or larger to expand the navbar.
In Bootstrap we use sm, md and lg to write how we want our page formatted for small, medium and large screens. This means we can make our site look different depending on the size of our screen.

Moving on to the other classes we can see in our code:
- The "container-fluid" class makes a section that fills the entire width of our screen.
- The "navbar-brand" class is where we can put our website brand name. This makes our site more professional and consistent across the pages.
- The "navbar-nav" class holds the links to our different web pages and the "me-auto" in this class put our links on the left side of our nav bar, if we want the links to be on the top right of our screen we could replace this with "ms-auto"
- The "nav-link" class styles the individual links 

**Note: You may have added more links to your navigation bar or may be using different names for your pages, make sure to include all pages in your navigation bar with the correct links! Keep this in mind when copying code for this whole section.**
## 3. Making our Nav Bar Responsive
Currently our nav bar changes it's layout when our screen changes sizes but our links are still visible. We want to change our nav bar so that when the screen is small the nav bar becomes collapable, this keeps its small and tucked away while still being useful.

The first step will be adding our toggle button this is what we can press to expand and collapse our nav bar. Directly after our navbar-brand, add the following button:
```html
<button  class="navbar-toggler"  type="button"  data-bs-toggle="collapse"  data-bs-target="#navbarContent">
	<span  class="navbar-toggler-icon"></span>
</button>
```
How does this code work:
- ```navbar-toggler``` styles our button to look like a Bootstrap navigation toggle button 
- ```data-bs-toggle="collapse"``` lets Bootstrap know that this button will control a collapsible element
- ```data-bs-target="#navbarContent"``` tells Bootstrap that the button should show or hide an element called #navbarContent
- ```navbar-toggler-icon``` displays a menu icon on our button (the three horizontal lines)

Next we want to make our navigation links collapsible. Our navbar links currently look like this:
``` html
<div  class="navbar-nav me-auto">
	<a  class="nav-link"  href="{{ url_for('index') }}">Home</a>
	<a  class="nav-link"  href="{{ url_for('new_job') }}">New Job</a>
	<a  class="nav-link"  href="{{ url_for('new_user') }}">New User</a>
	<a  class="nav-link"  href="{{ url_for('about') }}">About</a>
</div>
```
We want to wrap this section in another div that we can make collapsible like this:
```html
<div  class="collapse navbar-collapse"  id="navbarContent">
	<div  class="navbar-nav me-auto">
		<a  class="nav-link"  href="{{ url_for('index') }}">Home</a>
		<a  class="nav-link"  href="{{ url_for('new_job') }}">New Job</a>
		<a  class="nav-link"  href="{{ url_for('new_user') }}">New User</a>
		<a  class="nav-link"  href="{{ url_for('about') }}">About</a>
	</div>
</div>
```
In our newly added div:
- ```collapse``` tells Bootstrap that this element is collapsible 
- ```navbar-collapse``` applies the styling for a collapsible nav bar
- The ```id = navbarContent``` is the name of this collapsible area, it is important that this matches what we put for the  ```data-bs-target``` in our toggle button. The page uses this name to know what to collapse when we press the toggle button.

Now let's see what it looks like on our webpage:

![[Responsive Navbar Demo.gif]]

With all of the updates our base.html should look something like this: (**Please remember that this file may not have all of the pages as your own site so please do not blindly copy it**)
```html
<!DOCTYPE  html>
<html>
<head>
	<link  href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.3/dist/css/bootstrap.min.css"  rel="stylesheet"  integrity="sha384-rbsA2VBKQhggwzxH7pPCaAqO46MgnOM80zW1RWuH61DGLwZJEdK2Kadq2F9CUG65"  crossorigin="anonymous">
	<link  rel="stylesheet"  href="{{ url_for('static', filename='styles.css') }}">
	<title>Job Logging - {% block title %}{% endblock %}</title>
</head>
<body>
	<div  class="navbar navbar-expand-md bg-primary">
		<div  class="container-fluid">
			<a  class="navbar-brand"  href="#">RoJoR</a>
			<button  class="navbar-toggler"  type="button"  data-bs-toggle="collapse"  data-bs-target="#navbarContent">
				<span  class="navbar-toggler-icon"></span>
			</button>
			<div  class="collapse navbar-collapse"  id="navbarContent">
				<div  class="navbar-nav me-auto">
					<a  class="nav-link"  href="{{ url_for('index') }}">Home</a>
					<a  class="nav-link"  href="{{ url_for('new_job') }}">New Job</a>
					<a  class="nav-link"  href="{{ url_for('new_user') }}">New User</a>
					<a  class="nav-link"  href="{{ url_for('about') }}">About</a>
				</div>
			</div>
		</div>
	</div>

	<div  style="padding-left:16px">
	{% block content %}{% endblock %}
	</div>

	<script  src="https://cdn.jsdelivr.net/npm/bootstrap@5.2.3/dist/js/bootstrap.bundle.min.js"></script>
	{% block scripts %}
	{% endblock %}
</body>
</html>
```
## 4. Make Your Navbar Your Own
Now that you have seen some of what we can do with Bootstrap for our nav bar, have a go at customising yours to make it visually appealing and useful.
There is a lot of information and examples on the Bootstrap documentation, it can be overwhelming at first so I've given you a few things to check out to start.
There documentation for Navbars is [here](https://getbootstrap.com/docs/5.0/components/navbar) (Check out what you can do with Brands, Color Schemes and Placement)
The documentation for Navs is [here](https://getbootstrap.com/docs/5.0/components/navs-tabs/) (Check out  the available styles section)
## 5. Showing Active Pages
May do this later (Just uses variables from flask with Jinja to set active to true or false) makes it easier to see what page you're on
