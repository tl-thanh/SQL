# This repository contains answers to SQL questions; this is one of the class homework assignments.

## Here is how to install the sql files attached.

### In the terminal ... do the following.
1) Go to the specfic path where you want downloaded the 3 sql files.
2) Open up psql in the directory of these files and run these commands
3) Create your database
4) Connect to your database
5) \i pagila-schema.sql
6) \i pagila-data.sql
7) \i pagila-insert-data.sql

### Here are the answers to the SQL questions:

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

ALTER TABLE actor
ALTER COLUMN middle_name SET DATA TYPE TEXT;

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

UPDATE actor
SET first_name = REPLACE(first_name,'GROUCHO','HARPO')
WHERE first_name='GROUCHO' AND last_name='WILLIAMS';

•	4d. Perhaps we were too hasty in changing GROUCHO to HARPO. It turns out that GROUCHO was the correct name after all! 
In a single query, 
if the first name of the actor is currently HARPO, change it to GROUCHO. 
Otherwise, change the first name to MUCHO GROUCHO, as that is exactly what the actor will be with the grievous error. 
BE CAREFUL NOT TO CHANGE THE FIRST NAME OF EVERY ACTOR TO MUCHO GROUCHO
(Hint: update the record using a unique identifier.)

UPDATE actor
SET first_name = REPLACE(first_name, 'HARPO', 'GROUCHO')
WHERE first_name='GROUCHO' AND last_name='WILLIAMS';

•	5a.                                                                                                                      
o What’s the difference between a left join and a right join?

Left join returns all the rows from the left table, even if there are no matches from the right table.

Right join returns all the rows from the right table, even if there are no matches from the left table.

o What about an inner join and an outer join? 

Inner join returns the rows where there’s a match in both tables.

Outer join returns all rows from both tables, whether there is a match or not.

o When would you use rank? 

RANK gives ranking to your data based on your row, but it skipped rank if there are ties.  It provides the same ranking to “ties”.

o What about dense_rank? 

Similar to RANK, it ranks your data but in consecutive order.  It doesn’t skipped rank.

o When would you use a subquery in a select? 

Subquery is used when you know how to search for a value using a SELECT statement, but don’t know the exact value in the database.

o When would you use a right join?

A right join is similar to a left join except you are referencing the table on the “right”.  You want to return all data from the right table, but only certain data from the left table that intersect the data on the right table.

o When would you use an inner join over an outer join?

Use an inner join when you want to find and return matching data from both tables.  

o What’s the difference between a left outer and a left join?

There isn’t a difference really.

o When would you use a group by?

Group by is used to arrange identical data into groups.

o Describe how you would do data reformatting

You can use the FORMAT() function.
SELECT FORMAT (column_name, format)
FROM table_name;

o When would you use a with clause?

The WITH clause is used to combine subqueries.

o bonus: When would you use a self join?

When you want to compare values in a column with other values in the same column in the same table.

You’re answering each of these questions. Please include examples and answer them in plain english. 
•	6a. Use a JOIN to display the first and last names, as well as the address, of each staff member. Use the tables staff and address:

SELECT staff.first_name, staff.last_name, address.address
FROM staff
LEFT JOIN address ON staff.address_id=address.address_id;

•	6b. Use a JOIN to display the total amount rung up by each staff member in January of 2007. Use tables staff and payment.

SELECT staff.first_name, staff.last_name, SUM(payment_P2007_01.amount)
FROM staff
LEFT JOIN payment_P2007_01 ON staff.staff_id= payment_P2007_01.staff_id
GROUP BY staff.first_name, staff.last_name;

You’ll have to google for this one, we didn’t cover it explicitly in class. 

•	6c. List each film and the number of actors who are listed for that film. Use tables film_actor and film. Use inner join.

SELECT film.title, COUNT(film_actor.actor_id)
FROM film
INNER JOIN film_actor ON film.film_id=film_actor.film_id
GROUP BY film.title
ORDER BY film.title ASC;

•	6d. How many copies of the film Hunchback Impossible exist in the inventory system?

SELECT film.title, count(inventory.store_id)
FROM film
INNER JOIN inventory ON film.film_id=inventory.film_id
WHERE title iLIKE 'Hunchback Impossible'
GROUP BY film.title;

•	6e. Using the tables payment and customer and the JOIN command, list the total paid by each customer. List the customers alphabetically by last name:

SELECT customer.last_name, customer.first_name, SUM(payment.amount)
FROM customer
INNER JOIN payment ON customer.customer_id=payment.customer_id
GROUP BY customer.last_name, customer.first_name
ORDER BY customer.last_name  ASC;

•	7a. The music of Queen and Kris Kristofferson have seen an unlikely resurgence. As an unintended consequence, films starting with the letters K and Q have also soared in popularity. display the titles of movies starting with the letters K and Q whose language is English.

SELECT film.title, film.language_id, language.name
FROM film
INNER JOIN language ON film.language_id=language.language_id
WHERE title iLIKE 'K%' or title iLIKE 'Q%'
GROUP BY film.title, film.language_id, language.name
HAVING film.language_id='1';

•	7b. Use subqueries to display all actors who appear in the film Alone Trip.

SELECT sub.title, actor.first_name, actor.last_name
FROM (
SELECT film.title, film_actor.actor_id
FROM film
INNER JOIN film_actor ON film.film_id=film_actor.film_id
WHERE film.title iLIKE 'Alone Trip'	
) sub
	INNER JOIN actor ON sub.actor_id=actor.actor_id;

•	7c. You want to run an email marketing campaign in Canada, for which you will need the names and email addresses of all Canadian customers. Use joins to retrieve this information.

SELECT customer.first_name, customer.last_name, customer.email
FROM customer
INNER JOIN address ON customer.address_id = address.address_id
LEFT JOIN city ON address.city_id = city.city_id
LEFT JOIN country ON city.country_id = country.country_id
WHERE country.country_id = '20';

•	7d. Sales have been lagging among young families, and you wish to target all family movies for a promotion. Identify all movies categorized as a family film.

SELECT film.title
FROM film
INNER JOIN film_category ON film_category.film_id=film.film_id
WHERE category_id = '8';

Now we mentioned family film, but there is no family film category. There’s a category that resembles that. In the real world nothing will be exact.

•	7e. Display the most frequently rented movies in descending order.



•	7f. Write a query to display how much business, in dollars, each store brought in.

SELECT staff.store_id, SUM(payment.amount)
FROM staff
INNER JOIN payment ON staff.staff_id=payment.staff_id
LEFT JOIN store ON store.store_id=staff.store_id
GROUP BY staff.store_id;

•	7g. Write a query to display for each store its store ID, city, and country.

SELECT store.store_id, city.city, country.country
FROM store
INNER JOIN address ON store.address_id=address.address_id
LEFT JOIN city ON address.city_id=city.city_id
LEFT JOIN country ON city.country_id=country.country_id;

•	7h. List the top five genres in gross revenue in descending order. 

SELECT category.name, SUM(payment.amount)
FROM category
INNER JOIN film_category ON film_category.category_id = category.category_id

•	8a. In your new role as an executive, you would like to have an easy way of viewing the Top five genres by gross revenue. Use the solution from the problem above to create a view. 


•	8b. How would you display the view that you created in 8a?


•	8c. You find that you no longer need the view top_five_genres. Write a query to delete it.
