# Software Construction: Data Abstraction

## Table of Contents

* [Software Construction: Data Abstraction](#software-construction-data-abstraction)
  * [Disclaimer](#disclaimer)
  * [Notes](#notes)

## Disclaimer

Everything in this file is derived from going through [Software Construction: Data Abstraction](https://www.edx.org/course/software-construction-data-abstraction-ubcx-softconst1x) and is my own work. It is meant for revision and reference purposes and I take no responsibilities for any damaged caused by the use of it (see [LICENSE](https://github.com/honmanyau/study-notes/blob/master/LICENSE.md)).

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

#### Specifying and Implementing Interfaces

Interfaces represents data abstraction and only deals with the public specification of the data abstraction—no implementation.

> When a class implements an interface, it must include an implementation for the entire interface; that way users of the class can guarantee they will find all of I's behaviour in C. However, it is free to implement other methods that are not specified in the interface. Interfaces do not include implementations.

#### Apparent and Actual Types

When passing an argument, its type must be the same, or is a subtype of, the type declared in the parameter. This is known as **Subtype Polymorphism**.

#### Subclasses & Extends, Pt. 3 (Method Overriding)

Uwah... method overloading looks like a potential landmine.

### Calendar Long Form Project

```Java
// src/model/Calendar

package model;

import java.util.ArrayList;

public class Calendar {
    private Date date;
    private String email;
    private ArrayList<Entry> entries;

    public Calendar(Date date, String email) {
        this.date = date;
        this.email = email;
        this.entries = new ArrayList<Entry>();
    }

    public Date getCurrentDate() {
        return date;
    }

    public String getEmail() {
        return email;
    }

    public ArrayList<Entry> getEntries() {
        return entries;
    }

    public void addEntry(Entry event) {
        entries.add(event);
    }

    public ArrayList<Entry> getAllMeetingsByType(String label) {
        ArrayList<Entry> results = new ArrayList<Entry>();

        for (Entry entry: entries) {
            if (entry.getLabel() == label) {
                results.add(entry);
            }
        }

        return results;
    }
}
```

```Java
// src/model/Date

package model;

import java.util.ArrayList;
import java.util.Arrays;

public class Date {
    private int year;
    private int month;
    private int day;
    private String strYear;
    private String strMonth;
    private String strDay;
    private ArrayList<String> monthStrings;

    // REQUIRES: 1 <= day <= 31 (further restrictions depending on month applies),
    //           1 <= month <= 12
    public Date(int year, int month, int day) {
        this.year = year;
        this.month = month;
        this.day = day;
        this.strYear = String.valueOf(year);
        this.strMonth = String.valueOf(month);
        this.strDay = String.valueOf(day);
        this.monthStrings = new ArrayList<>(Arrays.asList(
                "January", "February", "March", "April", "May", "June",
                "July", "August", "September", "October", "November", "December"
        ));
    }

    public int getDay() {
        return day;
    }

    public int getMonth() {
        return month;
    }

    public int getYear() {
        return year;
    }

    public String getFormattedDate() {
        return strYear + "-" + strMonth + "-" + strDay;
    }

    public String getLongFormattedDate() {
        return monthStrings.get(month - 1) + " " + strDay + ", " + strYear;
    }
}
```

```Java
// src/model/Entry

package model;

public abstract class Entry {
    private Date date;
    private Time time;
    private String label;
    private String description;
    private Boolean repeat;
    private String repeatInterval;

    public Entry(Date date, Time time, String label, String description) {
        this.date = date;
        this.time = time;
        this.label = label;
        this.description = description;
        this.repeat = false;
        this.repeatInterval = "no-repeat";
    }

    public Date getDate() {
        return date;
    }

    public Time getTime() {
        return time;
    }

    public String getLabel() {
        return label;
    }

    public String getDescription() { return description; }

    public void toggleRepeat() {
        this.repeat = !repeat;
    }

    public Boolean isRepeating() {
        return repeat;
    }

    // REQUIRES: interval must be a String with a value of
    //           "daily", "weekly", "monthly" or "yearly"
    public void setRepeatInterval(String interval) {
        this.repeatInterval = interval;
    }

    public String getRepeatInterval() {
        return repeatInterval;
    }
}
```

```Java
// src/model/Event

package model;

public class Event extends Entry {
    private Reminder reminder;

    public Event(Date date, Time time, String label, String description) {
        super(date, time, label, description);
    }

    // For simplicity, a reminder is always set 00:00 the same day as the event
    public void setReminder() {
        Date date = super.getDate();
        Time time = new Time(0, 0);
        String label = "reminder";
        String description = super.getDescription();

        this.reminder = new Reminder(date, time, label, description);
    }

    public Reminder getReminder() {
        return reminder;
    }
}
```

```Java
// src/model/Meeting

package model;

import java.util.ArrayList;

public class Meeting extends Event {
    private ArrayList<String> emails;

    public Meeting(Date date, Time time, String label, String description) {
        super(date, time, label, description);

        this.emails = new ArrayList<String>();
    }

    public void addAttendees(String email) {
        if (!emails.contains(email)) {
            this.emails.add(email);
        }
    }

    public void removeAttendee(String email) {
        this.emails.remove(email);
    }

    public ArrayList<String> getAttendees() {
        return emails;
    }

    public void emailInvitations() {
        String inviting = "Inviting: ";
        String people = "";

        if (emails.size() > 0) {
            for (String email: emails) {
                people += email + ", ";
            }
        }
        else {
            people = "--nobody--";
        }

        people = people.replaceAll(", $", "");

        System.out.print(inviting + people + ".");
    }
}
```

```Java
// src/model/Reminder

package model;

public class Reminder extends Entry {
    private String note;

    public Reminder(Date date, Time time, String label, String description) {
        super(date, time, label, description);

        this.note = "";
    }

    public void setNote(String note) {
        this.note = note;
    }

    public String getNote() {
        return note;
    }
}
```

```Java
// src/model/Time

package model;

public class Time {
    private int hour;
    private int minute;
    private String strHour;
    private String strMinute;

    private String zeroPad(int num) {
        String numStr = String.valueOf(num);

        if (numStr.length() == 1) {
            numStr = "0" + numStr;
        }

        return numStr;
    }

    // REQUIRES: 0 <= minute <= 59
    //           0 <= hour <= 23
    public Time(int hour, int minute) {
        this.hour = hour;
        this.minute = minute;
        this.strHour = zeroPad(hour);
        this.strMinute = zeroPad(minute);
    }

    public int getHour() {
        return hour;
    }

    public int getMinute() {
        return minute;
    }

    public String getTime() {
        return strHour + ":" + strMinute;
    }
}
```

```Java
// src/tests/CalendarTest

package tests;

import model.*;
import org.junit.Before;
import org.junit.Test;

import java.util.ArrayList;
import java.util.Arrays;

import static org.junit.Assert.assertEquals;

public class CalendarTest {
    private Date date, dateTm;
    private Time time, timeTr, timeTe, timeTm;
    private String email;
    private Calendar tc;
    private Reminder tr;
    private Event te;
    private Meeting tm;

    @Before
    public void setup() {
        email = "rin@nadeshiko.party";

        date = new Date(2018, 3, 4);
        dateTm = new Date(2018, 3, 3);
        timeTr = new Time(0, 0);
        timeTe = new Time(17, 0);
        timeTm = new Time(12, 0);


        tc = new Calendar(date, email);
        tr = new Reminder(date, timeTr,"reminder", "Nadeshiko's Birthday!");
        te = new Event(date, timeTe,"event","Nadeshiko's Birthday Party.");
        tm = new Meeting(dateTm, timeTm,"meeting", "Final Meeting for Nadeshiko's Surprise Party.");
    }

    @Test
    public void testGetCurrentDate() {
        assertEquals(date, tc.getCurrentDate());
    }

    @Test
    public void testGetEmail() {
        assertEquals(email, tc.getEmail());
    }

    @Test
    public void testGetEntries() {
        assertEquals(0, tc.getEntries().size());

        tc.addEntry(te);

        assertEquals(1, tc.getEntries().size());
    }

    @Test
    public void testAddEntry() {
        tc.addEntry(te);

        assertEquals(new ArrayList<>(Arrays.asList(te)), tc.getEntries());

        tc.addEntry(tm);

        assertEquals(new ArrayList<>(Arrays.asList(te, tm)), tc.getEntries());

        tc.addEntry(tr);

        assertEquals(new ArrayList<>(Arrays.asList(te, tm, tr)), tc.getEntries());
    }

    @Test
    public void testGetAllMeetingsByType() {
        tc.addEntry(te);
        tc.addEntry(te);
        tc.addEntry(tm);
        tc.addEntry(te);
        tc.addEntry(tm);
        tc.addEntry(tr);
        tc.addEntry(tr);

        assertEquals(new ArrayList<>(Arrays.asList(te, te, te)), tc.getAllMeetingsByType("event"));
        assertEquals(new ArrayList<>(Arrays.asList(tm, tm)), tc.getAllMeetingsByType("meeting"));
        assertEquals(new ArrayList<>(Arrays.asList(tr, tr)), tc.getAllMeetingsByType("reminder"));
    }
}
```

```Java
// src/tests/DateTest

package tests;

import model.Date;
import org.junit.Before;
import org.junit.Test;

import static org.junit.Assert.assertEquals;

public class DateTest {
    private Date td1, td2, td3, td4;

    @Before
    public void setup() {
        td1 = new Date(2001, 1, 1);
        td2 = new Date(1982, 2, 22);
        td3 = new Date(1997, 7, 7);
        td4 = new Date(2031, 12, 31);
    }

    @Test
    public void testGetDay() {
        assertEquals(1, td1.getDay());
        assertEquals(31, td4.getDay());
    }

    @Test
    public void testGetMonth() {
        assertEquals(1, td1.getMonth());
        assertEquals(12, td4.getMonth());
    }

    @Test
    public void testGetYear() {
        assertEquals(2001, td1.getYear());
        assertEquals(2031, td4.getYear());
    }

    @Test
    public void testGetFormattedDate() {
        assertEquals("1982-2-22", td2.getFormattedDate());
        assertEquals("1997-7-7", td3.getFormattedDate());
    }

    @Test
    public void testGetLongFormattedDate() {
        assertEquals("January 1, 2001", td1.getLongFormattedDate());
        assertEquals("December 31, 2031", td4.getLongFormattedDate());
    }
}
```

```Java
// src/tests/EventTest

package tests;

import model.Date;
import model.Event;
import model.Time;
import org.junit.Before;
import org.junit.Test;

import static org.junit.Assert.assertEquals;
import static org.junit.Assert.assertNotNull;
import static org.junit.Assert.assertNull;

public class EventTest {
    private Date date;
    private Time time;
    private Event te;

    @Before
    public void setup() {
        date = new Date(2018, 3, 4);
        time = new Time(17, 0);
        te = new Event(date, time, "event", "Nadeshiko's Birthday Party.");
    }

    @Test
    public void testGetReminder() {
        assertNull(te.getReminder());

        te.setReminder();

        assertNotNull(te.getReminder());
    }

    @Test
    public void testSetReminder() {
        te.setReminder();

        assertEquals(te.getDate().getFormattedDate(), te.getReminder().getDate().getFormattedDate());
        assertEquals(17, te.getTime().getHour());
        assertEquals(0, te.getTime().getMinute());
        assertEquals("reminder", te.getReminder().getLabel());
        assertEquals("Nadeshiko's Birthday Party.", te.getReminder().getDescription());
    }
}
```

```Java
// src/tests/ExtendedEntryTest

package tests;

import model.Date;
import model.Reminder;
import model.Time;
import org.junit.Before;
import org.junit.Test;

import static org.junit.Assert.assertEquals;

public class ExtendedEntryTest {
    private Reminder tr;
    private Date date;
    private Time time;

    @Before
    public void setup() {
        date = new Date(2018, 3, 4);
        time = new Time(0, 0);
        tr = new Reminder(date, time, "event", "Nadeshiko's Birthday!");
    }

    @Test
    public void testGetDate() {
        assertEquals(date, tr.getDate());
    }

    @Test
    public void testGetTime() {
        assertEquals(time, tr.getTime());
    }

    @Test
    public void testGetLabel() {
        assertEquals("event", tr.getLabel());
    }

    @Test
    public void testGetDescription() {
        assertEquals("Nadeshiko's Birthday!", tr.getDescription());
    }

    @Test
    public void testIsRepeating() {
        assertEquals(false, tr.isRepeating());
    }

    @Test
    public void testToggleRepeating() {
        tr.toggleRepeat();

        assertEquals(true, tr.isRepeating());

        tr.toggleRepeat();

        assertEquals(false, tr.isRepeating());
    }

    @Test
    public void testGetRepeatInterval() {
        assertEquals("no-repeat", tr.getRepeatInterval());
    }

    @Test
    public void testSetRepeatInterval() {
        tr.setRepeatInterval("yearly");

        assertEquals("yearly", tr.getRepeatInterval());
    }
}
```

```Java
// src/tests/MeetingTest

package tests;

import model.Date;
import model.Meeting;
import model.Time;
import org.junit.After;
import org.junit.Before;
import org.junit.Test;

import java.io.ByteArrayOutputStream;
import java.io.PrintStream;
import java.util.ArrayList;
import java.util.Arrays;

import static org.junit.Assert.assertEquals;
import static org.junit.Assert.assertNotEquals;

public class MeetingTest {
    private Date date;
    private Time time;
    private Meeting tm;
    private ArrayList<String> tarr1, tarr2, tarr3;
    private ByteArrayOutputStream outContent;
    private PrintStream originalOut;

    @Before
    public void setup() {
        date = new Date(2018, 3, 3);
        time = new Time(12, 0);
        tm = new Meeting(date, time, "meeting", "Final Meeting for Nadeshiko's Surprise Party.");

        tarr1 = new ArrayList<>(Arrays.asList("rin@nadeshiko.party"));
        tarr2 = new ArrayList<>(Arrays.asList("rin@nadeshiko.party", "shizuka@nadeshiko.party"));
        tarr3 = new ArrayList<>(Arrays.asList("shizuka@nadeshiko.party"));

        // https://stackoverflow.com/questions/1119385/junit-test-for-system-out-println
        outContent = new ByteArrayOutputStream();
        System.setOut(new PrintStream(outContent));
        originalOut = System.out;
    }

    @Test
    public void testGetAttendees() {
        assertEquals(0, tm.getAttendees().size());

        tm.addAttendees("rin@nadeshiko.party");

        assertNotEquals(0, tm.getAttendees().size());
    }

    @Test
    public void testAddAttendees() {
        assertEquals(0, tm.getAttendees().size());

        tm.addAttendees("rin@nadeshiko.party");

        assertEquals(tarr1, tm.getAttendees());

        tm.addAttendees("shizuka@nadeshiko.party");

        assertEquals(tarr2, tm.getAttendees());

        tm.addAttendees("rin@nadeshiko.party");
        tm.addAttendees("shizuka@nadeshiko.party");

        assertEquals(tarr2, tm.getAttendees());
    }

    @Test
    public void testRemoveAttendee() {
        tm.addAttendees("rin@nadeshiko.party");
        tm.addAttendees("shizuka@nadeshiko.party");

        tm.removeAttendee("chiaki@nadeshiko.party");

        assertEquals(tarr2, tm.getAttendees());

        tm.removeAttendee("rin@nadeshiko.party");

        assertEquals(tarr3, tm.getAttendees());

        tm.removeAttendee("shizuka@nadeshiko.party");

        assertEquals(new ArrayList<String>(), tm.getAttendees());
    }

    @Test
    public void testEmailInvitation() {
        tm.emailInvitations();

        assertEquals("Inviting: --nobody--.", outContent.toString());

//        tm.addAttendees("rin@nadeshiko.party");
//        tm.emailInvitations();
//
//        assertEquals("Inviting: rin@nadeshiko.party.", outContent.toString());
//
//        tm.addAttendees("shizuka@nadeshiko.party");
//        tm.emailInvitations();
//
//        assertEquals("Inviting: rin@nadeshiko.party, shizuka@nadeshiko.party.", outContent.toString());
    }

    @Test
    public void testEmailInvitation2() {
        tm.addAttendees("rin@nadeshiko.party");
        tm.emailInvitations();

        assertEquals("Inviting: rin@nadeshiko.party.", outContent.toString());
    }

    @After
    public void restore() {
        System.setOut(originalOut);
    }
}
```

```Java
// src/tests/ReminderTest

package tests;

import model.Date;
import model.Reminder;
import model.Time;
import org.junit.Before;
import org.junit.Test;

import static org.junit.Assert.assertEquals;

public class ReminderTest {
    private Reminder tr;
    private Date date;
    private Time time;

    @Before
    public void setup() {
        date = new Date(2018, 3, 4);
        time = new Time(0, 0);
        tr = new Reminder(date, time,"reminder", "Nadeshiko's Birthday Party.");
    }

    @Test
    public void testGetNote() {
        assertEquals("", tr.getNote());
    }

    @Test
    public void testSetNote() {
        tr.setNote("Wish Nadeshiko Happy Birthday.");

        assertEquals("Wish Nadeshiko Happy Birthday.", tr.getNote());
    }
}
```

```Java
// src/tests/TimeTest

package tests;

import model.Time;
import org.junit.Before;
import org.junit.Test;

import static org.junit.Assert.assertEquals;

public class TimeTest {
    private Time tt1, tt2, tt3, tt4;

    @Before
    public void setup() {
        tt1 = new Time(0, 0);
        tt2 = new Time(23, 59);
    }

    @Test
    public void testGetHour() {
        assertEquals(0, tt1.getHour());
        assertEquals(23, tt2.getHour());
    }

    @Test
    public void testGetMinute() {
        assertEquals(0, tt1.getMinute());
        assertEquals(59, tt2.getMinute());
    }

    @Test
    public void testGetTime() {
        assertEquals("00:00", tt1.getTime());
        assertEquals("23:59", tt2.getTime());
    }
}
```

```Java
// src/main/Main

package main;

import model.*;

public class Main {
    private final static Date calendarDate = new Date(2018,1, 1);
    private final static String calendarEmail = "rin@nadeshiko.party";
    private static Calendar calendar;


    public static void main(String[] args) {
        calendar = new Calendar(calendarDate, calendarEmail);

        System.out.println("Calendar owner: " + calendar.getEmail());
        System.out.println("Current date: " + calendar.getCurrentDate().getLongFormattedDate());

        addEntiresToCalendar();

        // "Upcoming" is not actually implemented—the current date just happens to be
        // before all events in the calendar.
        System.out.println("\nUpcoming meetings: ");
        for (Entry entry: calendar.getAllMeetingsByType("meeting")) {
            Meeting meeting = (Meeting) entry;
            String repeating = meeting.isRepeating().toString();

            if (meeting.isRepeating()) {
                repeating = meeting.getRepeatInterval();
            }

            System.out.println("===" + meeting.getDescription().replaceAll("\\.$", "") + "===");
            System.out.println(meeting.getDate().getFormattedDate() + ", " + meeting.getTime().getTime());
            System.out.println("Attendees: " + meeting.getAttendees().toString());
            System.out.println("Repeating: " + repeating);
        }

        System.out.println("\nUpcoming events: ");
        for (Entry entry: calendar.getAllMeetingsByType("event")) {
            Event event = (Event) entry;
            String repeating = event.isRepeating().toString();

            if (event.isRepeating()) {
                repeating = event.getRepeatInterval();
            }

            System.out.println("===" + event.getDescription().replaceAll("\\.$", "") + "===");
            System.out.println(event.getDate().getFormattedDate() + ", " + event.getTime().getTime());
            System.out.println("Repeating: " + repeating);
        }

        System.out.println("\nReminders: ");
        for (Entry entry: calendar.getAllMeetingsByType("reminder")) {
            Reminder reminder = (Reminder) entry;
            String repeating = reminder.isRepeating().toString();

            if (reminder.isRepeating()) {
                repeating = reminder.getRepeatInterval();
            }

            System.out.println("===" + reminder.getDescription().replaceAll("\\.$", "") + "===");
            System.out.println(reminder.getDate().getFormattedDate() + ", " + reminder.getTime().getTime());
            System.out.println("Repeating: " + repeating);
        }


        // You should use as many different types of Entry getters as you designed in
        // You should also use your Meeting’s sendInvites() method.
    }

    public static void addEntiresToCalendar() {
        Date partyDate = new Date(2018, 3, 4);
        Date partyPlanningDate = new Date(2018, 3, 3);
        Time partyTime = new Time(17,0);
        Time partyPlanningTime = new Time(12,0);
        Time partyReminderTime = new Time(0,0);
        Meeting meeting = new Meeting(partyPlanningDate, partyPlanningTime, "meeting", "Final Meeting for Nadeshiko's Surprise Party.");
        Reminder reminder = new Reminder(partyDate, partyReminderTime, "reminder", "Nadeshiko's Birthday Party.");
        Event event = new Event(partyDate, partyTime, "event", "Nadeshiko's Birthday Party.");

        meeting.addAttendees(calendarEmail);
        meeting.addAttendees("shizuka@nadeshiko.party");
        meeting.addAttendees("aoi@nadeshiko.party");
        meeting.addAttendees("chiaki@nadeshiko.party");

        event.toggleRepeat();
        event.setRepeatInterval("yearly");
        event.setReminder();

        calendar.addEntry(meeting);
        calendar.addEntry(reminder);
        calendar.addEntry(event);
    }
}
```

#### Quiz

> In Java, multiple inheritance is not possible.

```Java
// src/model/Shape

// getters
public int getX() { return x; }

public int getY() { return y; }

public int getWidth() { return width; }

public int getHeight() { return height; }

public MidiSynth getMidiSynth() {
    return midiSynth;
}

public int getInstrument() {
    return instrument;
}

// EFFECTS: return a velocity based on the area of a Shape
//          The only meaningful velocities are between 0 and 127
//          Velocities less than 60 are too quiet to be heard
public int areaToVelocity() {
    return Math.max(60, Math.min(127, calcArea() / 30));
}

// EFFECTS: maps a given integer to a valid associated note
public int coordToNote(int y) {
    return 70 - y / 12;
}

//EFFECTS: draws the shape
//    public void drawGraphics(Graphics g) {
//        g.drawRect(x, y, width, height);
//    }
public abstract void drawGraphics(Graphics g);

//EFFECTS: fills the shape
//    public void fillGraphics(Graphics g) {
//        g.fillRect(x, y, width, height);
//    }
public abstract void fillGraphics(Graphics g);

public abstract int calcArea();

public abstract boolean contains(Point point);
```

```Java
// src/model/Rectangle

package model;

import sound.MidiSynth;

import java.awt.*;

public class Rectangle extends Shape {
    private int x;
    private int y;

    public Rectangle(Point topLeft, MidiSynth midiSynth) {
        super(topLeft, midiSynth, 0, new Color(255, 139, 148));

        this.x = getX();
        this.y = getY();
    }

    public void drawGraphics(Graphics g) {
        g.drawRect(this.x, this.y, getWidth(), getHeight());
    }

    public void fillGraphics(Graphics g) {
        g.fillRect(this.x, this.y, getWidth(), getHeight());
    }

    public int calcArea() {
        return getWidth() * getHeight();
    }

    public boolean contains(Point p) {
        return containsX(p.x) && containsY(p.y);
    }
}
```

```Java
// src/model/Oval

package model;

import sound.MidiSynth;

import java.awt.*;

public class Oval extends Shape {
    private int x;
    private int y;

    public Oval(Point topLeft, MidiSynth midiSynth) {
        super(topLeft, midiSynth,10, new Color(168, 230, 207));

        this.x = getX();
        this.y = getY();
    }

    public void drawGraphics(Graphics g) {
        g.drawOval(x, y, getWidth(), getHeight());
    }

    public void fillGraphics(Graphics g) {
        g.fillOval(x, y, getWidth(), getHeight());
    }

    public int calcArea() {
        return (int) Math.floor(Math.PI * getWidth() / 2 * getHeight() / 2);
    }

    public boolean contains(Point p) {
        final double TOL = 1.0e-6;
        double halfWidth = getWidth() / 2.0;
        double halfHeight = getHeight() / 2.0;
        double diff = 0.0;

        if (halfWidth > 0.0) {
            diff = diff + sqrDiff(this.x + halfWidth, p.x) / (halfWidth * halfWidth);
        } else {
            diff = diff + sqrDiff(this.x + halfWidth, p.x);
        }
        if (halfHeight > 0.0) {
            diff = diff + sqrDiff(this.y + halfHeight, p.y) / (halfHeight * halfHeight);
        } else {
            diff = diff + sqrDiff(this.y + halfHeight, p.y);
        }
        return  diff <= 1.0 + TOL;
    }

    // Compute square of difference
    // EFFECTS: returns the square of the difference of num1 and num2
    private double sqrDiff(double num1, double num2) {
        return (num1 - num2) * (num1 - num2);
    }
}
```

```Java
// src/ui/tools/ShapeTools

	//EFFECTS: Constructs and returns the new shape
	private void makeShape(MouseEvent e) {
		shape = makeSpecificShape(e.getPoint(), editor.getMidiSynth());
	}

    //EFFECTS: Returns the string for the label.
//    private String getLabel() {
//        return "Rectangle";
//    }
  public abstract String getLabel();

	public abstract Shape makeSpecificShape(Point topLeft, MidiSynth midiSynth);

```

```Java
// src/ui/tools/RectangleTool

package ui.tools;

import model.Shape;
import model.Rectangle;
import sound.MidiSynth;
import ui.DrawingEditor;

import javax.swing.*;
import java.awt.*;

public class RectangleTool extends ShapeTool {
    public RectangleTool(DrawingEditor editor, JComponent parent) {
        super(editor, parent);
    }

    public String getLabel() {
        return "Rectangle";
    }

    public Shape makeSpecificShape(Point topLeft, MidiSynth midiSynth) {
        return new Rectangle(topLeft, midiSynth);
    }
}
```

```Java
// src/ui/tools/OvalTool

package ui.tools;

import model.Oval;
import model.Shape;
import sound.MidiSynth;
import ui.DrawingEditor;

import javax.swing.*;
import java.awt.*;

public class OvalTool extends ShapeTool {
    public OvalTool(DrawingEditor editor, JComponent parent) {
        super(editor, parent);
    }

    public String getLabel() {
        return "Oval";
    }

    public Shape makeSpecificShape(Point topLeft, MidiSynth midiSynth) {
        return new Oval(topLeft, midiSynth);
    }
}
```

```Java
// src/ui/DrawingEditor

private void createTools() {
	JPanel toolArea = new JPanel();
	toolArea.setLayout(new GridLayout(0,1));
	toolArea.setSize(new Dimension(0, 0));
	add(toolArea, BorderLayout.SOUTH);

      ShapeTool rectTool = new RectangleTool(this, toolArea);
      tools.add(rectTool);

      ShapeTool ovalTool = new OvalTool(this, toolArea);
      tools.add(ovalTool);

      MoveTool moveTool = new MoveTool(this, toolArea);
      tools.add(moveTool);

      ResizeTool resizeTool = new ResizeTool(this, toolArea);
      tools.add(resizeTool);

      DeleteTool deleteTool = new DeleteTool(this, toolArea);
      tools.add(deleteTool);

      PlayShapeTool playShapeTool = new PlayShapeTool(this, toolArea);
	tools.add(playShapeTool);

      PlayDrawingTool playDrawingTool = new PlayDrawingTool(this, toolArea);
      tools.add(playDrawingTool);

      setActiveTool(rectTool);
}
```
