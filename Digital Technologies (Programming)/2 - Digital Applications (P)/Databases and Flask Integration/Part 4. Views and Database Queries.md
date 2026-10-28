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






## Basic Queries

## Sorting and Filtering