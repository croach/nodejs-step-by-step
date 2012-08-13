That should take a little while, so I'm going to take advantage of the time we have now to answer one of the questions
that has popped up in the comments for this series. Specifically, I want to talk about what makes Node just so good at
serving hundreds and thousands of simultaneous requests with a single thread of execution versus the more traditional
method of serving each request with its own thread.

I'm going to try and explain this concept with simple example. Visualize a very small restaurant, say one that can
serve up to three tables at a time, so this restaurant is very small indeed. Now let's assume that this restaurant has
one waiter to serve all of the guests, but that waiter must stay with each table until they have finished their
meal. Into our restaurant walks a couple looking for a relaxing lunch. Our waiter seats them, takes their order, and
brings them their meal and then sits with them taking care of their every beckon call until they have finished their
meal, and all is well with our fine eating establishment.

Now assume that another couple comes to lunch. Our waiter again seats them, takes their order, brings them their food,
remains with them throughout their entire meal. While the new couple is enjoying their lunch, however, a second couple
comes to the restaurant. Unfortunately for them, our waiter is already occupied and so they must wait until the current
couple finishes their meal before they can be seated. This is obviously a problem, as we have three tables in our
restaurant and so we could be serving up to three couples at a time! We must hire more waiters. So we bring on a second
waiter to handle another couple. The only problem is that our restaurant is very small and can barely fit two waiters
at the same time, so the two waiters start bumping into each other and slowing each other down a bit. We have improved
our service by being able to serve multiple meals in parallel, but we've slowed down each individual meal ever so
slightly and we still can't serve our full capacity since we only have two waiters.

The solution of course is to hire a third waiter. Now we can serve up to three diners at the same time, but
unfortunately our restaurant will not fit three waiters at a time. So now we have an issue and we need to figure out
how we can resolve it. One way to do it is to have one waiter waiting in the kitchen at all times and then alternate
between who's on the floor and who's in the kitchen. This is a brilliant idea on our part as it allows us to serve up
to our capacity simultaneously. Every time one of our waiters sees a small break (say the dining couple is having an
intimate conversation) they can run back and trade places with the waiter in the kitchen giving them some time to look
after their customers.

Not a bad solution, eh? But can we do better? We are currently serving our full capacity, yes, but at what cost? We
have to hire three waiters total (an expensive thing to do) and since only two can be on the floor at a time we are not
getting our full money's worth out of each employee. Not to mention that having even two waiters on the floor at one
time is pretty tight causing overall service to slow with just the two. Finally, what happens if we have only two
customers? I definitely think we can do better than that.

Let's take a look at the process of enjoying a meal. A customer comes in, is seated, looks over their menu shortly,
orders a meal and eats that meal. The first few minutes of a customers visit is busy for our waiter, but for the
majority of the meal, the customer is eating or conversing with their companions. Really the only time the waiter is
needed again is whenever the customer needs a drink refilled or wants their check (or perhaps to order a coffee and
desert). Do we really need to have a waiter sit with the customer for the entire time? Of course not, so, why don't we
change our approach. Let's get rid of two of our waiters (sorry guys) and instead of having our sole waiter sit with
each customer, now we'll have them sit them and get their order and then they can go off and serve another
customer. Then we can either have the customer signal our waiter when they need further assistance or we could have the
waiter check on each customer from time to time or a combination of the two methods. Now that's an efficient setup. We
can serve all three tables at the same time with a single waiter and since we have extra space without the other two
waiters we can even add a fourth table for more patrons which our single waiter user the new method can handle without
any issues.

This scenario is fantastic. Our customers are very happy and we are raking in the money now, but there is one problem
though. What happens if one of our customers takes up all of our waiters time. Assume that one of our customers comes
in and nothing is good enough for them. Their steak is too well done the first time and undercooked the next. They
order drink after drink and constantly need their waters refilled. You get the idea, our waiter basically has to spend
all of their time serving this one customer and since we have no other waiters, the line of customers begins to back up
until this one customer is finished and leaves.

I think this example gives you a good intuitive understanding for how Node works and it's typically how I like to think
of it. Obviously the waiters represent a thread in our system and each customer a request, the size of the restaurant
is supposed to represent the constraints of our system, and the tag team effort of the waiters trading places in the
kitchen represents the cost of doing a context switch in the OS. Given this example, it's easy to see just how a system
can quickly become bogged down with too many threads. The solution, of course, is to reduce the number of threads, but
to do so we must have customers (requests) that need very little attention from our waiter. I/O is like that in our
system. Performing I/O does not need the attention of a CPU since it is taken care of at the hardware level and using
techniques like polling and/or signaling our process can essentially answer the request and then only needs to come
back to it once the response is ready to be returned. It's like our waiter seating and serving our customers and then
only coming back once the meal is finished to bring the check. The one problem being a customer that needs a lot of our
attention. The demanding customer represents a request that does very
