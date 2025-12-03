# Hint for getting the angle off the motor encoder

## Hint 1

<details>
    <summary>Click to reveal hint!</summary>

Get the angle off the variable below.

```java
RelativeEncoder neoMotorEncoder;
```

</details>

## Hint 2

<details>
    <summary>Click to reveal hint!</summary>

Get the "position" off the relative encoder.

</details>

## Hint 3

<details>
    <summary>Click to reveal hint!</summary>

Getting the position from the motor actually gives you rotations. How do you convert rotations to degrees?

<details>
    <summary>Answer</summary>

Multiply the position value you get in rotations by `360` will convert from rotations to degrees

</details>

</details>

## Hint 4

<details>
    <summary>Click to reveal hint!</summary>

The relative encoder doesn't wrap its values from 0º to 360º. How do you remove extra 360 degrees?


<details>
    <summary>Answer</summary>

Using `%`, will help you.

For example `361 % 360 = 1`, removing the extra 360º

</details>

</details>
