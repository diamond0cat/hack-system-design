why we choose eventual consistency over strong consistency?
-

- ideally we want to use strong consistency on all our light operations but of course,  sometimes achieving strong consistency results in a large latency or preformance overhead. what will the effect on those 3 things:
    - if we switch away from strong consistency to eventual consistency
    - 三个要考虑的点：
      - 1. the realiaty of the sevice
      - 2. the functional correctness of the sevice
      - 3. the user experience
      - 