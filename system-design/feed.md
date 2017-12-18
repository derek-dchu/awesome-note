## Twitter

#### Senario

When user A write a tweet, the tweet will be shown on its followers' timeline. 

#### Necessary

* Read timeline: QPS = 300k
* Write timeline: QPS = 5k
* Notify: 1m followers within 1s
* Concurrent Users: 15m
* Daily tweets = 5k \* 60 \* 60 \* 24 ~ 400m

#### Application

##### Push Model

When user A write a tweet, the tweet is written to its followers' timeline: O\(n\)

Then user B who follows A just need to retrieve his timeline which contains the tweet from A already: O\(1\)

###### Problem

When user A has huge number of followers, O\(n\) write becomes very large.

One solution: write to current online users

##### Pull Model

When user A write a tweet, the tweet is written to A's timeline: O\(1\)

Then user B who follows A need to retrieve his timeline from all his followers' timeline: O\(n\)

##### Push & Pull Model

When user A write a tweet, use the number of his followers as a condition to choose between Push and Pull API. The condition need to has overlap: Push when followers &lt; 100k, Pull when followers &gt;= 90k.

Then user B will retrieve his timeline by merging tweets directly from his timeline and tweets pull from users with large followers.



