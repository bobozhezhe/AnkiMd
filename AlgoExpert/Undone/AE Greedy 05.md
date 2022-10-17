# AlgoExpert

## Greedy 05 Valid Starting City

### Question

Imagine you have a set of cities that are laid out in a circle, connected by a circular road that runs clockwise. Each city has a gas station that provides gallons of fuel, and each city is some distance away from the next city.

You have a car that can drive some number of miles per gallon of fuel, and your goal is to pick a starting city such that you can fill up your car with that city's fuel, drive to the next city, refill up your car with that city's fuel, drive to the next city, and so on and so forth until you return back to the starting city with 0 or more gallons of fuel left.

This city is called a valid starting city, and it's guaranteed that there will always be exactly one valid starting city.

For the actual problem, you'll be given an array of distances such that city i is distances[i] away from city i + 1. Since the cities are connected via a circular road, the last city is connected to the first city. In other words, the last distance in the distances array is equal to the distance from the last city to the first city. You'll also be given an array of fuel available at each city, where fuel[i] is equal to the fuel available at city i. The total amount of fuel available (from all cities combined) is exactly enough to travel to all cities. Your fuel tank always starts out empty, and you're given a positive integer value for the number of miles that your car can travel per gallon of fuel (miles per gallon, or MPG). You can assume that you will always be given at least two cities.

Write a function that returns the index of the valid starting city.

### Sample Input

distances = [5, 25, 15, 10, 15]
fuel = [1, 2, 1, 0, 3]
mpg = 10

### Sample Output

4

### Optimal Space & Time Complexity

O(n) time | O(1) space - where n is the number of cities

%

### Key Point

### Solution 1

```java
class Program {

  // O(n^2) time | O(1) space - where n is the number of cities
  public int validStartingCity(int[] distances, int[] fuel, int mpg) {
    int numberOfCities = distances.length;

    for (int startCityIdx = 0; startCityIdx < numberOfCities; startCityIdx++) {
      int milesRemaining = 0;

      for (int currentCityIdx = startCityIdx;
          currentCityIdx < startCityIdx + numberOfCities;
          currentCityIdx++) {
        if (milesRemaining < 0) {
          continue;
        }

        int currentCityIdxRotated = currentCityIdx % numberOfCities;

        int fuelFromCurrentCity = fuel[currentCityIdxRotated];
        int distanceToNextCity = distances[currentCityIdxRotated];
        milesRemaining += fuelFromCurrentCity * mpg - distanceToNextCity;
      }

      if (milesRemaining >= 0) {
        return startCityIdx;
      }
    }

    // This line should never be reached if the inputs are correct.
    return -1;
  }
}

```

### Solution 2

```java
class Program {

  // O(n) time | O(1) space - where n is the number of cities
  public int validStartingCity(int[] distances, int[] fuel, int mpg) {
    int numberOfCities = distances.length;
    int milesRemaining = 0;

    int indexOfStartingCityCandidate = 0;
    int milesRemainingAtStartingCityCandidate = 0;

    for (int cityIdx = 1; cityIdx < numberOfCities; cityIdx++) {
      int distanceFromPreviousCity = distances[cityIdx - 1];
      int fuelFromPreviousCity = fuel[cityIdx - 1];
      milesRemaining += fuelFromPreviousCity * mpg - distanceFromPreviousCity;

      if (milesRemaining < milesRemainingAtStartingCityCandidate) {
        milesRemainingAtStartingCityCandidate = milesRemaining;
        indexOfStartingCityCandidate = cityIdx;
      }
    }

    return indexOfStartingCityCandidate;
  }
}

```
