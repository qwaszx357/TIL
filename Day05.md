# Day05_2022-04-14

## AI 플랫폼을 활용한 웹서비스 개발

### 1. 메모리 구조

- 이차원배열이 만들어질 때 메모리 구조

```java
int ar[][] = { {100,99,80}, {98,91,83}, {89,96,82} };
```

<img src="C:\Users\hasun\AppData\Roaming\Typora\typora-user-images\image-20220414160030298.png" alt="image-20220414160030298" style="zoom:50%;" />

- 실행 순서
  1. 객체가 만들어진다.
  2. ar 배열이 만들어진다.
  3. 객체가 참조할 번지들이 만들어진다.
  4. 각 번지에 값이 들어간다. 

### 2. 팀 프로젝트 진행

#### 1. 문제 만들기 

1. Ws01.

- 문제 : 3X3 행렬에 난수를 입력받고, 사용자가 입력한 행의 합과 평균을 출력하시오.

  -  int형 [3][3] 사이즈의 이차원배열을 만든다.

  - 1 ~ 99까지의 난수를 입력받아 넣는다.

  - Scanner를 이용해 행 번호를 입력한다.

    입력한 값이 정수가 아닐 경우 "Error1" 출력, 범위를 벗어난 경우 "Error2"를 출력한다.

  - 사용자가 입력한 행에 해당하는 요소들의 합과 평균을 출력한다.

- 내가 작성한 코드

```java
package team4;

//import java.util.Arrays;
import java.util.Random;
import java.util.Scanner;

public class Ws01 {

	public static void main(String[] args) {
//		int형 3*3 행렬에 1 ~ 99까지의 난수를 입력받고,
//		사용자가 원하는 행의 값을 입력하면
//		그 행의 합과 평균이 출력된다.
		
		int ar [][] = new int[3][3];
		Random rd = new Random();

		Scanner sc = new Scanner(System.in);
		System.out.println("행의 값(0 ~ 2)을 입력하세요.");
		String sn = sc.next();
		double sum = 0.0;
		int n = 0;
		
		try {
			n = Integer.parseInt(sn);
			if (n < 0 || n > 2) {
				System.out.println("Error 2");
				sc.close();
				return;
			}
		} catch (Exception e) {
			System.out.println("Error 1");
			sc.close();
			return;
		}
		
		for (int i = 0; i < ar.length; i++) {
			for (int j = 0; j < ar[i].length; j++) {
				ar[i][j] = rd.nextInt(99) + 1;
			}
//			System.out.println(Arrays.toString(ar[i]));
			
			if (i == n) {
				for (int j = 0; j < ar[i].length; j++) {
					sum += ar[i][j];
				}
				System.out.printf("%d행의 합은: %.2f 입니다. \n", n, sum);
				System.out.printf("%d행의 평균은: %.2f 입니다. \n", n, sum / ar[n].length);
			}

		}
		sc.close();
	}

}
```

2. Ws02.

- 문제 : 3줄의 로또 번호를 자동으로 생성하는 프로그램을 만드시오.

  - int형 [3][6] 사이즈의 이차원배열을 만든다.
  2. 1 ~ 45까지의 난수를 입력받아 넣는다.
   같은 행에 있는 로또 번호는 중복될 수 없다.
  3. 로또 번호를 출력한다.

- 내가 작성한 코드

```java
package team4;

import java.util.Random;

public class Ws02 {

	public static void main(String[] args) {
//		6개의 번호로 이루어진 로또번호를 3줄 출력하시오.
//		같은 행에 있는 로또 번호는 중복될 수 없다.
		
		int ar [][] = new int[3][6];
		Random rd = new Random();
		
		for (int i = 0; i < ar.length; i++) {
			for (int j = 0; j < ar[i].length; j++) {
				ar[i][j] = rd.nextInt(45) + 1;
				for (int j2 = 0; j2 < j; j2++) {
					if (ar[i][j] == ar[i][j2]) {
						j--;
					}
				}
			}
		}
		for (int i = 0; i < ar.length; i++) {
			System.out.printf("%d번째 로또번호: \n", i + 1);
			for (int j = 0; j < ar[i].length; j++) {
				System.out.printf("%d \t", ar[i][j]);
				
			}System.out.println("");
		}
	}

}
```

3. Number Guess Game

- 문제

  - 1~100 까지의 정수를 난수로 발생시켜 정답으로 지정한다.

  - 숫자를 입력하면 정답보다 작을 경우 'UP', 클 경우 'DOWN'을 출력한다.

  - 5번의 기회가 주어지고 실패할 경우 카운트가 하나씩 줄어든다.

    입력한 값이 정수가 아니면 카운트는 줄어들지 않고 오류 코드를 출력한다.)

  - 정답이 나오면 'Congratulations!', 5번의 기회가 모두 끝나면 'GAME OVER'을 출력한다.

- 내가 작성한 코드

```java
package ws;

import java.util.Random;
import java.util.Scanner;

public class NumberGuessGame {

	public static void main(String[] args) {
		// 1 ~ 100 범위의 랜덤한 숫자를 발생시킨다.
		Random r = new Random();
		int rn = r.nextInt(100) + 1;  // 랜덤 숫자를 rn에 저장하기
		int n = 0;  // 사용자가 입력할 숫자
		
		System.out.println("Number Guess Game을 시작합니다.");
		Scanner sc = new Scanner(System.in);
		int cnt = 5;  // 도전할 수 있는 횟수
		
		while (true) {
			System.out.println("Input Number(1 ~ 100): ");
			String sn = sc.next();
			try {  // 정수인지 검증하기
				n = Integer.parseInt(sn);  
			} catch (Exception e) {  
				System.out.println("숫자를 입력하세요.");
				continue;
			}
			if (n < 1 || n > 100) {  //1 ~ 100 범위가 맞는지 검증하기
				System.out.println("1 ~ 100의 숫자를 입력하세요.");
				continue;
			}
			if (n == rn) {  // 정답일 경우
				System.out.println("Good");
				sc.close();
				return;
			}else if (n > rn) {  // 입력한 숫자가 더 큰 경우
				cnt--;
				if (cnt > 0) {
					System.out.printf("Down \t 남은 횟수: %d \n", cnt);
					continue;
				}else {
					System.out.println("Retry");
					sc.close();
					return;
				}
			}else if (n < rn) {  // 입력한 숫자가 더 작은 경우
				cnt--;
				if (cnt > 0) {
					System.out.printf("Up \t 남은 횟수: %d \n", cnt);
					continue;
				}else {
					System.out.println("Retry");
					sc.close();
					return;
				}
			}
		}
		
	}
	
}
```

4. Lotto Game

- 문제

  - 1~15까지의 숫자 중 6개가 중복없이 나열되어 당첨 코드로 정의된다.

  - 게임 참여자는 10,000원으로 시작한다.

  - 로또는 한 줄에 1,000원이며 당첨금은 등수에 따라 상이하다.

    (1등 : 1억, 2등 : 1만 원, 3등 : 5천 원, 4등 : 1천 원)

  - 각 메뉴에 해당하는 키를 누르면 해당 기능이 실행된다. (m 을 누르면 메뉴를 볼 수 있다.)

  - 's'(추첨시작)을 누르면 당첨 코드가 생성되고, 사용자는 몇개의 로또를 구매할 것인지 숫자로 입력하면 금액이 차감되고 당첨금이 있다면 지급된다.

  - 잔액이 바닥나면 'GAME OVER'출력

  - 각 항목에서 입력값에 오류가 있을 경우, 상황에 맞는 오류메세지 출력

- 내가 작성한 코드

```java
package ws;

import java.util.Arrays;
import java.util.Random;
import java.util.Scanner;

public class LottoGame {

	public static void main(String[] args) {
		System.out.println("start ...");
		Scanner sc = new Scanner(System.in);
		Random r = new Random();
		int balance = 10000;
		int n = 0;
		
 		while(true) {
			System.out.println("추첨을 시작합니다 ...");
			System.out.println("(m : 메뉴 확인)");
			String cmd = sc.next();
			
			if (!(cmd.equals("m") || cmd.equals("q") || cmd.equals("s") ||
					cmd.equals("a") || cmd.equals("p"))) {
				System.out.println("메뉴를 확인하세요.");
			}			
			if (cmd.equals("q")) {
				System.out.println("Bye");
				break;
			}else if(cmd.equals("m")) {
				System.out.println("s : 추첨 시작");
				System.out.println("a : 잔액 확인");
				System.out.println("p : 상금 확인");
				System.out.println("q : 게임 나가기");
			}else if(cmd.equals("a")) {  // 잔액 확인하기
				System.out.println("잔액 입니다.");
				System.out.printf("Balance: %d \n" , balance);
			}else if(cmd.equals("s")) {  // 추첨 시작하기
				// 로또 번호 생성하기
				int ar [] = new int[6];
				for (int i = 0; i < ar.length; i++) {
					ar[i] = r.nextInt(10) + 1;
					for (int j = 0; j < i; j++) {
						if (ar[i] == ar[j]) {
							i--;
						}
					}
				}
				System.out.println(Arrays.toString(ar));
				System.out.println("원하는 로또의 갯수를 입력하세요.");
				n = sc.nextInt();
				
				if ((n * 1000) > balance) {  // 잔액이 부족할 경우
					System.out.println("잔액이 부족합니다.");
					continue;
				}
				int arr [][] = new int[n][6];  // n만큼 배열을 만들기

				for (int i = 0; i < arr.length; i++) {
					for (int j = 0; j < arr[i].length; j++) {
						arr[i][j] = r.nextInt(10) + 1;
						for (int j2 = 0; j2 < j; j2++) {  // 중복되는 숫자 제거하기
							if (arr[i][j] == arr[i][j2]) {
								j--;
							}
						}
					}
					
				}
				// 구매 갯수만큼 차감하기
				balance -= (n * 1000);
				
				// 당첨 숫자 확인하기
				int accord = 0;
				for (int i = 0; i < arr.length; i++) {
					accord = 0;
					for (int j = 0; j < arr[i].length; j++) {
						for (int j2 = 0; j2 < ar.length; j2++) {
							if (arr[i][j] == ar[j2]) {
								accord++;
							}
						}
					}
					System.out.println(Arrays.toString(arr[i]) + "\t 당첨 갯수는: " + accord);
					// 당첨금 추가하기
					if (accord == 6) {
						balance += 100000000;
					}else if(accord == 5){
						balance += 10000;
					}else if(accord == 4){
						balance += 5000;
					}else if(accord == 3){
						balance += 1000;
					}
				}
			}else if(cmd.equals("p")) {
				System.out.println("1등: 1억원");
				System.out.println("2등: 1만원");
				System.out.println("3등: 5천원");
				System.out.println("4등: 1천원");
			}			
			
		}
		sc.close();
		System.out.println("End ...");
	}

}
```

