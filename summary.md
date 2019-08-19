- Find the longest comment
``` SQL
SELECT body
FROM reddit
WHERE is_submitter = FALSE
ORDER BY LENGTH(body) DESC
LIMIT 1
```
| body               |
|------------------------|
|**Deep Purple**    [artist pic](https://lastfm-img2.akamaized.net/i/u/252/a0dc0410107a4df586c34d34191fabdb.png)    &gt; Deep Purple is an English hard rock band that formed in Hertfordshire in 1968. Together with groups such as  Black Sabbath and Led Zeppelin, they're considered as heavy metal pioneers... [Click for more](https://github.com/raraei/general/blob/master/longestcomment.md)|

- Find the shortest for every 6months
- Find authors will all caps for there usernames
``` SQL
SELECT DISTINCT author
FROM reddit
WHERE author SIMILAR TO '%[A-Z]%'
AND author = UPPER(author) 
```
| author                |
|------------------------|
| "---YNWA---"           |
| "-JAS0N-"              |
| "102WOLFPACK"          |
| "A55A551N6847"         |
| "AA9126"               |
[Click for full list.](https://github.com/raraei/general/blob/master/allcaps.md)

- Find the sub reddit with most comments
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

- Find the comments created from Oct 1, 2017 to Oct 4, 2017

- Total count of comments with a score of 3
``` SQL
SELECT COUNT(score)
FROM reddit
WHERE is_submitter = FALSE
GROUP BY score
HAVING score = 3
```
| "count" |
|---------|
| 830     |

- Total count of comments per sub reddit
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
[Click for full list.](https://github.com/raraei/general/blob/master/subreddit_comments.csv)

- Total count of the word "because"

- Find the user with most comment
``` SQL
SELECT author, COUNT(*)
FROM reddit
WHERE author != '[deleted]'
AND is_submitter = FALSE
GROUP BY author
ORDER BY COUNT DESC
LIMIT 1
```
| subreddit | count |
|-----------|-------|
| ithinkisaidtoomuch | 	258   |

- Find the author with most post
``` SQL

```
- Find the author with the longest name
``` SQL
SELECT COUNT(*)
FROM reddit
WHERE is_submitter
WHERE LENGTH(author) = (SELECT MAX(LENGTH(author)) FROM reddit)
```
| author               |
|------------------------|
| "-Dr-Mantis-Toboggan-" |
| "-JustAnotherRedditor" |
| "209u-096727961609276" |
| "30bmd972ms910bmt85nd" |
| "A_Farewell_to_Clones" |
| "Ambitiouscouchpotato" |
[Click for full list.](https://github.com/raraei/general/blob/master/longestnames.md)


- Total count of the comments that was edited
``` SQL

```
- Total count of the comments that has more than 600 characters
``` SQL

```
- List all the subreddits
``` SQL

```
