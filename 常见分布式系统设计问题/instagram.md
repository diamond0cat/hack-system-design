### clarify problem statement, showing that you have good picture about the service 

- functional:  2 or 3 core features
  - store / get images  ==> cheaper storage
  - follow 
  - like and comment on images
  - can you comment on a comment   not required
  - can you like a comment  not required
  - share images
    - publish a newsfeed


- non functional:
  - high availability
  - low latency
  - 
### any of the features are not obvious, what about the interviewer are expecting something else, better ask them what are you expecting


- DB design for like/comment:
  - likes table
    - post ID
    - user ID
    - timestamp
    - isActive  (soft delete)

  - post table
    - postID
    - text
    - imageID
    - Timestamp

  - comment table
    - 