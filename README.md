# System-design
As of now, this repository is mainly used to push my learnings towards System Design concepts.

[Note: Some terms you would here in system design interviews are Fault Tolerance, in which case a machine crashes. And Scalability, in which case machines need to be added to process more requests. Another term used often is request allocation. This means assigning a request to a server.]
Replication, Consistency,  Availability, Partition Tolerance, Vertical & Horizontal scaling, Sharding 
https://www.interviewbit.com/tutorial/basic-terminologies/#basic-terminologies

**1) Scaling – Horizontal Vs Vertical**
Horizontal – buy more machines i.e. scale horizontally is adding more servers
Vertical – buy bigger machine i.e. to increase the resources of the server (RAM, CPU, storage, etc.)

Example: Lets say you own a restaurant which is now exceeding its seating capacity. One way of accommodating more people ( scaling ) would be to add more and more chairs (scaling vertically). However since the space is limited, you won’t be able to add more chairs once the space is full.
Another way of scaling would be to open new branches of the restaurant ( horizontal scaling).

**2) Distributed system basics with example**
E.g. Pizza shop
Concepts goes behind: 
1 Vertical scaling: pay Master chef more money to serve more customers
2 Backup server: Master chef & slave chef (comes in action when master is absent) & avoids single pt of failure
3 Horizontal Scaling: hire new chefs
4 Preprocessing using cron jobs: preparing stuff at non-peak hrs
5 Microservices: Separate chefs for separate orders (ChefA for Pizza, ChefB for Garlic bread, ChefC for X), route Pizza request to ChefA & etc, well defined responsibity
6 Distributed Systems: When pizza shops expands its business to various places so business don’t get affected entirely in case of any unlikely event where only branch is closed and unable to serve customer orders. And in future newer branch may serve orders local to that area and can serve requests faster (Partitioning) solves Fault Tolerant & Quicker response time
7 Load Balancing: routes the customer order to the branch based on the time order would take to deliver the order (based on pending orders, traffic, ...)
8 Decoupling: Delivery agent is agnostic of the item (pizza/ burger/ etc) being delivered to customer
9 Logging & Metrics calculation: Logs different events to assess in case something goes wrong ( e.g. Faulty machine that is causing delay to make pizzas, or faulty vehicle stopping agent to deliver in estimated time)
10 Extensibility: should be designed in such a way that it can be extensible an no cost to support different business use-case 

**3) Load Balancer**
Load Balancing is a key concept to system design. One simple way would be hashing all requests and then sending them to the assigned server.

The standard way to hash objects is to map them to a search space, and then transfer the load to the mapped computer. A system using this policy is likely to suffer when new nodes are added or removed from it. 

Load balancing is often tied with service discovery and global locks. The type of load we want to balance is that of sticky sessions.

[Load Balancing, Algo types, Session persistence, Dynamic Configuration of Server Groups, Hardware vs. Software Load Balancing, Seven-Layer Open System Interconnection (OSI)]
https://www.nginx.com/resources/glossary/load-balancing/

Sticky session: https://stackoverflow.com/questions/10494431/sticky-and-non-sticky-sessions
https://aws.amazon.com/blogs/aws/new-elastic-load-balancing-feature-sticky-sessions/

**4) Consistent Hashing**
Load Balancing is a key concept to system design. One of the popular ways to balance load in a system is to use the concept of consistent hashing. Consistent Hashing allows requests to be mapped into hash buckets while allowing the system to add and remove nodes flexibly so as to maintain a good load factor on each machine.

Consistent Hashing maps servers to the key space and assigns requests(mapped to relevant buckets, called load) to the next clockwise server. Servers can then store relevant request data in them while allowing the system flexibility and scalability.

Keys (Ids) & Servers are hashed in such a way that requests get mapped with servers in uniform distributed way, but upon addition or removal of a server may invite sudden load (may be majority of requests goes to the same server in worst case and/or invalidate the cache) on one of the servers and can degrade the performance of the application terribly.

To handle that we create multiple k virtual servers for one server (e.g. 3 servers will have 12 virtual server/points in the hash ring when k=4) just like on bloom filter
so that the virtual servers are spread/distributed evenly in the ring, so it will have minimal impact of any change in the no of servers  (reassign fraction of requests to new servers if any node joins or leaves the network)

https://www.youtube.com/watch?v=jznJKL0CrxM (Cube Drone)

Ref: Gaurav Sen YouTube channel
