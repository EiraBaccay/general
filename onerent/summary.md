## Creating the Database
I created a database in Postgres and connected it to [Postico](https://eggerapps.at/postico/).

In Postico, I created an empty table named `reddit` containing the following columns:
1. `author`
2. `is_submitter`: TRUE means that the text is a post, while FALSE means that the text is a comment
3. `body`: the contents of a post or comment
4. `created_utc`: the date when the post/comment was made
5. `retrieved_on`: the date when the text was retrieved
6. `subreddit`
7. `edited`: FALSE means the post/comment was not edited, otherwise, the date when it was last edited is provided
8. `score`
9. `permalink`
``` SQL
CREATE TABLE reddit(
   author VARCHAR (50) NOT NULL,
   is_submitter BOOL,
   body VARCHAR NOT NULL,
   created_utc INT NOT NULL,
   retrieved_on INT NOT NULL,
   subreddit VARCHAR NOT NULL,
   edited VARCHAR NOT NULL,
   score INT,
   permalink VARCHAR   
);
```
The data provided was a json file which I downloaded and then converted into a csv file via http://www.convertcsv.com/json-to-csv.htm. Then, I imported the csv file into table `reddit`.

![Table](https://github.com/raraei/general/blob/master/onerent/Screen%20Shot%202019-08-19%20at%205.46.02%20PM.png)

## Queries
#### 1. Find the longest comment
``` SQL
SELECT body
FROM reddit
WHERE is_submitter = FALSE
ORDER BY LENGTH(body) DESC
LIMIT 1
```
| body                   |
|------------------------|
|**Deep Purple**    [artist pic](https://lastfm-img2.akamaized.net/i/u/252/a0dc0410107a4df586c34d34191fabdb.png)    &gt; Deep Purple is an English hard rock band that formed in Hertfordshire in 1968. Together with groups such as  Black Sabbath and Led Zeppelin, they're considered as heavy metal pioneers... [Click for more](https://github.com/raraei/general/blob/master/onerent/longest_comment.md)|

#### 2. Find the shortest comment for every 6months
``` SQL
SELECT body
FROM reddit
WHERE LENGTH(body) = 1
```
#### 3. Find authors will all caps for their usernames

I excluded the usernames without any letters for this query.
``` SQL
SELECT DISTINCT author
FROM reddit
WHERE author SIMILAR TO '%[A-Z]%'
AND author = UPPER(author) 
```
| author               |
|----------------------|
| ---YNWA---           |
| -JAS0N-              |
| 102WOLFPACK          |
| A55A551N6847         |
| AA9126               |
[Click for full list.](https://github.com/raraei/general/blob/master/onerent/allcaps.csv)

#### 4. Find the sub reddit with most comments
``` SQL
SELECT subreddit, COUNT(*)
FROM reddit
WHERE is_submitter = FALSE
GROUP BY subreddit
ORDER BY COUNT DESC
LIMIT 1
```
| subreddit | count |
|-----------|-------|
| AskReddit | 460   |

#### 5. Find the comments created from Oct 1, 2017 to Oct 4, 2017
``` SQL
SELECT body, to_timestamp(created_utc)
FROM reddit
WHERE is_submitter = FALSE
AND to_timestamp(created_utc) BETWEEN '2017-10-01' AND '2017-10-04'
```
[Click for full list.](https://github.com/raraei/general/blob/master/onerent/Query5%20Results.csv)

#### 6. Total count of comments with a score of 3
``` SQL
SELECT COUNT(score)
FROM reddit
WHERE is_submitter = FALSE
GROUP BY score
HAVING score = 3
```
|  count  |
|---------|
| 830     |

#### 7. Total count of comments per sub reddit
``` SQL
SELECT subreddit, COUNT(*)
FROM reddit
WHERE is_submitter = FALSE
GROUP BY subreddit
ORDER BY COUNT DESC
```
| subreddit             | count |
|-----------------------|-------|
| AskReddit             | 460   |
| CFB                   | 403   |
| CrazyIdeas            | 261   |
| news                  | 158   |
| politics              | 114   |
[Click for full list.](https://github.com/raraei/general/blob/master/onerent/subreddit_comments.csv)

#### 8. Total count of the word "because"
``` SQL
CREATE TABLE because AS
SELECT regexp_split_to_table(body, E'\\s+') FROM reddit
```
``` SQL
SELECT COUNT(*)
FROM because
WHERE regexp_split_to_table SIMILAR TO '%because%'
```
|  count  |
|---------|
| 600     |
#### 9. Find the user with most comment

I excluded the '[deleted]' author as this will refer to multiple users.
``` SQL
SELECT author, COUNT(*)
FROM reddit
WHERE author != '[deleted]'
AND is_submitter = FALSE
GROUP BY author
ORDER BY COUNT DESC
LIMIT 1
```
| author             | count  |
|--------------------|--------|
| ithinkisaidtoomuch | 	258   |

#### 10. Find the author with most post
``` SQL
SELECT author, COUNT(*)
FROM reddit
WHERE author != '[deleted]'
AND is_submitter = TRUE
GROUP BY author
ORDER BY COUNT DESC
LIMIT 1
```
| author            | count   |
|-------------------|---------|
| Concise_AMA_Bot   | 	147   |

#### 11. Find the author with the longest name

I considered the longest name as the ones with most number of characters. Since many usernames have the same length, I have written the query such that it will result to all authors with the longest names.
``` SQL
SELECT COUNT(*)
FROM reddit
WHERE is_submitter
WHERE LENGTH(author) = (SELECT MAX(LENGTH(author)) FROM reddit)
```
| author               |
|----------------------|
| -Dr-Mantis-Toboggan- |
| -JustAnotherRedditor |
| 209u-096727961609276 |
| 30bmd972ms910bmt85nd |
| A_Farewell_to_Clones |
| Ambitiouscouchpotato |
[Click for full list.](https://github.com/raraei/general/blob/master/onerent/longest_names.csv)


#### 12. Total count of the comments that was edited
``` SQL
SELECT COUNT(*)
FROM reddit
WHERE edited != 'FALSE'
AND is_submitter = FALSE
```
| count   |
|---------|
| 8773    |
#### 13. Total count of the comments that has more than 600 characters
``` SQL
SELECT COUNT(*)
FROM reddit
WHERE is_submitter = FALSE
AND length(body) > 600
```
| count   |
|---------|
| 375    |
#### 14. List all the subreddits
``` SQL
SELECT DISTINCT subreddit
FROM reddit
```
| subreddit             |
|-----------------------|
| raspberry_pi          |
| Monsanto              |
| LINKTrader            |
| ArenaHS               |
| learnprogramming      |
[Click for full list.](https://github.com/raraei/general/blob/master/onerent/subreddit_list.csv)

