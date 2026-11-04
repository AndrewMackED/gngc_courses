# Part 6. Bootstrap Continued
In this lesson we are going to continue converting our old page elements to Bootstrap elements. This class we will look at the last two main components on our sites, Forms and Tables. We currently use forms for creating our users, and jobs, and we use tables to display our pending and completed jobs. This lesson will cover styling the new jobs page and the view jobs page, this will cover everything you need to style the elements on your pages.
## New Job Page (Bootstrap Forms)
Currently our create jobs page is setup like this:
```html
{% extends "base.html" %}

{% block title %} New Job {% endblock %}

{% block content %}
<form method="POST">
    <label for="user">User</label><br>
    <select id="user" name="user">
        {% for u in users %}
            <option value="{{u.id}}">{{u.name}}</option>
        {% endfor %}
    </select>
	
	<!-- All of our remaining questions-->
	
	
    <button type="submit">Create Job</button>
</form>
{% endblock %}

{% block scripts %}
<script>
    // Get today's date in local time ISO format (YYYY-MM-DD)
    const today = new Date().toLocaleDateString('en-CA');
    document.getElementById('date_required').setAttribute('min', today);
</script>
{% endblock %}
```

Our first step is to make our form centered on our page. To do this we wrap our `<form>` in three divs in this order:
- `<div  class="row justify-content-center">`
- `<div  class="col-12 col-md-8 col-lg-6">`
- `<div  class="card rounded-3 p-4 my-4">`
It will be setup like this:
```html
<div  class="row justify-content-center">
	<div  class="col-12 col-md-8 col-lg-6">
		<div  class="card rounded-3 p-4 my-4">
			<form method="POST">
				<!-- The rest of your form in here -->
			</form>
		</div>
	</div>
</div>
```

These divs are doing the following:
- The first div aligns our content in the center of our container using `justify-content-center`.
- The second div responsively resizes our content, adjusting the width depending on the screen size.
- The third div creates a Bootstrap card, which clearly defines the area that the form will be in with nice rounded edges and padding. You can learn more about the styling we can apply to our card [here](https://getbootstrap.com/docs/5.3/components/card/#card-styles).
### Styling our Labels: 
The next step is to format the labels of our form. To do this we just need to add `class="form-label"` to **all** of our labels, for example our user label would go from:
```html
<label  for="user">User</label>
```
to
```html
<label  for="user"  class="form-label">User</label>
```

### Styling our Selects:
For our `<select>` fields we add `class="form-select mb-3"`, for example our user select will go from:
```html
<select id="user" name="user">
	{% for u in users %}
		<option value="{{u.id}}">{{u.name}}</option>
	{% endfor %}
</select>
```
to 
```html
<select id="user" name="user" class="form-select mb-3">
	{% for u in users %}
		<option value="{{u.id}}">{{u.name}}</option>
	{% endfor %}
</select>
```

>[!Note] The `mb-3` ensures that we have a good amount of spacing between our questions, we are going to put this with every input in our form. You can read more about Bootstrap spacing [here](https://getbootstrap.com/docs/4.0/utilities/spacing/).

### Styling Text Area and Input:
To style our `<textarea>` and `<input>` we just need to add`class="form-control mb-3"` to each of them (this includes material and date). As an example our `<textarea>` will go from:
```html
<textarea id="description" name="description" rows="4" cols="80"></textarea>
```
to
```html
<textarea id="description" name="description" rows="4"  class="form-control mb-3"></textarea>
```
We can remove the `cols="80"` from our `<textarea>` since Bootstrap handles the width of our text area now. 

### Styling our Button:
To style our submit button we just need to add `class="btn"` to it. So it would go from:
```html
<button type="submit">Create Job</button>
```
to 
```html
<button type="submit" class="btn btn-primary">Create Job</button>
```
I added `btn-primary` to change the color of the button, you can see all of the different button variants [here](https://getbootstrap.com/docs/5.3/components/buttons/#variants).

The main content of our create jobs page should look like this:

```html
{% block content %}
<div class="row justify-content-center">
     <div class="col-12 col-md-8 col-lg-6">
        <div class="card rounded-3 p-4 my-4">
            <form method="POST">
                <label for="user" class="form-label">User</label>
                <select id="user" name="user" class="form-select mb-3">
                    {% for u in users %}
                        <option value="{{u.id}}">{{u.name}}</option>
                    {% endfor %}
                </select>

                <label for="machine" class="form-label">Machine</label>
                <select id="machine" name="machine" class="form-select mb-3">
                    {% for m in machines %}
                        <option value="{{m.id}}">{{m.name}}</option>
                    {% endfor %}
                </select>

                <label for="description" class="form-label">Explain the item you are asking to be manufactured</label>
                <textarea id="description" name="description" rows="4" cols="80" class="form-control mb-3"></textarea>

                <label for="material" class="form-label">Preferred Material and/or Colour</label>
                <input id="material" name="material" class="form-control mb-3"></input>

                <label for="date_required" class="form-label">Select the date you need the item by</label>
                <input type="date" id="date_required" name="date_required" class="form-control mb-3"></input>

                <button type="submit" class="btn btn-primary">Create Job</button>
            </form>
        </div>
    </div>
</div>
{% endblock %}
```

>[!Tip] Now you can use the same setup for your create user page and any other forms you have on your site.

## Jobs View Page (Bootstrap Tables)
For Bootstrap Tables there is less that we have to change. I will go through how to format the Pending table and you can use that to format your completed table.

The first thing we need to do is wrap our table in a div with the class `table-responsive` so we can make our table responsive. So we would go from:
```html
<table>
    <tr>
        <th>Job ID</th>
        <th>User</th>
        <th>Machine</th>
        <th>Description</th>
        <th>Material</th>
        <th>Date Logged</th>
        <th>Date Required</th>
        <th>Status</th>
    </tr>
    {%for j in jobs_pending%}
    <tr>
        <td>{{j.id}}</td>
        <td>{{j.user.name}}</td>
        <td>{{j.machine.name}}</td>
        <td>{{j.description}}</td>
        <td>{{j.material}}</td>
        <td>{{j.date_logged}}</td>
        <td>{{j.date_required}}</td>
        <td>
            {%if j.complete%}
            COMPLETE
            {%else%}
			<form action="{{ url_for('mark_complete', job_id=j.id) }}" method="POST">
	            <button type="submit">Mark Complete</button>
            </form>
			{%endif%}
        </td>
    </tr>
    {%endfor%}
</table>
```
to: 
```html
<div class="table-responsive">
	<table>
	    <tr>
	        <th>Job ID</th>
	        <th>User</th>
	        <th>Machine</th>
	        <th>Description</th>
	        <th>Material</th>
	        <th>Date Logged</th>
	        <th>Date Required</th>
	        <th>Status</th>
	    </tr>
	    {%for j in jobs_pending%}
	    <tr>
	        <td>{{j.id}}</td>
	        <td>{{j.user.name}}</td>
	        <td>{{j.machine.name}}</td>
	        <td>{{j.description}}</td>
	        <td>{{j.material}}</td>
	        <td>{{j.date_logged}}</td>
	        <td>{{j.date_required}}</td>
	        <td>
	            {%if j.complete%}
	            COMPLETE
	            {%else%}
				<form action="{{ url_for('mark_complete', job_id=j.id) }}" method="POST">
                    <button type="submit">Mark Complete</button>
                </form>
                {%endif%}
	        </td>
	    </tr>
	    {%endfor%}
	</table>
</div>
```

Next we want to make the table a Bootstrap table. To do this we change our `<table>` from:
```html
<table>
```
to 
```html
<table class="table table-bordered">
```
This tells Bootstrap to format this element like a table and adds borders to our table. 
>[!Note] There is a lot of formatting you can do with tables, you can read about them [here](https://getbootstrap.com/docs/5.3/content/tables/) (Variants and Accented tables are a good place to start). You can also adjust the color of your table borders, you can read about that [here](https://getbootstrap.com/docs/5.3/utilities/borders/#color). 

The last step is to make our "Mark Complete" button styled as well. This is done the same way we did it in our form. We just need to change:
```html
<button type="submit">Mark Complete</button>
```
to 
```html
<button type="submit" class="btn btn-primary">Mark Complete</button>
```

>[!Tip] You can check out the Bootstrap Docs [here](https://getbootstrap.com/docs/6.0/getting-started/install/) for styling any other components that you have in addition to the ones we have covered.

