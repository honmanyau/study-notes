# Software Engineering: Introduction

## Table of Contents

* [Software Construction: Data Abstraction](#software-construction-data-abstraction)
  * [Disclaimer](#disclaimer)
  * [Notes](#notes)

## Disclaimer

Everything in this file is derived from going through [Software Construction: Object-Oriented Design](https://www.edx.org/course/software-construction-object-oriented-ubcx-softconst2x) and is my own work. It is meant for revision and reference purposes and I take no responsibilities for any damaged caused by the use of it (see [LICENSE](https://github.com/honmanyau/study-notes/blob/master/LICENSE.md)). If you are stupid enough to plagiarise my work then I hope you get caught and fail at everything else in life.

## Notes

### 5: Designing Robust Classes

#### Coffeemakder Long-form Project

```Java
// src/Main

package main;

import exceptions.BeansAmountException;
import exceptions.NoCupsRemainingException;
import exceptions.StaleCoffeeException;
import exceptions.WaterException;
import model.CoffeeMaker;

public class Main {
    private static CoffeeMaker nadeshiko;

    public static void main(String[] args){
        nadeshiko = new CoffeeMaker();

        // Nadeshiko successfully brewed coffee
        try {
            nadeshiko.brew(2.50, 20);
        }
        catch (BeansAmountException e) {
            // Should never get here
        }
        catch (WaterException e) {
            // Should never get here
        }

        // Nadeshiko pours freshly made coffee
        try {
            nadeshiko.setCupsRemaining(20);
            nadeshiko.setTimeSinceLastBrew(1);
            nadeshiko.pourCoffee();
        }
        catch (NoCupsRemainingException e) {
            // Should never get here
        }
        catch (StaleCoffeeException e) {
            // Should never get here
        }

        System.out.println("\n");

        // Nadeshiko used too few beans!
        try {
            nadeshiko.brew(2.30, 20);
        }
        catch (BeansAmountException e) {
            System.out.println("===Exception===");
            System.out.println(e.getErrorMessage());
        }
        catch (WaterException e) {
            // Should never get here
        }

        // Nadeshiko used too many beans!
        try {
            nadeshiko.brew(2.70, 20);
        }
        catch (BeansAmountException e) {
            System.out.println("===Exception===");
            System.out.println(e.getErrorMessage());
        }
        catch (WaterException e) {
            // Should never get here
        }

        // Nadeshiko forgot to fill the water back up!
        try {
            nadeshiko.brew(2.50, 2);
        }
        catch (BeansAmountException e) {
            // Should never get here
        }
        catch (WaterException e) {
            System.out.println("===Exception===");
            System.out.println(e.getErrorMessage());
        }

        // Nadeshiko is trying to pour coffee without cups!
        try {
            nadeshiko.setCupsRemaining(0);
            nadeshiko.pourCoffee();
        }
        catch (NoCupsRemainingException e) {
            System.out.println("===Exception===");
            System.out.println(e.getErrorMessage());
        }
        catch (StaleCoffeeException e) {
            // Should never get here
        }

        // Nadeshiko is trying to pour stale coffee!
        try {
            nadeshiko.setCupsRemaining(5);
            nadeshiko.pourCoffee();
        }
        catch (NoCupsRemainingException e) {
            // Should never get here
        }
        catch (StaleCoffeeException e) {
            System.out.println("===Exception===");
            System.out.println(e.getErrorMessage());
        }
    }
}
```

```Java
// src/moel/Coffeemaker

package model;

import exceptions.*;

/**
 * A coffee maker used to train baristas.
 *
 * Class invariant: cups remaining >= 0, time since last brew >= 0
 */

public class CoffeeMaker {
    private int timeSinceLastBrew;
    private int cupsRemaining;
    private double water;

    public CoffeeMaker(){
        this.timeSinceLastBrew = 0;
        this.cupsRemaining = 0;
        this.water = 0;
    }

    // getters
    public int getTimeSinceLastBrew() {
        return this.timeSinceLastBrew;
    }
    public int getCupsRemaining() {
        return this.cupsRemaining;
    }

    // EFFECTS: return true if there are coffee cups remaining
    public boolean areCupsRemaining() {
        return this.cupsRemaining > 0;
    }

    //REQUIRES: a non-negative integer
    //EFFECTS: sets time since last brew
    public void setCupsRemaining(int cups) {
        this.cupsRemaining = cups;
    }

    //REQUIRES: a non-negative integer
    //EFFECTS: sets time since last brew
    public void setTimeSinceLastBrew(int time) {
        this.timeSinceLastBrew = time;
    }

    //REQUIRES: beans between 2.40 and 2.60 cups, water > 14.75 cups
    //EFFECTS: sets cups remaining to full (20 cups) and time since last brew to 0
    public void brew(double beans, double water) throws BeansAmountException, WaterException {
        if (beans < 2.40) {
            throw new TooFewBeansException(beans);
        }

        if (beans > 2.60) {
            throw new TooManyBeansException(beans);
        }

        if (water <= 14.75) {
            throw new WaterException(water);
        }

        System.out.println("Successfully brewed coffee!");

        this.cupsRemaining = 20;
        this.timeSinceLastBrew = 0;

        System.out.println("Refilled cups and reset time since last brew to 0!");
    }

    ///REQUIRES: cups remaining > 0, time since last brew < 60
    //MODIFIES: this
    //EFFECTS: subtracts one cup from cups remaining
    public void pourCoffee() throws NoCupsRemainingException, StaleCoffeeException {
        if (this.cupsRemaining == 0) {
            throw new NoCupsRemainingException();
        }

        if (this.timeSinceLastBrew > 2) {
            throw new StaleCoffeeException(this.timeSinceLastBrew);
        }

        System.out.println("Successfully poured coffee!");
    }
}
```

```Java
// src/exceptions/BeansAmountException

package exceptions;

public class BeansAmountException extends Exception {
    private double beans;
    private String errorMessage;

    protected BeansAmountException(double beans, String message){
        super(String.valueOf(beans) + " " + message);

        this.beans = beans;
        this.errorMessage = String.valueOf(beans) + " " + message;
    }

    public double getBeans() {
        return beans;
    }

    public String getErrorMessage() {
        return this.errorMessage;
    }
}
```

```Java
// src/exceptions/NoCupsRemainingException

package exceptions;

public class NoCupsRemainingException extends Exception {
    private String errorMessage;

    public NoCupsRemainingException () {
        super("No cups remaining!");

        this.errorMessage = "No cups remaining!";
    }

    public String getErrorMessage() {
        return this.errorMessage;
    }
}
```

```Java
// src/exceptions/StaleCoffeeException

package exceptions;

public class StaleCoffeeException extends Exception {
    private String errorMessage;

    public StaleCoffeeException (int timeSinceLastBrew) {
        super("The coffee is stale! Time since last brew: " + String.valueOf(timeSinceLastBrew));

        this.errorMessage = "The coffee is stale! Time since last brew: " + String.valueOf(timeSinceLastBrew);
    }

    public String getErrorMessage() {
        return this.errorMessage;
    }
}
```

```Java
// src/exceptions/TimeSinceLastBrewException

package exceptions;

public class TimeSinceLastBrewException extends Exception {
    private String errorMessage;

    public TimeSinceLastBrewException(double time) {
        super("Time since last brew cannot be negative! Time given: " + String.valueOf(time));

        this.errorMessage = "Time since last brew cannot be negative! Time given: " + String.valueOf(time);
    }

    public String getErrormessage() {
        return this.errorMessage;
    }
}
```

```Java
// src/exceptions/TooFewBeansException

package exceptions;

import exceptions.BeansAmountException;

public class TooFewBeansException extends BeansAmountException {
    public TooFewBeansException(double beans) {
        super(beans, " cups is too few beans!");
    }
}
```

```Java
// src/TooManyBeansException

package exceptions;

import exceptions.BeansAmountException;

public class TooManyBeansException extends BeansAmountException {
    public TooManyBeansException(double beans) {
        super(beans, " cups is too many beans!");
    }
}
```

```Java
// src/exceptions/WaterException

package exceptions;

public class WaterException extends Exception {
    private double amountOfWater; // unit cups
    private String errorMessage;

    public WaterException (double amountOfWater) {
        super(String.valueOf(amountOfWater) + " is too much water!");

        this.errorMessage = String.valueOf(amountOfWater) + " cups is too little water!";
    }

    public double getAmountOfWater() {
        return this.amountOfWater;
    }

    public String getErrorMessage() {
        return this.errorMessage;
    }
}
```

### 6: Extracting Object-Oriented Design

#### Extracting Associations

* Aggregration (empty diamond before arrow)
* Bidirectional relationship—no arrow head

#### Extracting Sequence Diagrams

#### Quiz

Select each of the following that describes an aggregation relationship (as opposed to a general association):

[x] A Body that has fields for different body parts correct
[ ] A Country that has fields for the countries it has trade relationships with
[x] A House that has fields for its rooms correct
[x] A Book that has fields for each of its chapters correct
[ ] A University Faculty that has fields for the other faculties at the university it interacts with

What's shown above is the "correct" answer but I tend to disagree (not because I picked the other two that are "incorrect"). The "correct" answers are **compositions** and not **aggregations** (the book example is ambiguous)—a body part cannot exist without a body (well, in a natural sort of sense—this becomes debatable if we consider things that biotechnology have managed), a rooms cannot exist without a container (otherwise they would not be rooms). Conceptually a brain can exist without a body (Psycho Pass!?) and a room can exist without a container (Chrono Trigger!?), and I suppose the latter is a counter-argument to my argument (WAAAAAH), but... you get the idea! It's at best ambiguous!

Time to find a corner and hide.

### 7: Implementing Object-Oriented Design

#### PetPairs Long-form Problem

```Java
// src/model/pets/Pet

public class Pet {
    // ...
    protected Human human;

    public Pet(String species, String color, boolean friendly, boolean needsAttention, double price){
        // ...
        this.human = null;
    }

    public void adoptHuman(Human human) {
        System.out.println("Adopting a human!");

        if (human != null && !human.hasPet(this)){
            human.adoptPet(this);
            System.out.println("Success! Adopted " + human);
        }
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (!(o instanceof Pet)) return false;
        Pet pet = (Pet) o;
        return Objects.equals(species, pet.species) &&
                Objects.equals(color, pet.color);
    }

    @Override
    public int hashCode() {

        return Objects.hash(species, color);
    }
}
```

```Java
// src/model/Human

// ...

import java.util.ArrayList;

public class Human {
    // ...
    private ArrayList<Pet> pets = new ArrayList<Pet>();

    // ...
    //EFFECTS: returns the number of pets belonging to species
    public int numPetsOfSpecies(String species) {
        int num = 0;

        for (Pet p : pets) {
            if (p.getSpecies() == species) {
                num++;
            }
        }

        return num;
    }
}
```

```Java
// src/model/PetStore

// ...

import java.lang.reflect.Array;
import java.util.ArrayList;
import java.util.Collection;
import java.util.HashMap;
import java.util.List;

public class PetStore {
    private HashMap<String, ArrayList<Pet>> animals = new HashMap<>();

    // ...

    //EFFECTS: prints out all pets in the store matching given attributes
    public void displayAllPetsWithAttributes(boolean friendly, boolean needsAttention, double price) {
        Collection<ArrayList<Pet>> allPets = animals.values();

        for (ArrayList<Pet> species : allPets) {
            displayOneSpeciesWithAttributes(species, friendly, needsAttention, price);
        }
    }

    //EFFECTS: prints out all pets of this species matching given attributes
    public void displayOneSpeciesWithAttributes(List<Pet> petList, boolean friendly, boolean needsAttention, double price) {
        for (Pet p : petList) {
            if (
                p.isFriendly() == friendly &&
                p.needsAttention() == needsAttention &&
                p.getPrice() <= price
            ) {
                System.out.println("Has attributes: " + p);
            }
        }
    }
}
```

### 8: Design Principles

#### What are Design Principles?

* Single Responsibiliy Principle
* Listkov Principle

#### Single Responsibility Principle

Each class/function should code that is as cohesive as possible.

#### Busy'sDiner 1 Long-form Problem

```Java
// src/model/Chef

package model;

import java.util.ArrayList;
import java.util.List;

public class Chef {
    private static final double DISH_PRICE = 10.00;
    private static final String PREFIX = "CHEF - ";

    private Order order;

    public Chef() {
        this.order = null;
    }

    //MODIFIES: this, order
    //EFFECTS: makes food and logs order as prepared
    public void makeDish(Order order) {
        this.order = order;

        prepareIngredients();
        followRecipe();
        cookFood();
        plate(order);
    }

    //EFFECTS: prints out a doing dishes message
    public void doDishes() {
        System.out.println(PREFIX + "Cleaning, scrubbing...");
        System.out.println("Dishes done.");
    }

    //EFFECTS: prints out the ingredients being prepared
    private void prepareIngredients() {
        System.out.println(PREFIX + "Slicing tomatoes... Shredding lettuce...");
    }

    //EFFECTS: prints out the recipe being followed
    private void followRecipe() {
        System.out.println(PREFIX + "Stacking meat... Placing veggies.... ");
    }

    //EFFECTS: prints out a message about cooking food
    private void cookFood() {
        System.out.println(PREFIX + "Grilling sandwich...");
    }

    //MODIFIES: order
    //EFFECTS: logs order as prepared and prints out a plating message
    private void plate(Order order) {
        order.setIsPrepared();
        System.out.print(PREFIX + "Plated order: ");
        order.print();

        this.order = null;
    }

}
```

```Java
// src/model/Server

package model;

import java.util.ArrayList;
import java.util.List;

public class Server {

    private static final double DISH_PRICE = 10.00;
    private static final String PREFIX = "SERVER - ";

    private List<Order> orders;
    private double cash;
    private int currentOrderNumber;

    public Server() {
        this.orders = new ArrayList<>();
        currentOrderNumber = 100;
    }


    //getter
    public List<Order> getActiveOrders() {
        return orders;
    }

    public double getCash() { return cash; }

    //MODIFIES: this
    //EFFECTS: creates new order with the dish on the menu
    public Order takeOrder() { //5: signature
        System.out.println(PREFIX + "Taking order...");
        Order o = new Order("Turkey club sandwich", currentOrderNumber++);
        orders.add(o);
        System.out.print("Order taken: ");
        o.print();
        return o;
    }

    //EFFECTS: prints out a description of the dish on the menu
    public void describeDish() {
        System.out.println("\"Our somewhat bland sandwich has bread, lettuce, tomato, " +
                "cheddar cheese, turkey and bacon.\"");
    }

    //EFFECTS: prints out a greeting
    public void greet() {
        System.out.println("\"Hello and welcome to Busy's, the home of the mediocre turkey club sandwich.\"");
    }

    //MODIFIES: this
    //EFFECTS: takes payment for the guest and removes order from system
    public void takePayment(Order order) {
        System.out.println(PREFIX + "Taking payment...");
        orders.remove(order);
        cash += DISH_PRICE;
        System.out.println("\"Thanks for visiting Busy's Diner!\"");
    }

    //MODIFIES: this, order
    //EFFECTS: logs order as served and brings to table
    public void deliverFood(Order order) {
        order.setIsServed();
        System.out.print(PREFIX + "Delivered food: ");
        order.print();
    }
}
```

```Java
// src/ui/Diner

package ui;

import model.Chef;
import model.Server;
import model.Order;

public class Diner {

    public static void main(String[] args) {
        Server server = new Server();
        Chef chef = new Chef();

        for (int i=0; i < 2 ; i++) {
            System.out.println("Table " + (i + 1) + ":\n");

            server.greet();
            server.describeDish();
            Order o = server.takeOrder();

            System.out.println();
            chef.makeDish(o);

            doOrderRoutine(server, o);
            System.out.println();
        }

        System.out.println();
        chef.doDishes();
    }

    private static void doOrderRoutine(Server e, Order o) {
        System.out.println();
        if (o.isReadyToBeServed())
            e.deliverFood(o);
        if(o.isReadyToBePaid())
            e.takePayment(o);
    }

}
```

#### Coupling

Relationship between modules (for example, if changes are made to one class, how large would the effect be another class that has a relationship with it).

#### Busy'sDiner 2 Long-form Problem

```Java
// src/model/Chef

package model;

public class Chef {

    private static final String PREFIX = "CHEF - ";

    private Order order;

    public Chef() {
        order = null;
    }

    //MODIFIES: this, order
    //EFFECTS: makes food and logs order as prepared
    public void makeDish(Order order) {
        this.order = order;

        prepareIngredients();
        followRecipe();
        cookFood();
        plate();
    }

    //EFFECTS: prints out a doing dishes message
    public void doDishes() {
        System.out.println(PREFIX + "Cleaning, scrubbing...");
        System.out.println("Dishes done.");
    }

    //EFFECTS: prints out the ingredients being prepared
    private void prepareIngredients() {
        for (String ingredient : this.order.getIngredients()) {
            System.out.println(PREFIX + "preparing " + ingredient);
        }
    }

    //EFFECTS: prints out the recipe being followed
    private void followRecipe() {
        System.out.println(PREFIX + "following receipe:");
        System.out.print(this.order.getRecipe());
    }

    //EFFECTS: prints out a message about cooking food
    private void cookFood() {
        System.out.println(PREFIX + "Grilling sandwich...");
    }

    //MODIFIES: order
    //EFFECTS: logs order as prepared and prints out a plating message
    private void plate() {
        order.setIsPrepared();
        System.out.print(PREFIX + "Plated order: ");
        order.print();
        this.order = null;
    }
}
```

```Java
// src/model/Dish

package model;

import java.util.ArrayList;

public class Dish {
    private String name;
    private String description;
    private ArrayList<String> ingredients;
    private String recipe;

    public Dish(String name) {
        this.name = "";
    }

    public Dish(String name, String description, ArrayList<String> ingredients, String recipe) {
        this.name = name;
        this.description = description;
        this.ingredients = ingredients;
        this.recipe = recipe;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getDescription() {
        return description;
    }

    public void setDescription(String description) {
        this.description = description;
    }

    public ArrayList<String> getIngredients() {
        return ingredients;
    }

    public void setIngredients(ArrayList<String> ingredients) {
        this.ingredients = ingredients;
    }

    public String getRecipe() {
        return recipe;
    }

    public void setRecipe(String recipe) {
        this.recipe = recipe;
    }
}
```

```Java
// src/model/Server

package model;

import java.util.ArrayList;
import java.util.List;

public class Server {

    private static final double DISH_PRICE = 10.00;
    private static final String PREFIX = "SERVER - ";

    private Dish dish;
    private List<Order> orders;
    private double cash;
    private int currentOrderNumber;

    public Server(Dish dish) {
        this.dish = dish;
        this.orders = new ArrayList<>();
        currentOrderNumber = 100;
    }


    //getter
    public List<Order> getActiveOrders() {
        return orders;
    }

    public double getCash() { return cash; }

    //MODIFIES: this
    //EFFECTS: creates new order with the dish on the menu
    public Order takeOrder() { //5: signature
        System.out.println(PREFIX + "Taking order...");
        Order o = new Order(dish, currentOrderNumber++);
        orders.add(o);
        System.out.print("Order taken: ");
        o.print();
        return o;
    }

    //EFFECTS: prints out a description of the dish on the menu
    public void describeDish() {
        System.out.println("\"Our somewhat bland sandwich has bread, lettuce, tomato, " +
                "cheddar cheese, turkey and bacon.\"");
    }

    //EFFECTS: prints out a greeting
    public void greet() {
        System.out.println("\"Hello and welcome to Busy's, the home of the mediocre turkey club sandwich.\"");
    }

    //MODIFIES: this
    //EFFECTS: takes payment for the guest and removes order from system
    public void takePayment(Order order) {
        System.out.println(PREFIX + "Taking payment...");
        orders.remove(order);
        cash += DISH_PRICE;
        System.out.println("\"Thanks for visiting Busy's Diner!\"");
    }

    //MODIFIES: this, order
    //EFFECTS: logs order as served and brings to table
    public void deliverFood(Order order) {
        order.setIsServed();
        System.out.print(PREFIX + "Delivered food: ");
        order.print();
    }

}
```

```Java
// src/ui/Diner

package ui;

import model.Chef;
import model.Dish;
import model.Server;
import model.Order;

import java.util.ArrayList;
import java.util.List;

public class Diner {

    public static void main(String[] args) {
        Server server = new Server(generateTurkeyClubSandwich());
        Chef chef = new Chef();

        for (int i=0; i < 2 ; i++) {
            System.out.println("Table " + (i + 1) + ":\n");

            server.greet();
            server.describeDish();
            Order o = server.takeOrder();

            System.out.println();
            chef.makeDish(o);

            doOrderRoutine(server, o);
            System.out.println();
        }

        System.out.println();
        chef.doDishes();
    }

    private static void doOrderRoutine(Server s, Order o) {
        System.out.println();
        if (o.isReadyToBeServed())
            s.deliverFood(o);
        if(o.isReadyToBePaid())
            s.takePayment(o);
    }

    private static Dish generateTurkeyClubSandwich() {
        ArrayList<String> ingredients = new ArrayList<>();
        ingredients.add("avocado");
        ingredients.add("sriracha");
        ingredients.add("cheddar cheese");
        ingredients.add("bread");
        ingredients.add("lettuce");
        ingredients.add("tomato");
        ingredients.add("turkey");
        ingredients.add("bacon");
        return new Dish("Turkey club sandwich",
                "\"Our trendy sandwich has avocado, sriracha sauce, cheese, veggies, turkey and bacon.\"",
                ingredients,
                "\t1. Pour sriracha\n\t2. Spread avocado\n\t3. Stack meat\n\t4. Place veggies");
    }
}
```

#### Liskov Substitution Principle

Whether one should substitute one subtype for another.

* Substitution Test:
  * Just try substituting a subtype for a supertype and see if it makes sense
* Concatenation Test:
  * Example: `StudentRegSys Student` (makes no sense), `Eagle bird` (makes sense)
* Is-a Test

* Postcondition—similar to what is described in the EFFECTS clause
* Preconditions—similar to what is described in the REQUIRES clause

* Strengthening preconditions-subclass accepts a narrower range of inputs
* Weakening postconditions-subclass cannot fulfill the EFFECTS clauses specified in Server

#### Busy'sDiner 3 Long-form Problem

```Java
// src/model/FOHEmployee

package model;

import java.util.ArrayList;
import java.util.List;

public abstract class FOHEmployee {
    private static final double DISH_PRICE = 10.00;

    protected Dish dish;

    public FOHEmployee(Dish dish) {
        this.dish = dish;
    }

    public abstract String getPrefix();

    //EFFECTS: prints out a description of the dish on the menu
    public void describeDish() {
        System.out.println(dish.getDescription());
    }

    //EFFECTS: prints out a greeting
    public void greet() {
        System.out.println("\"Hello and welcome to Busy's, the home of the trendy turkey club sandwich.\"");
    }

    //MODIFIES: this, order
    //EFFECTS: logs order as served and brings to table
    public void deliverFood(Order order) {
        order.setIsServed();
        System.out.print(getPrefix() + "Delivered food: ");
        order.print();
    }
}
```

```Java
// src/model/Host

package model;

public class Host extends FOHEmployee {
    private static final String ERROR = "ERROR!! ";
    private static final String PREFIX = "HOST - ";

    public Host(Dish dish) {
        super(dish);
    }

    public String getPrefix() {
        return PREFIX;
    }
}
```

```Java
// src/model/Server

package model;

import java.util.ArrayList;
import java.util.List;

public class Server extends FOHEmployee {
    private static final double DISH_PRICE = 10.00;
    private static final String PREFIX = "SERVER - ";

    private List<Order> orders;
    private double cash;
    private int currentOrderNumber;
    private Dish dish;

    public Server(Dish dish) {
        super(dish);

        this.orders = new ArrayList<>();
        currentOrderNumber = 100;
        this.dish = dish;
    }

    //getter
    public List<Order> getActiveOrders() {
        return orders;
    }

    public double getCash() { return cash; }

    public String getPrefix() {
        return PREFIX;
    }

    //MODIFIES: this
    //EFFECTS: creates new order with the dish on the menu
    public Order takeOrder() {
        System.out.println(PREFIX + "Taking order...");
        Order o = new Order(this.dish, currentOrderNumber++);
        orders.add(o);
        System.out.print("Order taken: ");
        o.print();
        return o;
    }

    //MODIFIES: this
    //EFFECTS: takes payment for the guest and removes order from system
    public void takePayment(Order order) {
        System.out.println(PREFIX + "Taking payment...");
        orders.remove(order);
        cash += DISH_PRICE;
        System.out.println("\"Thanks for visiting Busy's Diner!\"");
    }
}
```

```Java
// src/ui/Diner

// ...

public class Diner {
    public static void main(String[] args) {
        Dish dish = generateTurkeyClubSandwich();
        Server server = new Server(dish);
        Chef chef = new Chef();
        Host host = new Host(dish);

        //Table 1
        System.out.println("Table " + 1 + ":\n");

        server.greet();
        server.describeDish();
        Order o = server.takeOrder();

        System.out.println();
        chef.makeDish(o);

        doFOHOrderRoutine(server, o);
        doServerOrderRoutine(server, o);
        System.out.println();


        //Table 2
        System.out.println("Table " + 2 + ":\n");

        host.greet();
        host.describeDish();
        System.out.println();

        Order o2 = server.takeOrder();
        System.out.println();
        chef.makeDish(o2);

        doFOHOrderRoutine(host, o2);
        doServerOrderRoutine(server, o2);


        System.out.println();
        chef.doDishes();
    }

    public static void doFOHOrderRoutine(FOHEmployee e, Order o) {
        System.out.println();
        if (o.isReadyToBeServed())
            e.deliverFood(o);
    }

    public static void doServerOrderRoutine(Server s, Order o) {
        if(o.isReadyToBePaid())
            s.takePayment(o);
    }

    // ...
}
```

#### Quiz

Waaaah, looking at the feedbacks, it looks like I have mentally inverted what is and isn't a violation of preconditions and postconditions according to the Listkov Substitution Principle. D:

### 9: Design Patterns 







## Resources
