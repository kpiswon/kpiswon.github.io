---
layout: post
title:  "자료구조(Data Structure) 1주차 : 코딩테스트"
date:   2020-09-01 10:30:00
categories: Data Structure
tags : [C언어, C, 자료구조, 자료구조 테스트]
comments: true
---
자료구조 오리엔테이션 수업에서 C언어의 기본 수준이 되는지 파악하는 테스트를 거친다.  
제한시간은 30분이다.

---

#### 실습예제

```C
1. interger형 변수 'N'을 scanf 함수를 이용하여 입력을 받는다.
2. N개의 int 변수를 받는 배열을 선언한다.
3. 사용자로부터 N개의 int 변수를 입력받는다.
4. 입력받은 N개의 input 변수들의 최소값, 최대값, 중앙값을 구한다.
   - 만약 N이 짝수라면, 중앙값은 중간 2개 변수들의 평균값이다.
5. 최대값, 최대값, 중앙값을 출력한다.
```

* 예시  

```
5
1 2 3 4 5
Min: 1, Max: 5, Median: 3.0
```

```
6
1 2 3 4 5 6
Min: 1, Max: 6, Median: 3.5
```

```
6
19 5 10 20 15 9
Min: 5, Max: 20, Median : 12.5
```

아래 코드가 꼭 정답이 되는 것은 아니다. 나는 먼저 배열에 입력한 숫자들을 오름차순으로 정렬하여 접근하는 방법을 생각했다.

---

##### 작성코드 

{% highlight ruby %}
#include <stdio.h>
#include <stdlib.h>

int main() {
	int N, num1;
	scanf("%d", &N);
	
	int* arr;
	arr = (int*)malloc(sizeof(int) * N);
	
	for (int num = 0; num < N; num++) {
		scanf("%d", &num1);
		arr[num] = num1;
	}

	int temp;
	int temp2;
	int temp3;
	for (int i = 0; i < N; i++) {
		temp = arr[i];
		temp2 = i;
		for (int j = i; j < N; j++) {
			if (temp < arr[j]) {
				continue;
			}
			else {
				temp = arr[j];
				temp2 = j;
			}
		}
		temp3 = arr[i];
		arr[i] = temp;
		arr[temp2] = temp3;
	}


	float med;
	if (N % 2 == 0)
		med = ((float)arr[N / 2 - 1] + (float)arr[N / 2]) / 2;
	else
		med = arr[(1 + N) / 2 - 1];

	printf("Min: %d, Max: %d, Median: %.1f", arr[0], arr[N - 1], med);
	
	free(arr);

	return 0;
}
{% endhighlight %}

---

##### 코드 설명

* 입력받은 숫자만큼 arr 포인터(배열)에 메모리를 할당해주는 코드이다.  
```C
#include <stdlib.h>
int main(){
	...

	int* arr;
	arr = (int*)malloc(sizeof(int) * N);

	...
}
```

* 입력 받은 숫자만큼 사용자에게 입력을 받는 코드

```C
#include <stdio.h>
int main(){
	...

	for (int num = 0; num < N; num++) {
		scanf("%d", &num1);
		arr[num] = num1;
	}

	...
}
```

* 입력 받은 숫자들을 오름차순으로 재배열해주는 코드

```C
#include <stdio.h>
int main(){
	...

	int temp;
	int temp2;
	int temp3;
	for (int i = 0; i < N; i++) {
		temp = arr[i];
		temp2 = i;
		for (int j = i; j < N; j++) {
			if (temp < arr[j]) {
				continue;
			}
			else {
				temp = arr[j];
				temp2 = j;
			}
		}
		temp3 = arr[i];
		arr[i] = temp;
		arr[temp2] = temp3;
	}

	...
}
```

* Median을 계산하는 코드

```C
#include <stdio.h>
int main(){
	...

	float med;
	if (N % 2 == 0)
		med = ((float)arr[N / 2 - 1] + (float)arr[N / 2]) / 2;
	else
		med = arr[(1 + N) / 2 - 1];

	...
}
```

최종수정일 2020.09.01



> ###### References
> 고려대학교 202R 자료구조, 김민수

{% highlight ruby %}
print("hello, neighbors!")
print("Sungwon Kim © 2020 • All rights reserved.")
{% endhighlight %}

[Github][githuburl]

[githuburl]: https://github.com/kpiswon

