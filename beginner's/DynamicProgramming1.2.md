## DP문제풀이 1 - 1로 만들기
- 1로 만들기, boj.kr/1463
	- 세준이는 어떤 정수 N에 다음과 같은 연산 중 하나를 할 수 있다.
    	1. N이 3으로 나누어 떨어지면 3으로 나눈다.
        2. N이 2로 나누어 떨어지면 2로 나눈다.
        3. 1을 뺀다.
    - 세준이는 어떤 정수 N에 위와 같은 연산을 선택해서 1을 만드려고 한다.
    - 연산을 사용하는 횟수의 최소값을 출력하시오.
- 문제 풀이 접근법
	- 숫자를 빨리 줄여야 한다.
		- 3으로 나누는 연산을 최대한 많이 사용
    	- 2로 나누는 연산을 그 다음으로 많이 사용
    	- 1을 빼는 연산을 가장 적게 사용
    - 하지만 무작정 나눈다고 빨리 풀릴까?
    	- 10의 경우를 살펴보자
        	- 무작정 나누는 경우
            	- 10을 2로 나누면 5
                - 5에서 1을 빼면 4
                - 4에서 2로 나누면 2
                - 2에서 1을 빼거나 2로 나누면 1
            - 1을 빼서라도 3의 배수로 만드는 경우
            	- 10에서 1을 빼면 9
                - 9에서 3으로 나누면 3
                - 3에서 3으로 나누면 1
            - 결과적으로 무작정 나누는 것은 해답이 될 수 없다.
            - 다이나믹 프로그래밍이 필요하다.
    - D[N] 배열에는 무엇을 Memoization 해야 하는가?
    	- N을 1로 만드는데 필요한 연산의 최소값
    - N을 어떻게 분기시킬까?
    	- N을 3으로 나눈다면?
        	- D[N/3] + 1
        - N을 2로 나눈다면?
        	- D[N/2] + 1
        - N에서 1을 뺀다면?
        	- D[N-1] + 1
    - 간단한 프로세스를 정의해보자
    	- 먼저 n=1부터 최소한의 경우의 수를 구해놓는다.
       	- n=1일 때 구해놓은 값을 기준으로 n=2를 구한다.
        - n=2일 때 구해놓은 값을 기준으로 n=3을 구한다.
        - 반복한다... n=n일 때 최소값을 구한다.
	- 실제 구현(top-down)
    ```java
	import java.io.BufferedReader;
    import java.io.IOException;
    import java.io.InputStreamReader;

    public class Main{

        static int[] d;

        static int go(int n){
            if( n == 1 ) return 0; // 1일 때는 아무 연산도 할 필요 없음

            if( d[n] > 0 ) return d[n]; // 이미 계산된 값인 경우 그것을 이용하면 된다.

            d[n] = go(n-1) + 1; // n-1을 계속 수행했을 때 (일반적으로 최악의 경우)

            if (n%2 == 0) {
                int temp = go(n/2) + 1; // 2로 나누는 것이 이득일 때 배열 교환
                if (d[n] > temp) d[n] = temp;
            }

            if (n%3 == 0) {
                int temp = go(n/3) + 1; // 3으로 나누는 것이 이득일 때 배열 교환
                if (d[n] > temp) d[n] = temp;
            }

            return d[n];
        }

        public static void main(String args[]) throws IOException {
            BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
            int n = Integer.parseInt(br.readLine());
            d = new int[n+1];
            go(n);
            System.out.println(d[n]);
        }
    }
	```
    - 실제 구현(bottom-up)
    ```java
	import java.io.BufferedReader;
    import java.io.IOException;
    import java.io.InputStreamReader;
    
    public class Main{
    
        public static void main(String args[]) throws IOException {
            BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
            int n = Integer.parseInt(br.readLine());
    
            if(n == 1){
                System.out.println(0);
                return;
            }
    
            int[] d = new int[n+1];
    
            d[0] = 0;
            d[1] = 0;
            d[2] = 1;
    
            for(int i=3; i<=n; i++){
                int di = d[i-1]+1;
                if(i%2 == 0) di = (di <  d[i/2]+1) ? di : d[i/2]+1;
                if(i%3 == 0) di = (di <  d[i/3]+1) ? di : d[i/3]+1;
                d[i] = di;
            }
    
            System.out.println(d[n]);
        }
    }
	```
