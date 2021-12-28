---
layout: post
title: Can You SQL Inject a Jukebox?
---

Recently, I was grabbing drinks with a former colleague of mine and placed an overconfident wager. While sipping on a round of vodka tonics at a local Chicago bar, I bet her that I could hack into the establishment's digital jukebox and force it to play her favorite song, Jarvis Cocker's <a href="https://www.youtube.com/watch?v=u2zDurRP1yU&ab_channel=JarvisCocker-Topic" target="_blank">*Aline*</a>, without paying the required dollar for the performance.

In a conspiratorial fashion, we headed over to the corner of the bar which housed the jukebox. Once there, I opened the search prompt and furiously entered a bevy of SQL commands. Despite my best attempts, none of them worked, and I found myself paying for a surprisingly expensive round of tequila shots. Over the next few days, however, I couldn't help but wonder whether I had either entered the wrong commands or if the jukebox was even vulnerable to SQL injections at all.


## SQL Injections

For the uninitiated, a SQL injection commonly occurs when someone writes SQL statements into an entry field in order to extract information. For example, let's say you are shopping on Nike's website for a new pair of running shoes. When you reach the checkout page, instead of entering your name into the appropriate form field, you type in the following text: "`something_wrong OR 1 = 1`"

This will execute the below SQL query on the backend:

```sql
SELECT * FROM Users
WHERE user_id = 'something_wrong'
OR 1 = 1;
```

Since there is probably no row in the database where user_id is equal to 'something_wrong', this command will return all of the rows in the Users table if the application is not protected against SQL injections. A hacker could use this command to retrieve personal information on users such as name, address, and payment details.

Here is another example of malicious SQL input. This time we use batched statements to drop the Users table.

```sql
SELECT * FROM Shoes
WHERE shoe_id = 1;
DROP TABLE Users;
```

## How to Protect Against SQL Injections

In the 2000s and 2010s these types of attacks were a substantial threat. However, today most static code scanners, such as Veracode, are able to detect these issues and flag them to software engineers. In my own software development work, I used Brakeman to assess the security of our Ruby on Rails applications. I added this code scanning tool to our CI/CD pipeline and this prevented code from being built and deployed into our environments before it passed through the scan. Additionally, many common web development frameworks and libraries automatically sanitize inputs to cleanse them of malicious commands.

For Ruby on Rails, the best practice is to use parameterized statements when querying the database from user inputs. The first query (unsafe) uses string interpolation to create a SQL query while the second example (safe) utilizes parameterization.

### Unsafe query

```ruby
Book.where("title = '%#{params[:title]}%'"
```

### Safe query
```ruby
Book.where("title = ?", params[:title])
```


## Notable Attacks
Searching around, I found a few interesting examples of hackers exploiting these vulnerabilities.

- On October 1, 2012, a hacker group called "Team GhostShell" published the personal records of students, faculty, employees, and alumni from 53 universities including Harvard, Princeton, Stanford, Cornell, Johns Hopkins, and the University of Zurich on pastebin.com. The hackers claimed that they were trying to "raise awareness towards the changes made in today's education", bemoaning changing education laws in Europe and increases in tuition in the United States
- In July 2012 a hacker group was reported to have stolen 450,000 login credentials from Yahoo!. The logins were stored in plain text and were allegedly taken from a Yahoo subdomain, Yahoo! Voices. The group breached Yahoo's security by using a "union-based SQL injection technique"
- On March 27, 2011, www.mysql.com, the official homepage for MySQL, was compromised by a hacker using SQL blind injection (*this seems ironic*)
- In February 2002, Jeremiah Jacks discovered that Guess.com was vulnerable to an SQL injection attack, permitting anyone able to construct a properly-crafted URL to pull down 200,000+ names, credit card numbers and expiration dates in the site's customer database.
- A team of attackers used SQL injection to penetrate corporate systems at several companies, primarily the 7-Eleven retail chain, stealing 130 million credit card numbers.

## How to Play Free Music

Circling back to the original story, apparently there is a <a href="https://forums.hak5.org/topic/39981-touchtunes-jukebox-pin-discovery-with-the-yso/" target="_blank">method</a> for hacking these jukeboxes. Unfortunately, it involves a remote and brute forcing the pin to break into the system and is probably beyond the scope of this post.
