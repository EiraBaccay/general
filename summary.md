- Find the longest comment
``` SQL
SELECT body
FROM reddit
WHERE is_submitter = FALSE
ORDER BY LENGTH(body) DESC
LIMIT 1
```
| "body"               |
|------------------------|
|**Deep Purple**    [artist pic](https://lastfm-img2.akamaized.net/i/u/252/a0dc0410107a4df586c34d34191fabdb.png)    &gt; Deep Purple is an English hard rock band that formed in Hertfordshire in 1968. Together with groups such as  Black Sabbath and Led Zeppelin, they're considered as heavy metal pioneers... [Click for more](https://github.com/raraei/general/blob/master/longestcomment.md)|

- Find the shortest for every 6months
- Find authors will all caps for there usernames
- Find the sub reddit with most comments
- Find the comments created from Oct 1, 2017 to Oct 4, 2017
- Total count of comments with a score of 3
- Total count of comments per sub reddit
- Total count of the word "because"
- Find the user with most comment
- Find the author with most post
- Find the author with the longest name
``` SQL
SELECT COUNT(*)
FROM reddit
WHERE is_submitter
WHERE LENGTH(author) = (SELECT MAX(LENGTH(author)) FROM reddit)
```
| "author"               |
|------------------------|
| "-Dr-Mantis-Toboggan-" |
| "-JustAnotherRedditor" |
| "209u-096727961609276" |
| "30bmd972ms910bmt85nd" |
| "A_Farewell_to_Clones" |
| "Ambitiouscouchpotato" |
[Click for full list.](https://github.com/raraei/general/blob/master/longestnames.md)


- Total count of the comments that was edited
- Total count of the comments that has more than 600 characters
- List all the subreddits

