# Part 4. Views and Database Queries

Now we have all the data being entered into our database, let's work on **displaying the data.**
Before we go any further, let's recap the assignment requirements:

1. View all jobs, sorted by creation time (sorted by pending and completed)
2. Users to view all their jobs (sorted pending and completed)
3. View all jobs per machine  
4. Machine Details

## So what's the plan here?
- Decide the data we need to display
- Extract the data we need from the database
- Display it in HTML (likely a table, maybe a card?)

# View all Jobs

## What data do we need?
![[Robotics Job Logger ERD.png]]
For viewing all jobs, we're going to need **everything** from the Job table, with a couple exceptions:
- User ID
	- Users aren't going to care about the User ID, they would probably prefer the User Name
- Machine ID
	- Same again, let's display the Machine Name instead of the Machine ID
- Complete
	- This is being stored as a boolean value, and when we try to display it, it will probably just get shown as a 0 or 1, so when we write the HTML for this, we're going to include a little IF statement to display it as "complete" or "pending"

## Extracting the Job Table Data
We are also going to want to get the Job table data. Since we're going to be displaying it in two sections ("Complete" and "Pending"), we're going to *query the database twice*, so we have the data grouped into two different objects. 

So let's talk about **Queries**. 

Queries are what we use in Databases to extract the data that we need. So far, we have just used basic queries, but we're going to want to extract fairly specific content from our database, and being able to sort it is really useful too.

### Basic Queries
Basic queries are really simple, we've done them before in fact!
These will just get all of the entries in a database table, without any sorting (just sorted in primary key order)

```python
machines = Machine.query.all()
users = User.query.all()
```

## Sorting and Filtering
If we want to do more advanced queries, we need to know about **sorting** and **filtering**

### Sorting
If we want to organise by date for example, that could be really useful!
```python
jobs = Job.query.order_by(Job.date_created.desc()).all()
```

### Filtering
Filtering is also really useful, and we're going to use this so sort outr jobs by whether they're completed or not!
```python
jobs_completed = Job.query.filter(Job.complete == true).all()
```

We can even stack these to get both filtered and sorted results!
```python
jobs = Job.query.filter(Job.complete == true).order_by(Job.date_created.desc()).all()
```

## Our Implementation
### Backend
In our `app.py` file , I'm going to be sending over two separate lists, one of them will be the Completed jobs, and the other list will be the Pending jobs:
```python
@app.route("/view_jobs", methods=["GET"])
def view_jobs():
    jobs_completed = Job.query.filter(Job.complete == True).order_by(Job.date_created.desc()).all()
    jobs_pending = Job.query.filter(Job.complete == False).order_by(Job.date_created.desc()).all()

    return render_template("view_jobs.html", jobs_completed = jobs_completed,jobs_pending = jobs_pending)

```

### Frontend
Now, we need to work out how we want to display all of this on the user's side. I'm going to suggest a table! Now, setting up tables in HTML can be a tad annoying, as we have explored before, so I'm going to give you this first one as a refresher, you'll have to do the rest:

```html
<h1>Pending</h1>
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
            NOT YET COMPLETE
            {%endif%}
        </td>
    </tr>
    {%endfor%}
</table>
```

Oh, don't forget to add some basic styling to you CSS for tables!
```css
table,
th,
td {
    border: 2px solid #333;
    border-collapse: collapse;
    padding: 10px;
}
```

That gives you the **Pending** table, to get the **Completed** table, you'll just use the exact same code, but change the list you're looping through to be the `jobs_completed` list.

## Marking Jobs as Completed
We're actually missing a piece of functionality here. We need to be able to change jobs to be marked as **completed** once they're finished!

This is a little bit of an interesting one, so follow along closely.

### Backend
```python
@app.route("/mark_complete/<int:job_id>", methods=["POST"])
def mark_complete(job_id):

    job = Job.query.get_or_404(job_id)
    job.complete = 1
    db.session.commit()

    return redirect(request.referrer or url_for("index"))
```

You'll note that this is a *post only* route, so we're not creating a new webpage here, we're just sending a request to the backend.
This code will:
- Accept a Job ID from the front-end
- Attempt to find the Job ID in the database, and return the specific job entry
- Mark the job entry as completed
- Update the Database
- Return the user to the route they came from (so we can actually use this functionality from anywhere in the website, and they'll go back to the page they came from)

### Frontend
Ok, so now we're going to need to update our HTML and Jinja, so that instead of displaying whether or not the job is complete, if it's PENDING we're going to provide a button for the user to click to mark is as complete.
This is the button code:
```html
<form action="{{ url_for('mark_complete', job_id=j.id) }}" method="POST">
	<button type="submit">Mark Complete</button>
</form>
```
Here it is again, in full context of the code we already had.
```html
<h1>Pending</h1>
<table>
    <tr>
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
        <td>{{j.user.name}}</td>
        <td>{{j.machine.name}}</td>
        <td>{{j.description}}</td>
        <td>{{j.material}}</td>
        <td>{{j.date_created}}</td>
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

Ok, let's see it!
![[Mark_as_complete.gif]]
Please pretend that the heading for the second section said "Completed", I noticed it too late...
