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









## Resources
