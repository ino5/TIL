[메인으로 이동](../README.md)

<br>

# BFS, DFS 경로 탐색 문제 풀이

## 소개

- BFS와 DFS를 이용한 경로 탐색 문제와 풀이 코드를 주석으로 설명과 함께 정리한다.

## 추후 공부해야할 내용

- 메모리 초과 고려해서 코드 구성 다시 짜기
	- 현재 내가 푼 코드 구성은 큐 리스트에 중복으로 등록 될 수 있다. 그래서 백준에서 문제 풀이 시 다른 문제의 경우 메모리 초과가 발생할 수 있다. 대기열에 노드 추가할 때에도 방문 로직 추가하는 것이 필요하다.


<br>

# 📒 목차 <a id="index"></a>

- [📖 문제 - BFS와 DFS 탐색 순서](#Problem1)
- [📖 문제 - 바이러스](#Problem-virus)

<br>

# 📖 문제 - BFS와 DFS 탐색 순서 <a id="Problem1"></a>

[목차로 이동](#index)

<br>

[백준 - 'DFS와 BFS'](https://www.acmicpc.net/problem/1260)

BFS와 DFS에 따른 노드의 탐색 순서를 출력하는 문제다.

BFS와 DFS를 구현할 수 있는지 묻는 기본 문제이다.

<br>

### 🍳 나의 풀이 코드

```java
package javaalgorithm.baekjoon.silver.s2;

import java.util.*;

// DFS, BFS
public class P1260 {
	static int N, M, V;
	static boolean[][] map; // 노드 연결 여부
	static boolean[] isVisited; // 방문 여부
	static Stack<Integer> stk = new Stack<>(); // BFS를 위한 대기열
	static StringBuffer sbBfs = new StringBuffer(); // BFS 출력 결과
	static Queue<Integer> que = new LinkedList<>();  // DFS를 위한 대기열
	static StringBuffer sbDfs = new StringBuffer(); // DFS 출력 결과
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		
		N = sc.nextInt();
		M = sc.nextInt();
		V = sc.nextInt();
		
		map = new boolean[N+1][N+1];
		isVisited = new boolean[N+1];

		// 간선 정보 입력
		for (int i = 0; i < M; i++) {
			int a = sc.nextInt();
			int b = sc.nextInt();
			map[a][b] = map[b][a] = true;
		}
		
		// dfs 탐색
		stk.push(V); // 초기 위치 V
		while (!stk.isEmpty()) {
			dfs();
		}
		
		// 방문 내역 초기화
		initiate();
		
		// bfs 탐색
		que.offer(V); // 초기 위치 V
		while (!que.isEmpty()) {
			bfs();
		}
		
		// 출력
		System.out.println(sbDfs.toString().trim());
		System.out.println(sbBfs.toString().trim());
		
		sc.close();
	}
	
	// dfs 탐색
	public static void dfs() {
		// 대기열에서 수행할 노드 찾기
		int x = stk.peek();		
		
		// 방문 로직 수행
		if (!isVisited[x]) {
			isVisited[x] = true;
			sbDfs.append(x + " ");
		}

		// 다음 대기열 추가한다.
		if (!addStk(x)) { // 더이상 추가할 이웃 노드 없으면
			stk.pop(); // 대기열에서 노드 꺼내기
		}
	}
	
	// 스택 대기열에 노드 추가하기
	public static boolean addStk(int x) {
		for (int i = 1; i <= N; i++) {
			if (map[x][i] && !isVisited[i]) {
				stk.push(i);
				return true;
			}
		}
		return false;
	}
	
	// bfs 탐색
	public static void bfs() {
		// 대기열에서 수행할 노드를 찾기
		int x = que.peek();
		
		// 방문 로직 수행
		if (!isVisited[x]) {
			sbBfs.append(x + " ");
			isVisited[x] = true;			
		}
		
		// 다음 대기열 추가
		addQue(x);
		
		// 대기열에서 노드 꺼내기
		que.poll();
	}
	
	// 큐 대기열에 노드 추가하기
	public static void addQue(int x) {
		for (int i = 1; i <= N; i++) {
			if (map[x][i] && !isVisited[i]) {
				que.offer(i);
			}
		}
	}
	
	// 방문 내역 초기화하기
	public static void initiate() {
		for (int i = 0; i < isVisited.length; i++) {
			isVisited[i] = false;
		}
	}
}
```


<br>

# 📖 문제 - 바이러스 <a id="Problem-virus"></a>

[목차로 이동](#index)

<br>

[백준 - '바이러스'](https://www.acmicpc.net/problem/2606)

연결된 컴퓨터를 따라 바이러스에 감염될 때 감염된 컴퓨터의 수를 구하는 문제이다.

BFS나 DFS를 이용하여 연결된 노드를 방문하는 기본 문제이다.

<br>

### 🍳 나의 풀이 코드

```java
package javaalgorithm.baekjoon.silver.s3;

import java.util.*;

// 바이러스
public class P2606_2 {
	static int N, P;
	static int cnt = 0; // 감염된 수
	final static int START_NUM = 1; // 시작 감염 컴퓨터 번호
	static boolean[][] map; // 노드 연결 여부
	static boolean[] isVstd; // 방문 여부
	static Queue<Integer> que = new LinkedList<>(); // 방문 대기열
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		N = sc.nextInt();
		P = sc.nextInt();
		map = new boolean[N+1][N+1];
		isVstd = new boolean[N+1];
		// 간선 정보 입력
		for (int i = 0; i < P; i++) {
			int a = sc.nextInt();
			int b = sc.nextInt();
			map[a][b] = map[b][a] = true;
		}
		
		// bfs 탐색
		que.add(START_NUM); // 첫 감염 컴퓨터 입력
		while(!que.isEmpty()) { 
			bfs();
		}
		
		
		// 1번 컴퓨터 제외한 감염된 수 출력
		System.out.println(cnt - 1); 
		
		sc.close();
	}
	
	// bfs 탐색
	public static void bfs() {
		// 대기열에서 가져오기
		int x = que.peek();
		
		// 방문 로직 수행
		if (!isVstd[x]) {
			isVstd[x] = true;
			cnt++;
		}
		
		// 대기열에 추가하기
		addQue(x);
		
		// 대기열에서 꺼내기
		que.poll();
	}
	
	// 대기열에 추가하기
	public static void addQue(int x) {
		for (int i = 1; i <= N; i++) {
			if (map[x][i] && !isVstd[i]) {
				que.offer(i);
			}
		}
	}
}
```




<br><br><br>

[목차로 이동](#index)

[메인으로 이동](../README.md)
