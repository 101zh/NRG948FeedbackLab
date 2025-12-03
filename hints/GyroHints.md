# Hint for getting the angle off gyro

## Hint 1

<details>
    <summary>Click to reveal hint!</summary>

Get the angle off the variable below.

```java
StatusSignal<Angle> boardRotation;
```

</details>

## Hint 2

<details>
    <summary>Click to reveal hint!</summary>

Get the angle off the `boardRotation` by getting the value as a double.

</details>

## Hint 3

<details>
    <summary>Click to reveal hint!</summary>

Remember to refresh the variable, otherwise you aren't getting the most recent result.

</details>

## Hint 4

<details>
    <summary>Click to reveal hint!</summary>

The gyro doesn't wrap its values from 0º to 360º. How do you remove extra 360 degrees?


<details>
    <summary>Answer</summary>

Using `%`, will help you.

For example `361 % 360 = 1`, removing the extra 360º

</details>

</details>
