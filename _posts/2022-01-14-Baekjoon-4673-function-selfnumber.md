---
layout: single
title: "Coding Test - Slef Number"
permalink: /codingtest
---

# Self Number
## Question
 백준 > 단계별 문제 > 함수 >  [셀프 넘버(4673)]("https://www.acmicpc.net/problem/4673")

[문제]  

셀프 넘버는 1949년 인도 수학자 D.R. Kaprekar가 이름 붙였다. 양의 정수 n에 대해서 d(n)을 n과 n의 각 자리수를 더하는 함수라고 정의하자. 예를 들어, d(75) = 75+7+5 = 87이다.

양의 정수 n이 주어졌을 때, 이 수를 시작해서 n, d(n), d(d(n)), d(d(d(n))), ...과 같은 무한 수열을 만들 수 있다. 

예를 들어, 33으로 시작한다면 다음 수는 33 + 3 + 3 = 39이고, 그 다음 수는 39 + 3 + 9 = 51, 다음 수는 51 + 5 + 1 = 57이다. 이런식으로 다음과 같은 수열을 만들 수 있다.

33, 39, 51, 57, 69, 84, 96, 111, 114, 120, 123, 129, 141, ...

n을 d(n)의 생성자라고 한다. 위의 수열에서 33은 39의 생성자이고, 39는 51의 생성자, 51은 57의 생성자이다. 생성자가 한 개보다 많은 경우도 있다. 예를 들어, 101은 생성자가 2개(91과 100) 있다. 

생성자가 없는 숫자를 셀프 넘버라고 한다. 100보다 작은 셀프 넘버는 총 13개가 있다. 1, 3, 5, 7, 9, 20, 31, 42, 53, 64, 75, 86, 97

10000보다 작거나 같은 셀프 넘버를 한 줄에 하나씩 출력하는 프로그램을 작성하시오.

[입력]  

입력은 없다.

[출력]  

10,000보다 작거나 같은 셀프 넘버를 한 줄에 하나씩 증가하는 순서로 출력한다.

<br>

---

<br>

## Solution 1
[풀이]  
생성자 `n`이 `1000 * a + 100 * b + 10 * c + d`일때(a, b, c, d는 0에서 9사이의 정수), `d(n)`은 `1001 * a + 101 * b + 11 * c + 2 * d`이므로 `(i == (1001 * a + 101 * b + 11 * c + 2 * d))`를 만족하는 경우가 있다면 `i`는 셀프 넘버가 될 수 없음을 이용한다. 각 숫자에 대해 중첩 for문으로 생성자가 있는지 확인하고, 셀프넘버인지를 가리키는 bool 타입의 변수를 조건문에 포함시킴으로써 생성자를 찾을 경우 반복문을 중단하고 숫자를 출력한다.

```c
#include <stdio.h>
#include <stdbool.h>

void printSelfNumber() {
	bool sn; // true if the number is self number
	int i = 1;
	int a = 0, b = 0, c = 0, d = 0; // n = 1000 * a + 100 * b + 10 * c + d
	for (i; i < 10000; i++) {
		sn = true;
		for (a = 0; a < 10 && sn; a++) {
			for (b = 0; b < 10 && sn; b++) {
				for (c = 0; c < 10 && sn; c++) {
					for (d = 0; 1001 * a + 101 * b + 11 * c + 2 * d <= i && d < 10; d++) { // keep checking until d(n) is less than or equal with number i
						if (i == (1001 * a + 101 * b + 11 * c + 2 * d)) {
							sn = false; // change sn to false and escape nested loop
							break;
						}
					}
				}
			}
		}
		if (sn) { // print number only when sn is true
			printf("%d\n", i);
		}		
	}
}

int main(void) {
	printSelfNumber();
	return 0;
}
```

<br>

## Solution 2
숫자 10001개를 저장할 수 있는 배열에 해당 숫자가 셀프넘버일 경우 1, 아닐 경우 0을 저장한다. 생성자가 i일 때 d(n)을 계산하는 함수를 호출하여 d(n)이 10000 이하일 경우 배열의 d(n)번째 요소에 0 또는 1을 대입한다.

```c
#include<stdio.h>

int SelfNumber(int num)
{
    int sum = num; // num == n, sum == d(n)
    while(num > 0)
    {
        sum += num % 10;
        num /= 10;
    }
    return sum;
}

int main(void)
{
    int arr[10001] = {0, };
    int i, check;
    
    for(i = 0; i < 10001; i++)
    {
        check = SelfNumber(i);
        if(check < 10001)
        {
            arr[check] = 1;
        }
    }
    
    for(i = 0; i < 10001; i++)
    {
        if(arr[i] != 1)
        {
            printf("%d\n", i);
        }
    }
    return 0;
}
```
<br>

---

<br>

## Code Review
solution1은 숫자의 범위가 달라질 경우 코드 수정이 필요하며, 중첩 for문에 의해 가독성이 떨어지고 반복해서 같은 숫자를 확인하기 때문에 solution2가 더 효율적인 시간 복잡도를 가진다.