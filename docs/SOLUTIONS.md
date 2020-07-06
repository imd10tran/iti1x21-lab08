## Application

[Les principales classes de la simulation](lab8_img1_supermarket-01.png "Les principales classes de la simulation") **Figure 1 :** Main classes for the simulation.

The above figure shows the UML diagrams of the two main classes for the simulation. In addition to these classes, there is an interface, called Queue, as well as a simple implementation, called ArrayQueue. The source code is provided below.
Here are the files needed for the Queue implementation:

*   [Queue.java](https://www.site.uottawa.ca/~gvj/Courses/ITI1121/lectures/labs/lab8_template/Queue.java)
*   [ArrayQueue.java](https://www.site.uottawa.ca/~gvj/Courses/ITI1121/lectures/labs/lab8_template/ArrayQueue.java)

## Question 1:
### Customer
Write a class for modeling a customer. A customer knows its arrival time (represented by an **int** value), its initial number of items, as well as the number of items remaining to be processed. The maximum number of items per customer is (**MAX_NUM_ITEMS**).

*   What are the **instance variables**?
*   What are the **class variables**?

The constructor has a single parameter. It specifies the arrival time. The initial number of items is determined when the object is first created using the following formula:
```java
    Random generator;
    generator = new Random();

    int numItems;
    numItems = generator.nextInt(MAX_NUM_ITEMS-1)+1;
```
Here, we make sure that no customer would show up empty handed!

The instance methods of a customer include:

*   **int getArrivalTime()** returns the arrival time;
*   **int getNumberOfItems()** returns the number of items remaining to be processed;
*   **int getNumberOfServedItems()** returns the number of items that have been processed;
*   **serve()** decrements by one the number of items of this customer.
### Solutions

```java
//                              -*- Mode: Java -*- 
// Customer.java --- models a customer, determines the initial number of items
// Author          : Marcel Turcotte
// Created On      : Sat Mar  3 07:58:50 2007
// Last Modified By: Marcel Turcotte
// Last Modified On: Sat Mar  3 07:59:08 2007
// ITI 1121/1521. Introduction to Computer Science II

public class Customer {

    // Constant

    private static final int MAX_NUM_ITEMS = 25;

    // Instance variables

    private int arrivalTime;
    private int numberOfItems;
    private int initialNumberOfItems;

    // Constructor

    public Customer( int arrivalTime ) {
        this.arrivalTime = arrivalTime;
        numberOfItems =  (int) ( ( MAX_NUM_ITEMS - 1 ) * Math.random() ) + 1;
        initialNumberOfItems = numberOfItems;
    }

    // Access methods

    public int getArrivalTime() {
        return arrivalTime;
    }

    public int getNumberOfItems() {
        return numberOfItems;
    }

    public int getNumberOfServedItems() {
       return initialNumberOfItems - numberOfItems;
    }

    public void serve() {
        numberOfItems--;
    }
}

```
### Cashier
A cashier is responsible for helping a queue of customers. It serves one customer at a time. Since the simulation is used to produce statistics, a cashier also memorizes the total number of customers served, the total amount of time the customers have been waiting, as well as the total number of items served (processed).

*   What are the instance variables? What are the class variables?

The class has a single constructor. It has no parameters. It initializes the instance variables of the cashier.

The method **addCustomer( Customer c )** adds a customer to the rear of its queue. The method **int getQueueSize()** returns the number of customers currently waiting in line.

The method **serveCustomers( int currentTime )** is a key element of the simulation. The method **serveCustomers** of each cashier is called once for each step of the simulation. The parameter **currentTime** is used to compute the total amount of time this customer has spent waiting in line. Here is the behaviour of the cashier when serving customers.

*   If the cashier is not presently serving a customer, the next customer in line becomes the current customer, unless the queue is empty. Whenever, a customer is taken out of the queue, the cashier tallies the total amount time this customer spent waiting in line. If the cashier has no customer and the queue is empty, there is nothing to be done for this step;
*   The cashier serves one item (a call to the method serve() of its current customer);
*   If the current customer has no more items, the cashier adds the number of items of this customer to the tally. The state of this cashier object now indicates that this cashier has no current customer (the customer has been sent away).

There are also 3 instance methods used to report statistics for the total waiting time, total number of items served and total number of customers served by this cashier: **int getTotalCustomerWaitTime()**, **int getTotalItemsServed()**, and **int getTotalCustomersServed()**. Finally, the **String toString()** method returns a String that summarizes the statistics of this cashier.


### Solutions

```java

public class Cashier {

    // Constant

    private static final String nl = System.getProperty( "line.separator" );

    // Instance variables

    private Queue<Customer> queue;
    private Customer currentCustomer;

    private int totalCustomerWaitTime;
    private int customersServed;
    private int totalItemsServed;
   
    // constructor

    public Cashier(){

        queue = new ArrayQueue<Customer>();

        totalCustomerWaitTime = 0;
        customersServed = 0;
        totalItemsServed = 0;
    }

    public void addCustomer( Customer c ) {
        queue.enqueue( c );
    }

    public int getQueueSize() {
        return queue.size();
    }

    public void serveCustomers( int currentTime ){

        if ( currentCustomer == null && queue.isEmpty() ) {
            return;
        }

        // Either currentCustomer is not null or the queue is not empty!

        if ( currentCustomer == null ){
            
            currentCustomer = queue.dequeue();

            totalCustomerWaitTime += currentTime - currentCustomer.getArrivalTime();

            customersServed++;

        }

        // Give a unit of service

        currentCustomer.serve();

        // If current customer is finished, send it away

        if ( currentCustomer.getNumberOfItems() == 0 ) {
            totalItemsServed += currentCustomer.getNumberOfServedItems();
            currentCustomer = null;    
        }
    }

    public int getTotalCustomerWaitTime() {      
        return totalCustomerWaitTime;      
    }    

    public int getTotalItemsServed() {      
        return totalItemsServed;
    }    

    public int getTotalCustomersServed() {
        return customersServed;    
    }

    // private int totalCustomerWaitTime;
    // private int customersServed;
    // private int totalItemsServed;
   
    public String toString() {

        StringBuffer results = new StringBuffer();

        results.append( "The total number of customers served is " );
        results.append( customersServed );
        results.append( nl );

        results.append( "The average number of items per customer was " );
        results.append( totalItemsServed / customersServed );
        results.append( nl );

        results.append( "The average waiting time (in seconds) was " );
        results.append( totalCustomerWaitTime / customersServed );
        results.append( nl );

        return results.toString();
    }

}

```

## Question 2. :
### Simulation

*   [Simulation.java](https://www.site.uottawa.ca/~gvj/Courses/ITI1121/lectures/labs/lab8_template/Simulation.java)

The class **Simulation** orchestrates the simulation. A **Simulation** object has two cashiers; one is responsible for the express line, the other for the regular line. The object also memorizes the duration of the simulation. The constructor creates the two necessary cashier objects.

The method **run()** implements the main loop of the simulation. It sets the current time to zero then increments the current time by a fixed amount (TICK) at each iteration.

At each iteration, the method **run** must:

*   Determine if a new customer has arrived, and if so place this customer in the appropriate waiting line (based on the number of items);
*   Tell the two cashiers to serve their customers;
*   Increment the current time.

At the end of the simulation, the method **run()** displays the statistics.

1.  Execute the simulation several times. Discuss your observations with your neighbours. Here is an example of a run:

    SIMULATION ::
    The duration (in seconds) of the simulation was 28800
```java
    EXPRESS LINE ::
    The total number of customers served is 376
    The average number of items per customer was 6
    The average waiting time (in seconds) was 16

    REGULAR LINE ::
    The total number of customers served is 311
    The average number of items per customer was 18
    The average waiting time (in seconds) was 2597
```
3.  As you can see, the average waiting time for the express line was short (16 seconds!) but the customers in the regular line are waiting nearly 45 minutes. The manager needs to add more regular lines. But how many lines should be added? To answer this question, you will need to modify this application to allow for more regular lines;
4.  If time allows, experiment with different parameter values: change the probability of an arrival, the number of items, the number of regular and express checkout lines.
### Solutions

```java
public class Simulation {

    private static final String nl = System.lineSeparator();

    private static final int SECONDS_PER_MINUTE = 60;
    private static final int MINUTES_PER_HOUR = 60;
    private static final int TICK = 5;

    private static final double PROBABILITY_NEW_ARRIVAL = 0.125;

    private static final int EXPRESS_MAX_NUM_ITEMS = 12;

    // Instance variables

    private Cashiers express;
    private Cashiers regular;

    private int lengthOfSimulation;

    // Constructor

    public Simulation( int duration ) {

        lengthOfSimulation = duration;

        express = new Cashiers( 1 );
        regular = new Cashiers( 2 );

    }
   
    public void run() {

        int currentTime = 0;

        while ( currentTime < lengthOfSimulation ) {

            if ( Math.random() <= PROBABILITY_NEW_ARRIVAL ) {

                Customer customer = new Customer( currentTime );

                if ( customer.getNumberOfItems() <= EXPRESS_MAX_NUM_ITEMS ) {
                    express.addCustomer( customer );
                } else  {
                    regular.addCustomer( customer );
                }
            }

            express.serveCustomers( currentTime );
            regular.serveCustomers( currentTime );

            currentTime += TICK;

        }

        display();
    }

    private void display() {

        System.out.println( "SIMULATION :: " );
        System.out.println( "The duration (in seconds) of the simulation was " + lengthOfSimulation + nl );

        System.out.println( "EXPRESS CHECKOUT LINE(S) :: " + nl );
        System.out.println( express );

        System.out.println( "REGULAR CHECKOUT LINE(S) :: " + nl );
        System.out.println( regular );

    }

    public static void main(String[] args) {

        int duration = SECONDS_PER_MINUTE * MINUTES_PER_HOUR * 8;
        Simulation sim = new Simulation( duration );
        sim.run();

    }
}
```
