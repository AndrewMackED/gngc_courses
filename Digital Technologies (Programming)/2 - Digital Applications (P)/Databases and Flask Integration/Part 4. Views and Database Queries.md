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

Queries are what we use in Databases to extract the data that we need. So far, we have just used basic queries, 




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