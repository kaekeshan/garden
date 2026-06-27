---
title: Computational Logics
description: General Purpose Algorithm Collections
draft: false
tags: [python, logic]
date: 2025-11-30
---

General Purpose Algorithm Collections

<details>
<summary>Prime Number</summary>

  ```python
  def is_prime(n):
      """
      Function to check if a number is prime or not
      :param n: the input number, can expect integer
      :return: boolean. True - input is prime number, False - not a prime number
      """
      if n <= 1:
          return False
      if n <= 3:
          return True
      if n % 2 == 0 or n % 3 == 0:
          return False
      i = 5
      while i * i <= n:
          if n % i == 0 or n % (i + 2) == 0:
              return False
          i += 6
      return True
 ```

</details>


<details>
<summary> Sieve of Eratosthenes</summary>


  ```python
  def sieve_eratosthenes(limit):
      """
      Function to create a black box of primes. i.e. a box of all primes till the given limit.
      :param limit: int - the number up to which the blackbox should be created
      :return: list - the list containing indicator for all primes till the limit
      Function will create a list of boolean indicating True for prime index and False for composite index
      """
      sieve = [True] * limit
      sieve[0] = sieve[1] = False
      for current in range(2, int(limit ** 0.5) + 1):
          if sieve[current]:
              for multiple in range(current * current, limit, current):
                  sieve[multiple] = False
      # primes = [num for num, is_prime in enumerate(sieve) if is_prime]
      return sieve
  ```

</details>


<details>
  <summary>Sum of Arithmetic Progression</summary>


```python
def sum_arithmetic_progression(step: int, limit: int) -> int:
    """
    Calculates the sum of all multiples of 'step' strictly below 'limit'
    using O(1) floor division and arithmetic series properties.
    """
    target = limit - 1
    n = target // step
    first_term = step
    last_term = n * step

    return (n * (first_term + last_term)) // 2
```

</details>

## Related

- These helpers power the Project Euler solutions — see [[projects/euler/index|Project Euler]].

