# Strategy
- Clarifications
- Bandwidth, Storage numbers
- Database design
- API design
- HLD
- LLD
- Bottlenecks

# Tips
- Take few features and explain its usage.
- Try a naive solution if you dont have any idea.
- Is it READ heavy? - URL redirection, Twitter, LinkedIn, Facebook

# Twitter
- READ heavy application.
- https://www.youtube.com/watch?v=KmAyPUv9gOY

## Features
1. Tweeting
2. Timeline
3. Following
4. Search
5. Push Notifications
6. Advertising

## Design 1: Naive Solution
Create a USER table and a TWEET table.

USER:
------------------
|id|name|following
------------------

TWEET:
-------------------
|id|content|ownerid
-------------------

- TWEET table will have all the tweets and the USER table will have all the users.
- Add a record to TWEET table during Tweeting
- Update record in USER table whenever the user follows someone new
- To get the Timeline content, a big select statement on TWEET is needed.
- ```SELECT * FROM TWEET WHERE OWNERID IN ( SELECT following FROM USER WHERE ID = 'pavan' )```

### Considerations
- Will be slow over the course of time as new records get added.
- Sharding could be some respite.
- Most users READ than WRITE. So over the course of time, application cannot scale.
- Twitter wants Eventual Consistency - high availability over accurate information.

## Design 2: 
Because it is READ heavy, it should be fast.
![image](https://user-images.githubusercontent.com/42272776/111832555-f2c1b900-8916-11eb-8fe0-7f42b26c6165.png)

Whenever a new post is submitted by an user, the REDIS clusters ( 3 from it ) will have their content recomputed and stored. So the next time their followers login or refresh, this cached content would be served.

This solution needs lot of memory which is still ok as the content is just a 140 character text.

For SEARCH functionality, we can use a SOLR index.

### Considerations
What if a celebrity posts a tweet - in this case, there will be large number of followers and so large number of computations involved at REDIS. To circumvent this, celebrity tweets can be stored in SQL/RDBMS and then dynamically be merged before serving to the end user.














