1. How can you retrieve all the information from the cd.facilities table?


```SELECT *
FROM cd.facilities
```

2. You want to print out a list of all of the facilities and their cost to members. How would you retrieve a list of only facility names and costs?

```SELECT name, membercost
FROM cd.facilities
```

3. How can you produce a list of facilities that charge a fee to members?
```SELECT name, membercost
FROM cd.facilities
WHERE membercost > 0
```

4. How can you produce a list of facilities that charge a fee to members, and that fee is less than 1/50th of the monthly maintenance cost? Return the facid, facility name, member cost, and monthly maintenance of the facilities in question.

```SELECT name, membercost, monthlymaintenance
FROM cd.facilities
WHERE membercost > 0
AND membercost < (monthlymaintenance/50)
```

5. How can you produce a list of all facilities with the word 'Tennis' in their name?

```
SELECT name
FROM cd.facilities
WHERE name LIKE '%Tennis%'
```

6. How can you retrieve the details of facilities with ID 1 and 5? Try to do it without using the OR operator.

7.
How can you produce a list of members who joined after the start of September 2012? Return the memid, surname, firstname, and joindate of the members in question.
How can you produce an ordered list of the first 10 surnames in the members table? The list must not contain duplicates.
You'd like to get the signup date of your last member. How can you retrieve this information?
Produce a count of the number of facilities that have a cost to guests of 10 or more.
Skip this one, no question for #11.
Produce a list of the total number of slots booked per facility in the month of September 2012. Produce an output table consisting of facility id and slots, sorted by the number of slots.
Produce a list of facilities with more than 1000 slots booked. Produce an output table consisting of facility id and total slots, sorted by facility id.
How can you produce a list of the start times for bookings for tennis courts, for the date '2012-09-21'? Return a list of start time and facility name pairings, ordered by the time.
How can you produce a list of the start times for bookings by members named 'David Farrell'?
