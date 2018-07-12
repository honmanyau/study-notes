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





## Resources
