## Lambda
- Java로 함수형 언어를 따라할 수 있는 Lamda에 대해서 간단히 정리하려고 한다.
- Lamda의 형식은 어떻게 되는가?
	- [인터페이스명] [변수명] = (매개변수1, 매개변수2, ...) -> { ... };
    - [인터페이스명] [변수명] = (매개변수1, 매개변수2, ...) ->  ... ; // (즉시 리턴)
    - 코드 예제 1
    	- 함수형 계산기 구현
    ```Java
	public class Calculator {
        interface IntegerMath {
          int operation (int a, int b);
        }

        public int operateBinary(int a, int b, IntegerMath op) {
          return op.operation(a, b); 
        }

        public static void main(String... args) {
          Calculator myApp = new Calculator();
          IntegerMath addition = (a, b) -> a + b;
          IntegerMath subbtraction = (a, b) -> a - b;
          System.out.println("40 + 2 = " + myApp.operateBinary(40, 2, addition));
          System.out.println("20 - 10 = " + myApp.operateBinary(20, 10, subtraction));
        }
    }
	```
    	- Interface는 Argument로 들어오는 것들의 데이터 타입을 정의해준다.
    - 코드예제 2
    	- Comparator의 구현
        
    ```Java
      	class Point implements Comparable<Point> {
          int x, y;

          Point(int x, int y) {
              this.x = x;
              this.y = y;
          }
          
          public String toString() {
              return this.x + " " + this.y;
          }

          public int compareTo(Point that) {
              if (this.x < that.x) {
                  return -1;
              } else if (this.x == that.x) {
                  if (this.y < that.y) {
                      return -1;
                  } else if (this.y == that.y) {
                      return 0;
                  } else {
                      return 1;
                  }
              } else {
                  return 1;
              }
        	}
      	}

        public class Main {
        public static void main(String args[]) throws IOException {
            BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
            int n = Integer.parseInt(br.readLine());
            Point points[] = new Point[n];

            for(int i=0; i<n; i++) {
                String xy = br.readLine();
                StringTokenizer st = new StringTokenizer(xy, " ");
                int x = Integer.parseInt(st.nextToken());
                int y = Integer.parseInt(st.nextToken());
                points[i] = new Point(x, y);
            }

            Comparator<Point> comparator = (p1, p2) -> { return p1.compareTo(p2) ; };
			// 혹은 Comparator<Point> comparator = Comparator.naturalOrder()로 해도 된다.
            Arrays.sort(points, comparator);

            StringBuilder sb = new StringBuilder();

            for( Point p : points ){
                sb.append(p.toString() + "\n");
            }

            System.out.println(sb.toString().trim());
        }
    }
	``` 
    ```Java
      	class Point implements Comparable<Point> {
	    	int x, y;

            Point(int x, int y) {
                this.x = x;
                this.y = y;
            }

            public String toString() {
                return this.x + " " + this.y;
            }
      	}

        public class Main {
          public static void main(String args[]) throws IOException {
              BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
              int n = Integer.parseInt(br.readLine());
              Point points[] = new Point[n];

              for(int i=0; i<n; i++) {
                  String xy = br.readLine();
                  StringTokenizer st = new StringTokenizer(xy, " ");
                  int x = Integer.parseInt(st.nextToken());
                  int y = Integer.parseInt(st.nextToken());
                  points[i] = new Point(x, y);
              }
			 // Lambda로만 구현하기
             Comparator<Point> comparator = (p1, p2) -> {
                  if(p1.x > p2.x) {
                      return 1;
                  }
                  else if(p2.x > p1.x) {
                      return -1;
                  }
                  else if(p1.x == p2.x){
                      if(p1.y > p2.y){
                          return 1;
                      }
                      else if(p2.y > p1.y){
                          return -1;
                      }
                      else{
                          return 0;
                      }
                  }
                  return 1;
              };
            
              Arrays.sort(points, comparator);

              StringBuilder sb = new StringBuilder();

              for( Point p : points ){
                  sb.append(p.toString() + "\n");
              }

              System.out.println(sb.toString().trim());
          }
    }
	```
    
