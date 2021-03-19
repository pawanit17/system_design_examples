# 
Clarifications
Bandwidth, Storage numbers
Database design
API design
HLD
LLD
Bottlenecks

# Tips
Take few features and explain its usage.
Try a naive solution if you dont have any idea.
Is it READ heavy? - URL redirection, Twitter, LinkedIn, Facebook

# Twitter
READ heavy application.

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
















