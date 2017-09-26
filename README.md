# This repository contains answers to SQL questions; this is one of the class homework assignments.

### Here is how to install the sql files attached.  In the terminal ... do the following.
1) Go to the specfic path where you downloaded the 3 sql files.
<br />	a) pagila-data.sql
<br />	b) pagila-insert-data.sql
<br />	c) pagila-schema.sql
2) Open up psql in the directory of these files and do the following
<br />	a) Create your database
<br />	b) Connect to your database
<br />	c) Run these commands
<br />	⋅⋅⋅1. \i pagila-schema.sql
<br />	⋅⋅⋅2. \i pagila-data.sql
<br />	⋅⋅⋅3. \i pagila-insert-data.sql

### Here are the answers to the SQL questions:

•	1a. You need a list of all the actors’ first name and last name

SELECT first_name, last_name<br />
FROM actor;

•	1b. Display the first and last name of each actor in a single column in upper case letters. Name the column Actor Name

SELECT CONCAT (UPPER(first_name), ' ', UPPER(last_name))<br />
AS Actor_Name<br />
FROM actor;

•	2a. You need to find the id, first name, and last name of an actor, of whom you know only the first name of "Joe." What is one query would you use to obtain this information?

SELECT actor_id, first_name, last_name<br />
FROM actor<br />
WHERE first_name='JOE';

•	2b. Find all actors whose last name contain the letters GEN. Make this case insensitive

SELECT *<br />
FROM actor<br />
WHERE last_name iLIKE '%GEN%';

•	2c. Find all actors whose last names contain the letters LI. This time, order the rows by last name and first name, in that order. Make this case insensitive.

SELECT *<br />
FROM actor<br />
WHERE last_name iLIKE '%LI%'<br />
ORDER BY last_name ASC, first_name ASC;

•	2d. Using IN, display the country_id and country columns of the following countries: Afghanistan, Bangladesh, and China:

SELECT country_id, country<br />
FROM country<br />
WHERE country IN ('Afghanistan', 'Bangladesh', 'China');<br />

•	3a. Add a middle_name column to the table actor. Specify the appropriate column type

ALTER TABLE actor<br />
ADD middle_name varchar(45);<br />

•	3b. You realize that some of these actors have tremendously long last names. Change the data type of the middle_name column to something that can hold more than varchar.

ALTER TABLE actor<br />
ALTER COLUMN middle_name SET DATA TYPE TEXT;<br />

•	3c. Now write a query that would remove the middle_name column.

ALTER TABLE actor<br />
DROP COLUMN middle_name;<br />

•	4a. List the last names of actors, as well as how many actors have that last name.

SELECT last_name, count(last_name)<br />
FROM actor<br />
GROUP BY last_name;<br />

•	4b. List last names of actors and the number of actors who have that last name, but only for names that are shared by at least two actors

SELECT last_name, count(last_name)<br />
FROM actor<br />
GROUP BY last_name<br />
HAVING count(last_name) > 1

•	4c. Oh, no! The actor HARPO WILLIAMS was accidentally entered in the actor table as GROUCHO WILLIAMS. Write a query to fix the record.

UPDATE actor<br />
SET first_name = REPLACE(first_name,'GROUCHO','HARPO')<br />
WHERE first_name='GROUCHO' AND last_name='WILLIAMS';

•	4d. Perhaps we were too hasty in changing GROUCHO to HARPO. It turns out that GROUCHO was the correct name after all! 
In a single query, 
if the first name of the actor is currently HARPO, change it to GROUCHO. 
Otherwise, change the first name to MUCHO GROUCHO, as that is exactly what the actor will be with the grievous error. 
BE CAREFUL NOT TO CHANGE THE FIRST NAME OF EVERY ACTOR TO MUCHO GROUCHO
(Hint: update the record using a unique identifier.)

UPDATE actor<br />
SET first_name = CASE<br />
WHEN first_name = 'GROUCHO' THEN 'MUCHO GROUCHO'<br />
WHEN first_name = 'HARPO' THEN 'GROUCHO'<br />
END<br />
WHERE actor_id = 172;

•	5a.<br />
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

You can use the FORMAT() function.
SELECT FORMAT (column_name, format)
FROM table_name;

o	When would you use a with clause?

The WITH clause is used to combine subqueries.

o	bonus: When would you use a self join?

When you want to compare values in a column with other values in the same column in the same table.

•	6a. Use a JOIN to display the first and last names, as well as the address, of each staff member. Use the tables staff and address:

SELECT staff.first_name, staff.last_name, address.address<br />
FROM staff<br />
LEFT JOIN address ON staff.address_id=address.address_id;

•	6b. Use a JOIN to display the total amount rung up by each staff member in January of 2007. Use tables staff and payment.

SELECT staff.first_name, staff.last_name, SUM(payment_P2007_01.amount)<br />
FROM staff<br />
LEFT JOIN payment_P2007_01 ON staff.staff_id= payment_P2007_01.staff_id<br />
GROUP BY staff.first_name, staff.last_name;<br />

OR

SELECT s.staff_id, s.first_name, s.last_name, SUM(p.amount) as total_amt<br />
FROM payment p<br />
JOIN staff s<br />
ON p.staff_id = s.staff_id<br />
WHERE LEFT((CAST(payment_date AS text)), 7) = '2007-01'<br />
GROUP BY s.staff_id;

•	6c. List each film and the number of actors who are listed for that film. Use tables film_actor and film. Use inner join.

SELECT film.title, COUNT(film_actor.actor_id)<br />
FROM film<br />
INNER JOIN film_actor ON film.film_id=film_actor.film_id<br />
GROUP BY film.title<br />
ORDER BY film.title ASC;

•	6d. How many copies of the film Hunchback Impossible exist in the inventory system?

SELECT film.title, count(inventory.store_id)<br />
FROM film<br />
INNER JOIN inventory ON film.film_id=inventory.film_id<br />
WHERE title iLIKE 'Hunchback Impossible'<br />
GROUP BY film.title;

•	6e. Using the tables payment and customer and the JOIN command, list the total paid by each customer. List the customers alphabetically by last name:

SELECT customer.last_name, customer.first_name, SUM(payment.amount)<br />
FROM customer<br />
INNER JOIN payment ON customer.customer_id=payment.customer_id<br />
GROUP BY customer.last_name, customer.first_name<br />
ORDER BY customer.last_name  ASC;

•	7a. The music of Queen and Kris Kristofferson have seen an unlikely resurgence. As an unintended consequence, films starting with the letters K and Q have also soared in popularity. display the titles of movies starting with the letters K and Q whose language is English.

SELECT film.title, film.language_id, language.name<br />
FROM film<br />
INNER JOIN language ON film.language_id=language.language_id<br />
WHERE title iLIKE 'K%' or title iLIKE 'Q%'<br />
GROUP BY film.title, film.language_id, language.name<br />
HAVING film.language_id='1';

•	7b. Use subqueries to display all actors who appear in the film Alone Trip.

SELECT sub.title, actor.first_name, actor.last_name<br />
FROM (<br />
SELECT film.title, film_actor.actor_id<br />
FROM film<br />
INNER JOIN film_actor ON film.film_id=film_actor.film_id<br />
WHERE film.title iLIKE 'Alone Trip'<br />
) sub<br />
INNER JOIN actor ON sub.actor_id=actor.actor_id;

•	7c. You want to run an email marketing campaign in Canada, for which you will need the names and email addresses of all Canadian customers. Use joins to retrieve this information.

SELECT customer.first_name, customer.last_name, customer.email<br />
FROM customer<br />
INNER JOIN address ON customer.address_id = address.address_id<br />
LEFT JOIN city ON address.city_id = city.city_id<br />
LEFT JOIN country ON city.country_id = country.country_id<br />
WHERE country.country_id = '20';

•	7d. Sales have been lagging among young families, and you wish to target all family movies for a promotion. Identify all movies categorized as a family film.

SELECT film.title<br />
FROM film<br />
INNER JOIN film_category ON film_category.film_id=film.film_id<br />
WHERE category_id = '8';

•	7e. Display the most frequently rented movies in descending order.

SELECT f.title, COUNT(i.inventory_id) AS times_rented<br />
FROM film f<br />
JOIN inventory i<br />
ON f.film_id = i.film_id<br />
JOIN rental r<br />
ON i.inventory_id = r.inventory_id<br />
GROUP BY f.title<br />
ORDER BY COUNT(i.inventory_id) DESC;

•	7f. Write a query to display how much business, in dollars, each store brought in.

SELECT staff.store_id, SUM(payment.amount)<br />
FROM staff<br />
INNER JOIN payment ON staff.staff_id=payment.staff_id<br />
LEFT JOIN store ON store.store_id=staff.store_id<br />
GROUP BY staff.store_id;

•	7g. Write a query to display for each store its store ID, city, and country.

SELECT store.store_id, city.city, country.country<br />
FROM store<br />
INNER JOIN address ON store.address_id=address.address_id<br />
LEFT JOIN city ON address.city_id=city.city_id<br />
LEFT JOIN country ON city.country_id=country.country_id;

•	7h. List the top five genres in gross revenue in descending order. 

SELECT c.name AS genre, SUM(p.amount) AS gross_revenue<br />
FROM category c<br />
JOIN film_category fc<br />
ON c.category_id = fc.category_id<br />
JOIN inventory i<br />
ON fc.film_id = i.film_id<br />
JOIN rental r<br />
ON i.inventory_id = r.inventory_id<br />
JOIN payment p<br />
ON r.rental_id = p.rental_id<br />
GROUP BY c.name<br />
ORDER BY gross_revenue DESC<br />
LIMIT 5;

•	8a. In your new role as an executive, you would like to have an easy way of viewing the Top five genres by gross revenue. Use the solution from the problem above to create a view. 


CREATE VIEW top_five_genre AS<br />
SELECT c.name AS genre, SUM(p.amount) AS gross_revenue<br />
FROM category c<br />
JOIN film_category fc<br />
ON c.category_id=fc.category_id<br />
JOIN inventory i<br />
ON fc.film_id=i.film_id<br />
JOIN rental r<br />
ON i.inventory_id=r.inventory_id<br />
JOIN payment p<br />
ON r.rental_id=p.rental_id<br />
GROUP BY c.name<br />
ORDER BY gross_revenue DESC<br />
LIMIT 5;

•	8b. How would you display the view that you created in 8a?

SELECT *<br />
FROM top_five_genre;

•	8c. You find that you no longer need the view top_five_genres. Write a query to delete it.

DROP VIEW top_five_genre;
