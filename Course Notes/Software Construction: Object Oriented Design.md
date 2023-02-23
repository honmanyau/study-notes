# Software Construction: Object-Oriented Design

## Table of Contents

* [Software Construction: Object-Oriented Design](#software-construction-object-oriented-design)
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

* Strengthening preconditions-subclass should not accept a narrower range of inputs
* Weakening postconditions-subclass should fulfil the EFFECTS clauses specified in Server

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

* Composite Pattern, an OO solution for a hierarchical structure
* Observer Pattern, a solution for when one (or more) object(s) wants to watch the state of one (or more) other object(s)
* Iterator Pattern, which allows us to collect behaviour related to iterating over a collection

#### Composite

* Classic example folders and files
* Nodes with children (Composite) differ in behaviour to those without children (Leaf)
* Both composite and leaf extend a Component superclass
* Composite has a list of Component with an associated set method
* Without Component, Composite would have to have methods and fields for each type of Leaf/Composite (well, they wouldn't be Leaf and Composites anymore in that case!)

#### Long-form Problem Console Arcade - Monster Maze III

```Java
// src/model/Choice

package model;

public abstract class Choice {
    protected String optionMessage;

    public Choice(String optionMessage) {
        this.optionMessage = optionMessage;
    }

    public void printOptionMessage() {
        System.out.println(optionMessage);
    }
}
```

```Java
// src/model/Monster

package model;

public class Monster extends Choice {
    private Treasure treasure;

    public Monster() {
        super("Fight a monster.");
        treasure = null;
    }

    //MODIFIES: this
    //EFFECTS: sets the treasure to t
    public void setTreasure(Treasure t) {
        this.treasure = t;
    }

    //EFFECTS: prints the result of choosing this choice
    public void printOutcome() {
        if (treasure == null) {
            System.out.println("Ha! I killed you!");
        } else {
            System.out.println("Ahh! You killed me!");
            treasure.printOutcome();
        }
    }

}
```

```Java
// src/model/Treasure

package model;

public class Treasure extends Choice {
    private int prize;

    public Treasure(int prize) {
        super("Claim your treasure!");
        this.prize = prize;
    }

    //EFFECTS: prints the result of choosing this choice
    public void printOutcome() {
        System.out.println("Your prize is " + prize + " spendibees.");
    }

}
```

```Java
// src/model/Room

package model;

import java.util.ArrayList;
import java.util.List;

public class Room extends Choice {
    private List<Choice> choices;
    private int id;

    public Room(int id) {
        super("Enter Room " + id + ".");

        this.id = id;
        choices = new ArrayList<>();
    }

    //EFFECTS: prints all possible next choices
    public void printNextChoices() {
        System.out.println("You are now in Room " + id + ".\n");
        System.out.println("You have the following options: ");

        int counter = 1;

        for (Choice c: choices) {
            System.out.print("\tOption " + counter + ": ");
            c.printOptionMessage();
            counter++;
        }
    }

    public void addChoice(Choice c) {
        choices.add(c);
    }

    //getters for gameplay
    public Choice getChoice(int i) {
        return choices.get(i);
    }
}
```

```Java
// src/ui/Game

package ui;

import model.Choice;
import model.Monster;
import model.Room;
import model.Treasure;

import java.util.Scanner;

public class Game {
    // ...

    //MODIFIES: this
    //EFFECTS: displays the next option chosen from the options displayed by current room
    private void printNextChoiceById(int id) {
        System.out.println("Nya: " + (id + 1));

        Choice c = current.getChoice(id);

        if (c instanceof Room) {
            current = (Room) c;
            return;
        }
        // Further abstracting Monster and Treasure as Objects can reduce repetition here
        else if (c instanceof Monster) {
            Monster m = (Monster) c;
            m.printOutcome();
            roundOver = true;
            return;
        }
        else if (c instanceof Treasure) {
            Treasure m = (Treasure) c;
            m.printOutcome();
            roundOver = true;
            return;
        }
        else {
            System.out.println(INVALID_CHOICE);
        }
    }

    // ...
}
```

```Java
// src/ui/MonsterMaze

// ...

public class MonsterMaze {

    public static void main(String[] args) throws InterruptedException {
        // ...

        m3.setTreasure(t1);

        r1.addChoice(r2);
        r1.addChoice(r4);
        r1.addChoice(m1);
a
        r2.addChoice(r3);
        r2.addChoice(r6);

        r3.addChoice(m3);

        r4.addChoice(r5);

        r5.addChoice(m2);

        r6.addChoice(t1);

        Game g = new Game(r1);
    }
}
```

#### Observer Pattern

> The Observer Pattern is a design that lets one or more objects watch (Observer) the state of one or more other objects (Subject)

(MobX?)

#### Console Arcade - Bingo II

```Java
// src/model/PlayingCard

package model;

import model.observer_pattern.Observer;

// ...

public class PlayerCard implements Observer {
    private List<NumberSquare> numbers;
    private List<Collection<Integer>> colIndices;
    private List<Collection<Integer>> rowIndices;
    private List<Collection<Integer>> diagonalIndices;
    private int numberSquaresStamped;
    private boolean hasBingo;

    public PlayerCard(){
        numbers = new ArrayList<>();
        populateIndices();

        for(int i=0; i < CARD_SIZE; i++){
            numbers.add(new NumberSquare());
        }
    }

    @Override
    public void update(Object o) {
        BingoNumber bc = (BingoNumber) o;
        int i = numberSquaresMatch(bc);
        for (int j=0; j < i; j++) {
            int index = getSquareIndexOfNextUnstamped(bc);
            stampSquare(index);
            checkIfBingo(index);
        }
    }

    // ...
}
```

```Java
// src/model/Game

// ...

public class Game extends Subject {

    public static final int CARD_SIZE = 25;
    public static final int SIDE_LENGTH = (int) Math.sqrt(CARD_SIZE);

    private BingoNumber currentCall;
    private boolean gameOver;

    public Game() {
        super();

        callNext();
    }

    // ...

    public List<PlayerCard> getCards() {
        List<PlayerCard> playerCards = new ArrayList<>();
        for (Observer o : this.getObservers()) {
            if (o.getClass().getSimpleName().equals("PlayerCard"))
                playerCards.add((PlayerCard) o);
        }
        return playerCards;
    }

    public void callNext() {
        currentCall = new BingoNumber();
        notifyObservers();
        refreshGameOver();
    }

    public void addCard(Observer pc) {
        this.addObserver(pc);
    }

    public void refreshGameOver(){
        for (Observer o : this.getObservers()) {
            PlayerCard p = (PlayerCard) o;
            if (p.hasBingo()) {
                gameOver = true;
                break;
            }
        }
    }

    @Override
    public void notifyObservers() {
        for (Observer o: this.getObservers()) {
            o.update(currentCall);
        }
    }
}
```

#### Basic Iterator Pattern

> The Iterator Pattern allows us to separate out all the logic for iterating over a collection

#### Long-form Problem: Console Arcade

```Java
// src/model/SillyWordGame

package model;

import java.util.Iterator;
import java.util.List;

public class SillyWordGame implements Iterable<Phrase> {

    private List<Phrase> phrases;

    public SillyWordGame(List<Phrase> phrases) {
        this.phrases = phrases;
    }

    public List<Phrase> getAllPhrases() {
        return phrases;
    }

    @Override
    public Iterator<Phrase> iterator() {
        return new PhraseIterator();
    }

    private class PhraseIterator implements Iterator<Phrase> {
        Iterator<Phrase> it = phrases.iterator();
        Phrase p;

        @Override
        public boolean hasNext() {
            return it.hasNext();
        }

        @Override
        public Phrase next() {
            p = (Phrase) it.next();

            while(!p.needsWord()) {
                p = (Phrase) it.next();
            }

            return p;
        }
    }
}
```

```Java
// src/ui/SillyWordGame

package ui;

import model.SillyWordGame;
import model.Phrase;
import model.words.WordEntry;

import java.util.Iterator;
import java.util.Scanner;

public class SillyWordGameUI {

    private SillyWordGame wordGame;
    private Scanner s;

    public static void main(String[] args) {
        new SillyWordGameUI(new PhraseResource());
    }

    public SillyWordGameUI(PhraseResource pr) {
        s = new Scanner(System.in);
        wordGame = new SillyWordGame(pr.generatePhraseList());
        System.out.println(wordGame);
        userInteraction();
        printSillyGame();
    }

    //MODIFIES: this
    //EFFECTS: fills each needed word entry with user input
    private void userInteraction(){
        System.out.println("NYA!");
        for (Phrase p: wordGame) {
            WordEntry w = p.getNeededWordEntry();
            printWordInputDescription(w);

            String input = "";
            while (input.length() == 0) {
                if (s.hasNext())
                    input = s.nextLine();
            }
            input = input.trim();

            p.fillWordEntry(input);
        }
//        }
    }

    //EFFECTS: prints out all phrases in this game
    private void printSillyGame() {
        for(Phrase p : wordGame.getAllPhrases())
            System.out.println(p.toString());
    }

    //EFFECTS: prints out the correct description for the next word needed
    private void printWordInputDescription(WordEntry w) {
        StringBuilder str = new StringBuilder(w.getType().getInstructions());
        String desc = w.getDescription();
        if (desc.length() > 0) {
            str.append(" (" ).append(desc).append( ")");
        }
        str.append(": ");
        System.out.println(str.toString());
    }
}
```

### Final Project

```Java
// src/filters/AndFilter

package filters;

import twitter4j.Status;

import java.util.ArrayList;
import java.util.List;

/**
 * A filter that represents the logical and of its child filters
 */
public class AndFilter implements Filter {
    private final Filter child1;
    private final Filter child2;

    public AndFilter(Filter child1, Filter child2) {
        this.child1 = child1;
        this.child2 = child2;
    }

    /**
     * An and filter matches when both of it its children match
     * @param s     the tweet to check
     * @return      whether or not it matches
     */
    @Override
    public boolean matches(Status s) {
        return child1.matches(s) && child2.matches(s);
    }

    @Override
    public List<String> terms() {
        ArrayList<String> terms = new ArrayList<String>();
        terms.addAll(child1.terms());
        terms.addAll(child2.terms());

        return terms;
    }

    public String toString() {
        return "(" + child1.toString() + " and " + child2.toString() + ")";
    }
}
```

```Java
// src/filters/OrFilter
package filters;

import twitter4j.Status;

import java.util.ArrayList;
import java.util.List;

/**
 * A filter that represents the logical and of its child filters
 */
public class OrFilter implements Filter {
    private final Filter child1;
    private final Filter child2;

    public OrFilter(Filter child1, Filter child2) {
        this.child1 = child1;
        this.child2 = child2;
    }

    /**
     * An and filter matches when both of it its children match
     * @param s     the tweet to check
     * @return      whether or not it matches
     */
    @Override
    public boolean matches(Status s) {
        return child1.matches(s) || child2.matches(s);
    }

    @Override
    public List<String> terms() {
        ArrayList<String> terms = new ArrayList<String>();
        terms.addAll(child1.terms());
        terms.addAll(child2.terms());

        return terms;
    }

    public String toString() {
        return "(" + child1.toString() + " or " + child2.toString() + ")";
    }
}
```

```Java
// src/filters/Parser

public class Parser {
    // ...

    private Filter orexpr() throws SyntaxError {
        Filter sub = andexpr();
        String token = scanner.peek();
        while (token != null && token.equals(OR)) {
            scanner.advance();
            Filter right = andexpr();
            // At this point we have two subexpressions ("sub" on the left and "right" on the right)
            // that are to be connected by "or"
            // TODO: Construct the appropriate new Filter object
            // The new filter object should be assigned to the variable "sub"
            sub = new OrFilter(sub, right); ////
            token = scanner.peek();
        }
        return sub;
    }

    private Filter andexpr() throws SyntaxError {
        Filter sub = notexpr();
        String token = scanner.peek();
        while (token != null && token.equals(AND)) {
            scanner.advance();
            Filter right = notexpr();
            // At this point we have two subexpressions ("sub" on the left and "right" on the right)
            // that are to be connected by "and"
            // TODO: Construct the appropriate new Filter object
            // The new filter object should be assigned to the variable "sub"
            sub = new AndFilter(sub, right); ////
            token = scanner.peek();
        }
        return sub;
    }

    //...
}
```

```Java
// src/filters/test/TestFilters

// ...

public class TestFilters {
    // ...

    @Test void testAnd() {
        Filter f = new AndFilter(new BasicFilter("fred"), new BasicFilter("george"));
        assertFalse(f.matches(makeStatus("Fred Flintstone")));
        assertFalse(f.matches(makeStatus("George Flintstone")));
        assertTrue(f.matches(makeStatus("Fred George Flintstone")));
        assertTrue(f.matches(makeStatus("George Fredstone")));
    }

    @Test void testOr() {
        Filter f = new OrFilter(new BasicFilter("fred"), new BasicFilter("george"));
        assertFalse(f.matches(makeStatus("Gred Flintstone")));
        assertTrue(f.matches(makeStatus("Fred Flintstone")));
        assertTrue(f.matches(makeStatus("George Flintstone")));
    }

    // ...
}
```

```Java
// src/filters/TestParser

// ...

public class TestParser {
    // ...

    @Test
    public void testAnd() throws SyntaxError {
        Filter x = new Parser("blue and green").parse();

        assertTrue(x.toString().equals("(blue and green)"));
    }

    @Test
    public void testNestedAnd() throws SyntaxError {
        Filter x = new Parser("blue and green and red").parse();

        assertTrue(x.toString().equals("((blue and green) and red)"));
    }

    @Test
    public void testOr() throws SyntaxError {
        Filter x = new Parser("blue or green").parse();

        assertTrue(x.toString().equals("(blue or green)"));
    }

    @Test
    public void testNestedOr() throws SyntaxError {
        Filter x = new Parser("blue or green or red").parse();

        assertTrue(x.toString().equals("((blue or green) or red)"));
    }

    // ...
}
```

```Java
// src/twitter/TwitterSource

// ...

public abstract class TwitterSource extends Observable {
    // ...

    protected void handleTweet(Status s) {
        setChanged();
        notifyObservers(s);
    }
}

```

```Java
// src/query/Query

package query;

import filters.Filter;
import org.openstreetmap.gui.jmapviewer.Coordinate;
import org.openstreetmap.gui.jmapviewer.JMapViewer;
import org.openstreetmap.gui.jmapviewer.Layer;
import org.openstreetmap.gui.jmapviewer.interfaces.MapMarker;
import twitter.TwitterSource;
import twitter4j.Status;
import ui.DummyImage;
import ui.MapMarkerInteresting;
import ui.MapMarkerSimple;
import util.Util;

import javax.imageio.ImageIO;
import javax.swing.*;
import java.awt.*;
import java.io.IOException;
import java.net.MalformedURLException;
import java.net.URL;
import java.util.ArrayList;
import java.util.Observable;
import java.util.Observer;

public class Query implements Observer {
    // ...

    public void terminate() {
        this.setVisible(false);
        map.repaint();
        layer = null;
        checkBox = null;
    }

    @Override
    public void update(Observable o, Object arg) {
        Status s  = (Status) arg;
        DummyImage di = new DummyImage();
        Image image = di.getImage();

        if (filter.matches(s)) {
            Coordinate coor = Util.statusCoordinate(s);
            MapMarker marker = new MapMarkerInteresting(this.layer, coor, color, s.getText(), image);
            map.addMapMarker(marker);
        }
    }
}
```

```Java
// src/ui/Application

package ui;

import org.openstreetmap.gui.jmapviewer.Coordinate;
import org.openstreetmap.gui.jmapviewer.JMapViewer;
import org.openstreetmap.gui.jmapviewer.Layer;
import org.openstreetmap.gui.jmapviewer.interfaces.ICoordinate;
import org.openstreetmap.gui.jmapviewer.interfaces.MapMarker;
import org.openstreetmap.gui.jmapviewer.tilesources.BingAerialTileSource;
import query.Query;
import sun.applet.Main;
import twitter.PlaybackTwitterSource;
import twitter.TwitterSource;
import util.SphericalGeometry;

import javax.imageio.ImageIO;
import javax.swing.*;
import javax.xml.bind.DatatypeConverter;
import java.awt.*;
import java.awt.event.MouseAdapter;
import java.awt.event.MouseEvent;
import java.awt.image.RenderedImage;
import java.io.ByteArrayOutputStream;
import java.io.IOException;
import java.io.OutputStream;
import java.util.*;
import java.util.List;
import java.util.Timer;

/**
 * The Twitter viewer application
 * Derived from a JMapViewer demo program written by Jan Peter Stotz
 */
public class Application extends JFrame {
    public void addQuery(Query query) {
        queries.add(query);
        Set<String> allterms = getQueryTerms();
        twitterSource.setFilterTerms(allterms);
        contentPanel.addQuery(query);

        twitterSource.addObserver(query);
    }

    // ...

    public Application() {
        // ...

        // Set up a motion listener to create a tooltip showing the tweets at the pointer position
        map().addMouseMotionListener(new MouseAdapter() {
            @Override
            public void mouseMoved(MouseEvent e) {
                // https://docs.oracle.com/javase/tutorial/uiswing/components/html.html
                // https://www.java2s.com/Tutorial/Java/0240__Swing/Useimagesintooltips.htm
                Point p = e.getPoint();
                ICoordinate pos = map().getPosition(p);
                // TODO: Use the following method to set the text that appears at the mouse cursor
                String tooltipText = "<html><b>";
                List<MapMarker> coveredMarkers = getMarkersCovering(pos, pixelWidth(p));

                for (MapMarker m: coveredMarkers) {
                    MapMarkerInteresting im = (MapMarkerInteresting) m;
                    String s = im.getDescription();
                    Image i = im.getImage();

                    tooltipText += "<p><img src=\"https://redacted.com/redacted.png\" height=\"40\" width=\"40\"  />&nbsp;";
                    tooltipText += s + "</p>";
                }

                map().setToolTipText(tooltipText + "</b></html>");
            }
        });
    }

    //...

    // A query has been deleted, remove all traces of it
    public void terminateQuery(Query query) {
        twitterSource.deleteObserver(query);
        queries.remove(query);
        Set<String> allterms = getQueryTerms();
        twitterSource.setFilterTerms(allterms);
    }
}
```

```Java
// src/ui/DummyImage

package ui;

import javax.imageio.ImageIO;
import java.awt.*;
import java.io.IOException;
import java.net.MalformedURLException;
import java.net.URL;

public class DummyImage {
    private Image image;

    public DummyImage() {
        try {
            this.image = ImageIO.read(new URL("https://redacted.com/redacted.png"));
        } catch (MalformedURLException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    public Image getImage() {
        return this.image;
    }
}
```

```Java
// src/ui/MapMarkerInteresting

package ui;

import org.openstreetmap.gui.jmapviewer.Coordinate;
import org.openstreetmap.gui.jmapviewer.Layer;
import org.openstreetmap.gui.jmapviewer.MapMarkerCircle;

import java.awt.*;

public class MapMarkerInteresting extends MapMarkerCircle {
    public static final double defaultMarkerSize = 16.0;
    private String description;
    private Image image;

    public MapMarkerInteresting(Layer layer, Coordinate coord, Color color, String description, Image image) {
        super(layer, null, coord, defaultMarkerSize, STYLE.FIXED, getDefaultStyle());
        setColor(Color.BLACK);
        setBackColor(color);
        this.description = description;
        this.image = image;
    }

    // Getters
    public String getDescription() {
        return this.description;
    }

    public Image getImage() {
        return this.image;
    }

    @Override
    public void paint(Graphics g, Point position, int radius) {
        int size = radius * 2;
        int halfSize = size / 2;
        int quarterSize = halfSize / 2;

        if (g instanceof Graphics2D && this.getBackColor() != null) {
            Graphics2D g2 = (Graphics2D)g;
            Composite oldComposite = g2.getComposite();
            g2.setComposite(AlphaComposite.getInstance(3));
            g2.setPaint(this.getBackColor());
            g.fillOval(position.x - radius, position.y - radius, size, size);
            g2.setComposite(oldComposite);
        }

        g.setColor(this.getColor());
        g.drawOval(position.x - radius, position.y - radius, size, size);
        if (this.getLayer() == null || this.getLayer().isVisibleTexts()) {
            this.paintText(g, position);
        }

        if (!this.image.equals(null)) {
            g.drawImage(this.image, position.x - quarterSize, position.y - quarterSize, halfSize, halfSize, null);
        }
    }
}
```

**Remarks**

Quite a lot of time was wasted on trying to figure out how Java works than learning design patterns. Twitter4J doesn't have an HTTPS-enabled site at the time of writing that really bothered me to have to blindly trust those `.jar` files.

JMapViewer is okay but, as with using library in other languages, if one doesn't know the fundamental of a language well then much time is spent on fighting with getting things done the hard way—getting the tooltip to work ate up a lot more time than I would have liked, in the process of getting it to work, I often thought about how active the JavaScript community is—it's not easy to find something that hasn't been answered properly before (the GitHub source code of JMapViewer also appears to be a few years old, that's... kind of unthinkable for a useful library in JavaScript)

The images URL for the mocked Twitter doesn't work anymore, a placeholder image is used so that I don't have to set up yet another zombie Twitter app that I'm never going to tend to (also, it doesn't really add any value in terms of what I really wanted to learn from the course).

It was a bit of an anticlimax for an otherwise mostly excellent course.

## Resources
