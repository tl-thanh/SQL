•	1a. You need a list of all the actors’ first name and last name

SELECT first_name, last_name
FROM actor;

•	1b. Display the first and last name of each actor in a single column in upper case letters. Name the column Actor Name

SELECT CONCAT (UPPER(first_name), ' ', UPPER(last_name))
AS Actor_Name
FROM actor;

•	2a. You need to find the id, first name, and last name of an actor, of whom you know only the first name of "Joe." What is one query would you use to obtain this information?

SELECT actor_id, first_name, last_name
FROM actor
WHERE first_name='JOE';

•	2b. Find all actors whose last name contain the letters GEN. Make this case insensitive

SELECT *
FROM actor
WHERE last_name iLIKE '%GEN%';

•	2c. Find all actors whose last names contain the letters LI. This time, order the rows by last name and first name, in that order. Make this case insensitive.

SELECT *
FROM actor
WHERE last_name iLIKE '%LI%'
ORDER BY last_name ASC, first_name ASC;


•	2d. Using IN, display the country_id and country columns of the following countries: Afghanistan, Bangladesh, and China:

SELECT country_id, country
FROM country
WHERE country IN ('Afghanistan', 'Bangladesh', 'China');

•	3a. Add a middle_name column to the table actor. Specify the appropriate column type

ALTER TABLE actor
ADD middle_name varchar(45);

•	3b. You realize that some of these actors have tremendously long last names. Change the data type of the middle_name column to something that can hold more than varchar.



•	3c. Now write a query that would remove the middle_name column.

ALTER TABLE actor
DROP COLUMN middle_name;

•	4a. List the last names of actors, as well as how many actors have that last name.

SELECT last_name, count(last_name)
FROM actor
GROUP BY last_name;

•	4b. List last names of actors and the number of actors who have that last name, but only for names that are shared by at least two actors

SELECT last_name, count(*)
FROM actor
GROUP BY last_name
HAVING count(*) > 1;

•	4c. Oh, no! The actor HARPO WILLIAMS was accidentally entered in the actor table as GROUCHO WILLIAMS. Write a query to fix the record.

SELECT REPLACE(first_name,'GROUCHO','HARPO')
FROM actor
WHERE first_name='GROUCHO' AND last_name='WILLIAMS';

•	4d. Perhaps we were too hasty in changing GROUCHO to HARPO. It turns out that GROUCHO was the correct name after all! 
In a single query, 
if the first name of the actor is currently HARPO, change it to GROUCHO. 
Otherwise, change the first name to MUCHO GROUCHO, as that is exactly what the actor will be with the grievous error. 
BE CAREFUL NOT TO CHANGE THE FIRST NAME OF EVERY ACTOR TO MUCHO GROUCHO
(Hint: update the record using a unique identifier.)
•	5a. 
o	What’s the difference between a left join and a right join?

Left join returns all the rows from the left table, even if there are no matches from the right table.

Right join returns all the rows from the right table, even if there are no matches from the left table.

o	What about an inner join and an outer join? 

Inner join returns the rows where there’s a match in both tables.

Outer join returns all rows from both tables, whether there is a match or not.

o	When would you use rank? 

RANK gives ranking to your data based on your row, but it skipped rank if there are ties.  It provides the same ranking to “ties”.

o	What about dense_rank? 

Similar to RANK, it ranks your data but in consecutive order.  It doesn’t skipped rank.

o	When would you use a subquery in a select? 

Subquery is used when you know how to search for a value using a SELECT statement, but don’t know the exact value in the database.

o	When would you use a right join?

A right join is similar to a left join except you are referencing the table on the “right”.  You want to return all data from the right table, but only certain data from the left table that intersect the data on the right table.

o	When would you use an inner join over an outer join?

Use an inner join when you want to find and return matching data from both tables.  

o	What’s the difference between a left outer and a left join?

There isn’t a difference really.

o	When would you use a group by?

Group by is used to arrange identical data into groups.

o	Describe how you would do data reformatting



o	When would you use a with clause?

The WITH clause is used to combine subqueries.

o	bonus: When would you use a self join?

When you want to compare values in a column with other values in the same column in the same table.

You’re answering each of these questions. Please include examples and answer them in plain english. 
•	6a. Use a JOIN to display the first and last names, as well as the address, of each staff member. Use the tables staff and address:

SELECT staff.first_name, staff.last_name, address.address
FROM staff
LEFT JOIN address ON staff.address_id=address.address_id;

•	6b. Use a JOIN to display the total amount rung up by each staff member in January of 2007. Use tables staff and payment.

SELECT staff.first_name, staff.last_name, payment.amount
FROM staff
LEFT JOIN payment ON staff.staff_id=payment.staff_id
WHERE payment_date iLIKE ‘2007-01%’;

You’ll have to google for this one, we didn’t cover it explicitly in class. 
•	6c. List each film and the number of actors who are listed for that film. Use tables film_actor and film. Use inner join.


•	6d. How many copies of the film Hunchback Impossible exist in the inventory system?


•	6e. Using the tables payment and customer and the JOIN command, list the total paid by each customer. List the customers alphabetically by last name:



•	7a. The music of Queen and Kris Kristofferson have seen an unlikely resurgence. As an unintended consequence, films starting with the letters K and Q have also soared in popularity. display the titles of movies starting with the letters K and Q whose language is English.



•	7b. Use subqueries to display all actors who appear in the film Alone Trip.
•	7c. You want to run an email marketing campaign in Canada, for which you will need the names and email addresses of all Canadian customers. Use joins to retrieve this information.


•	7d. Sales have been lagging among young families, and you wish to target all family movies for a promotion. Identify all movies categorized as a family film.


Now we mentioned family film, but there is no family film category. There’s a category that resembles that. In the real world nothing will be exact.
•	7e. Display the most frequently rented movies in descending order.


•	7f. Write a query to display how much business, in dollars, each store brought in.


•	7g. Write a query to display for each store its store ID, city, and country.


•	7h. List the top five genres in gross revenue in descending order. 


•	8a. In your new role as an executive, you would like to have an easy way of viewing the Top five genres by gross revenue. Use the solution from the problem above to create a view. 


•	8b. How would you display the view that you created in 8a?


•	8c. You find that you no longer need the view top_five_genres. Write a query to delete it.
