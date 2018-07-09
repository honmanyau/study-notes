# Software Engineering: Introduction

## Table of Contents

* [Software Construction: Data Abstraction](#software-construction-data-abstraction)
  * [Disclaimer](#disclaimer)
  * [Notes](#notes)
  * [Resources](#resources)

## Disclaimer

Everything in this file is derived from going through [Software Construction: Data Abstraction](https://www.edx.org/course/software-construction-data-abstraction-ubcx-softconst1x) and is my own work. It is meant for revision and reference purposes and I take no responsibilities for any damaged caused by the use of it (see [LICENSE](https://github.com/honmanyau/study-notes/blob/master/LICENSE.md)). If you are stupid enough to plagiarise my work then I hope you get caught and fail at everything else in life.

## Notes

### 1. Introduction to Software Construction

#### First Look

* Look and files and folders
* Run the program
* Relate the app to the code
* Inter-package relationship
* Inter-file relationship

### 2a. Control and Data Flow: Structures

#### Call Graphs

A graph showing what methods is called by other methods.

#### Classes and Objects

* Class is a way of encapsulating information
* A class contains the characteristics of the data it represents
* A class contains operations which can be performed on the data it represents (methods)

#### Anatomy of a Class

* `final`: immutable
* `static`: stays within a class and not instantiated with an object

**Style conventions**

1. Important statements
2. Class declaration
3. Constants and fields
4. Constructors
5. Public and private methods

### 2b: Control and Data Flow: Models

#### Data Flow

How information is passed around in a program.

#### Debugging, Part 3

> What is the main reason to avoid magic numbers?

> Because we don't do magic, we do computer science

Gold!

#### Quiz

> if the condition at A was changed to (!(condition at A)) then how would the flowchart for the method change: (Assume only labels are used for the flowchart)

That last bit about "Assume only labels are used for the flowchart" is misleading. D: The chart would reverse if we were not using labels, but if the label `A` is used to abstract away all the detail, as suggested by the provided assumption, then it does not matter if the conditions inside A is now the reverse of what it was before—because `A` will always evaluate to either `true` or `false` and the branches in the flow chart does not reverse...

### 3a: Data Abstraction: Specifying and Using

#### What is Abstraction?

* Hiding implementation detail behind an interface/container
* There are different levels of abstraction

Examples:

* Statement
* Functional
* Data abstraction
  * Classes

#### Visibility

Four phase process for designing and implementing abstraction:

1. Specify
2. Use
3. Test
4. Implement

#### Specifying a Data Abstraction

1. Requires clause—preconditions for correctness
2. Modifies clause—what does the method change
3. Effects clause—what does the method do

#### Requires

Examples:

* Requires: `amount >= 0`
* Requires: `y != 0`

#### Modifies

Examples:

* Modifies: `this` (implementation detail)
* Modifies: `this`, `deposits`

#### Effects

Examples:

* Effects: Adds `amount` fo `balance`

#### Using a Data Abstraction

Going through possible usage scenarios for how a data abstraction may be called by a client.

#### Discussion: Black Box and White Box Testing

> Black Box Testing is a way of testing software without knowing any of the internal structure or implementation details of the system being tested. In this case, we are interested in testing the functionality of the software.

> White Box Testing is a way of testing software that actually tests the internal structure and implementation details of the system.

#### JUnit, part 1 (\@Test and Fields)

* `@Test`
* `@Before`

#### Testability

### 3b: Data Abstraction: Testing and Implementing

#### Debugging Exercise

Setting a breakpoint at `line 50` of `FerryCardTest.java` and stepping over the execution suggests that the price for `testFerry` is never "set". Indeed, the constructor in `Ferry` sets `this.ticketPrice` to `0` instead of the `ticketPrice` passed in as an argument:

```Java
// src/model/Ferry.java

// Change:
this.ticketPrice = 0;

// To:
this.ticketPrice = ticketPrice;
```

It appears that 3 of the 5 bugs given are caused by this mistake.

Setting a breakpoint at `line 55` in `FerryCardTest.java` and stepping over the code indicates that the `buyTicket` method never deduct anything from `testFerryCard`'s `balance`. Solution:

```Java
// src/model/FerryCard.java

// Add to line 39
this.balance = balance - ticketPrice;
```

The last bug in `PassengerTest.java` is simply a typo on `line 20` of `PassengerTeset.java`, where `"Bruce Wayne"` is misspelt as `"Burce Wayne"`.

#### Implementing a Data Abstraction

Collection in Java:

* Collection
  * List
    * ArrayList
    * LinkedList
  * Set
    * HashSet
    * TreeSet

#### Student Transcript: Implement

```Java
// TranscriptTest.java

package tests;

import model.Transcript;
import org.junit.Before;
import org.junit.Test;

import static org.junit.Assert.assertEquals;

public class TranscriptTest {
    private static final double DELTA = 1e-15;
    private Transcript testTranscript;

    @Before
    public void setUp(){
        testTranscript = new Transcript("Nadeshiko Kagamihara", 1337);
    }

    @Test
    public void testGetStudentName() {
        assertEquals(testTranscript.getStudentName(), "Nadeshiko Kagamihara");
    }

    @Test
    public void testAddGrade() {
        // Valid cases
        testTranscript.addGrade("MISO 1001", 4.0);
        testTranscript.addGrade("MISO 1002", 2.0);
        testTranscript.addGrade("MISO 1003", 0);
        // Invalid cases
        testTranscript.addGrade("SOBA 1001", 5.0);
        testTranscript.addGrade("SOBA 1002", -1.0);

        assertEquals(testTranscript.getCourseAndGrade("MISO 1001"), "MISO 1001: 4.0");
        assertEquals(testTranscript.getCourseAndGrade("MISO 1002"), "MISO 1002: 2.0");
        assertEquals(testTranscript.getCourseAndGrade("MISO 1003"), "MISO 1003: 0.0");
        assertEquals(testTranscript.getCourseAndGrade("SOBA 1001"), "");
        assertEquals(testTranscript.getCourseAndGrade("SOBA 1002"), "");
    }

    @Test
    public void testGetCourseAndGrade() {
        assertEquals(testTranscript.getCourseAndGrade("MISO 1001"), "");

        testTranscript.addGrade("MISO 1001", 4.0);
        testTranscript.addGrade("MISO 1002", 2.0);
        testTranscript.addGrade("MISO 1003", 0);

        assertEquals(testTranscript.getCourseAndGrade("MISO 1001"), "MISO 1001: 4.0");
        assertEquals(testTranscript.getCourseAndGrade("MISO 1002"), "MISO 1002: 2.0");
        assertEquals(testTranscript.getCourseAndGrade("MISO 1003"), "MISO 1003: 0.0");
        assertEquals(testTranscript.getCourseAndGrade("SOBA 1001"), "");
        assertEquals(testTranscript.getCourseAndGrade("SOBA 1002"), "");
    }

    @Test
    public void testGetGPA() {
        testTranscript.addGrade("MISO 1001", 3.0);
        testTranscript.addGrade("MISO 1002", 3.0);
        testTranscript.addGrade("MISO 1003", 0);

        assertEquals(testTranscript.getGPA(), 2.0, DELTA);

        testTranscript.addGrade("MISO 1004", 4.0);

        assertEquals(testTranscript.getGPA(), 2.5, DELTA);
    }
}
```

```Java
// Transcript.java

package model;

//Jane Doe: CPSC-210: 3.5, ENGL-201: 4.0, CPSC-110: 3.1,
//        GPA: 3.533333333333333

import java.util.ArrayList;

/**
 * INVARIANT: course list and grade list are the same size
 * each course has a grade associated, and vice versa, at matching indices
 */
public class Transcript {
    private String name;
    private int id;
    private ArrayList<String> courses;
    private ArrayList<Double> gpas;

    public Transcript(String studentName, int studentID) {
        this.name = studentName;
        this.id = studentID;
        this.courses = new ArrayList<String>();
        this.gpas = new ArrayList<Double>();
    }

    public String getStudentName() {
        return name;
    }

    // REQUIRES: 0 <= grade && grade <= 4.0
    // MODIFIES: this
    // EFFECTS: Adds a new course and the student's GPA to the transcript
    public void addGrade(String course, double grade) {
        if (grade < 0 || grade > 4) {
            // Do nothing when grade is out of range.
            // Ideally this should be error handling.
        }
        else {
            courses.add(course);
            gpas.add(grade);
        }
    }

    // REQUIRES: a course the student has already taken
    // EFFECTS: Return a formatted string containing the course code given and
    //          the corresponding GPA for the given course code. If the course
    //          code given is invalid, return an error a message indicating
    //          that the course code given is invalid
    public String getCourseAndGrade(String course) {
        if (courses.contains(course)) {
            int index = courses.indexOf(course);
            double gpa = gpas.get(index);

            return course + ": " + String.valueOf(gpa);
        }

        return "";
    }

    // EFFECTS: List every course its corresponding GPA for the student
    public void printTranscript() { }

    // EFFECTS: Calculates and return the average of the GPAs of all courses
    public double getGPA() {
        int size = gpas.size();
        double sum = 0;

        for (Double num: gpas) {
            sum += num;
        }

        return sum / size;
    }
}
```

#### Quiz

```Java
// src/test/TransactionSummaryTest.java

package test;

import model.Transaction;
import model.TransactionSummary;
import org.junit.Before;
import org.junit.Test;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

import static model.TransactionType.*;
import static org.junit.Assert.assertEquals;
import static org.junit.Assert.assertTrue;

public class TransactionSummaryTest {

    private TransactionSummary testSummary;
    private Transaction t1, t2, t3, t4, t5, t6, t7, t8;

    @Before
    public void setUp() {
        testSummary = new TransactionSummary("Donald Knuth");
        t1 = new Transaction("Movie", "May 1st", 10, ENTERTAINMENT);
        t2 = new Transaction("Restaurant", "May 11th", 20, FOOD);
        t3 = new Transaction("Clothes", "May 9", 40, SHOPPING);
        t4 = new Transaction("Textbooks", "May 8", 150, EDUCATION);
        t5 = new Transaction("Korean Food", "May 11", 11, FOOD);
        t6 = new Transaction("Vancouver Symphony", "May 15", 40, ENTERTAINMENT);
        t7 = new Transaction("Cake", "May 4", 5, FOOD);
        t8 = new Transaction("Anime", "Oct 7", 77, ENTERTAINMENT);

        testSummary.addTransaction(t1);
        testSummary.addTransaction(t2);
        testSummary.addTransaction(t3);
        testSummary.addTransaction(t4);
        testSummary.addTransaction(t5);
        testSummary.addTransaction(t6);
        testSummary.addTransaction(t7);
    }

    @Test
    public void testConstructor() {
        assertEquals(testSummary.getName(), "Donald Knuth");
    }

    @Test
    public void testAddTransaction() {
        Transaction testTransaction = new Transaction("Test", "Test date", 0, FOOD);
        assertEquals(testSummary.getNumTransactions(), 7);
        testSummary.addTransaction(testTransaction);
        assertEquals(testSummary.getNumTransactions(), 8);
        assertTrue(testSummary.contains(testTransaction));
    }

    @Test
    public void testAverageTransactionCost() {
        TransactionSummary oneSummary = new TransactionSummary("Edsger Dijkstra");
        Transaction testTransaction = new Transaction("Food", "June 12", 5, FOOD);
        oneSummary.addTransaction(testTransaction);
        assertEquals(oneSummary.averageTransactionCost(), 5, 0.05);
        assertEquals(testSummary.averageTransactionCost(), (10 + 20 + 40 + 150 + 11 + 40 + 5)/7, 0.05);
    }

    @Test
    public void testLargestTransaction() {
        assertEquals(testSummary.largestTransaction(), t4);
        TransactionSummary oneSummary = new TransactionSummary("Edsger Dijkstra");
        Transaction testTransaction = new Transaction("Food", "June 12", 5, FOOD);
        oneSummary.addTransaction(testTransaction);
        assertEquals(oneSummary.largestTransaction(), testTransaction);
    }

    @Test
    public void testspecificTypeAverage() {
        assertEquals(testSummary.specificTypeAverage(FOOD), (11+5+20)/3, 0.05);
        assertEquals(testSummary.specificTypeAverage(EDUCATION), 150, 0.05);
    }

    @Test
    public void testGetTransactions() {
        List<Transaction> actualList = new ArrayList<>(Arrays.asList(t1, t2, t3, t4, t5, t6, t7));
        assertEquals(testSummary.getTransactions(), actualList);
    }

    @Test
    public void testContains() {
        assertEquals(testSummary.contains(t7), true);
        assertEquals(testSummary.contains(t8), false);
    }
}
```

```Java
// src/tests/TransactionTest.java

package test;

import model.Transaction;
import org.junit.Before;
import org.junit.Test;

import static model.TransactionType.EDUCATION;
import static model.TransactionType.ENTERTAINMENT;
import static org.junit.Assert.assertEquals;

public class TransactionTest {

    private Transaction t1, t4, t8;

    @Before
    public void setUp() {
        t1 = new Transaction("Movie", "May 1st", 10, ENTERTAINMENT);
        t4 = new Transaction("Textbooks", "May 8", 150, EDUCATION);
        t8 = new Transaction("Anime", "Oct 7", 77, ENTERTAINMENT);
    }

    // Transaction

    @Test
    public void testGetName() {
        assertEquals(t1.getName(), "Movie");
        assertEquals(t4.getName(), "Textbooks");
    }

    @Test
    public void testGetDate() {
        assertEquals(t1.getDate(), "May 1st");
        assertEquals(t4.getDate(), "May 8");
    }

    @Test
    public void testGetAmount() {
        assertEquals(t1.getAmount(), 10);
        assertEquals(t4.getAmount(), 150);
    }

    @Test
    public void testGetType() {
        assertEquals(t1.getType(), ENTERTAINMENT);
        assertEquals(t4.getType(), EDUCATION);
    }
}
```

```Java
// src/model/Transaction.java

package model;

public class Transaction {

    private String name;
    private String date;
    private int amount;
    private TransactionType type;

    public Transaction(String name, String date, int amount, TransactionType type) {
       this.name = name;
       this.date = date;
       this.amount = amount;
       this.type = type;
    }

    // getters
    public String getName() {
        return name;
    }
    public String getDate() {
        return date;
    }
    public int getAmount() {
        return amount;
    }
    public TransactionType getType() {
        return type;
    }


}
```

```Java
// src/model/TransactionSummary.java

package model;

import java.util.ArrayList;
import java.util.List;

public class TransactionSummary {

    private String name;
    private List<Transaction> transactions;

    public TransactionSummary(String name) {
        this.name = name;
        this.transactions = new ArrayList<Transaction>();
    }

    // getters
    public String getName() {
        return name;
    }
    public List<Transaction> getTransactions() {
        return transactions;
    }
    public int getNumTransactions() {
        return transactions.size();
    }

    // REQUIRES: t is not already within transactions
    // MODIFIES: this
    // EFFECTS: adds the given transaction t to the list of transactions
    public void addTransaction(Transaction t) {
        if (!transactions.contains(t)) {
            transactions.add(t);
        }
    }

    // REQUIRES: transactions is non-empty
    // EFFECTS: computes the average amount of money spent on a transaction
    public double averageTransactionCost() {
        int size = transactions.size();
        int sum = 0;

        if (size > 0) {
            for (Transaction t: transactions) {
                sum += t.getAmount();
            }

            return sum / size;
        }

        return sum;
    }

    // REQUIRES: transactions is non-empty
    // EFFECTS:  returns the average cost of all transactions of type specificType in this
    //           TransactionSummary
    public double specificTypeAverage(TransactionType specificType) {
        int size = transactions.size();
        int subsize = 0;
        int sum = 0;

        for (Transaction t: transactions) {
            if (t.getType() == specificType) {
                subsize++;
                sum += t.getAmount();
            }
        }

        if (subsize > 0) {
            return sum / subsize;
        }

        return sum;
    }


    // REQUIRES: transactions is non-empty
    // EFFECTS: returns the largest transaction (in terms of cost) in this TransactionSummary
    public Transaction largestTransaction() {
        int largest = 0;
        Transaction largestTransaction = null;

        for (Transaction t: transactions) {
            int a = t.getAmount();

            if (a > largest) {
                largest = a;
                largestTransaction =  t;
            }
        }

        return largestTransaction;
    }

    // EFFECTS: returns true if the given transaction is contained within the list of transactions
    public boolean contains(Transaction t) {
        return transactions.contains(t);
    }
}
```

### 4: Type Hierarchies, Polymorphism and Dispatching 








## Resources
