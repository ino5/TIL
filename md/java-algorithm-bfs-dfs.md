[메인으로 이동](../README.md)

<br>

# BFS, DFS 경로 탐색 기본 문제 정리

## 소개

- BFS와 DFS를 이용한 경로 탐색 문제와 풀이 코드를 주석으로 설명과 함께 정리한다.

## 추후 공부해야 할 내용

- 바이러스 문제 코드 보다 최적화해서 다시 짜보기


<br>

# 📒 목차 <a id="index"></a>

- [📖 문제 - BFS와 DFS 탐색 순서](#Problem1)
- [📖 문제 - 바이러스](#Problem-virus)
- [📖 메모리 초과가 발생하는 경우](#memory-excess)

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

public class P1260_2 {
	final static int DFS = 1;
	final static int BFS = 2;
	static int N, M, V;
	static boolean[][] isEdge;
	static boolean[]isVisited;
	static Stack<Integer> dfsList = new Stack<>();
	static Queue<Integer> bfsList = new LinkedList<>();
	static StringBuffer sbDfs = new StringBuffer();
	static StringBuffer sbBfs = new StringBuffer();
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		int N = sc.nextInt();
		int M = sc.nextInt();
		int V = sc.nextInt();
		
		// 간선 정보 입력
		isEdge = new boolean[N+1][N+1];
		for (int i = 0; i < M; i++) {
			int a = sc.nextInt();
			int b = sc.nextInt();
			isEdge[a][b] = isEdge[b][a] = true;
		}
		
		
		/* DFS */
		isVisited = new boolean[N+1]; // 방문 여부 초기화
		visit(V, DFS); // 초기 위치 V 방문
		dfs(); // DFS 수행
		
		/* BFS */
		isVisited = new boolean[N+1]; // 방문 기록 초기화
		visit(V, BFS); // 초기 위치 V 방문
		bfs(); // BFS 수행
		
		// 결과 출력하기
		System.out.println(sbDfs.toString().trim());
		System.out.println(sbBfs.toString().trim());
		
		sc.close();
	}
	
	// 해당 노드에 방문하기 
	public static void visit(int node, int mode) {
		// 방문 처리하기
		isVisited[node] = true;
		
		// 방문 로직 수행 및 리스트 등록
		if (mode == DFS) {
			sbDfs.append(node+ " ");
			dfsList.push(node);
		} else if (mode == BFS) {
			sbBfs.append(node+ " ");
			bfsList.offer(node);
		}
	}
	
	// DFS 탐색
	public static void dfs() {
		// 리스트에서 탐색할 노드 찾기
		int node = dfsList.peek();
		
		// DFS 방식으로 이웃 노드 탐색
		for (int i = 1; i < isEdge[node].length; i++) {
			// 이웃노드이면서 아직 방문하지 않은 노드에 대해서
			if (isEdge[node][i] && !isVisited[i]) {
				// 노드 방문하기
				visit(i, DFS);
				
				// 다음 DFS 탐색 수행하기
				dfs();
			}
		}
		
		// 리스트에서 탐색 완료한 노드 꺼내기
		dfsList.pop();
	}
	
	// BFS 탐색
	public static void bfs() {
		// 리스트에서 탐색할 노드 찾기
		int node = bfsList.peek();
		
		// BFS 방식으로 이웃 노드 탐색
		for (int i = 1; i < isEdge[node].length; i++) {
			// 이웃노드이면서 아직 방문하지 않은 노드에 대해서
			if (isEdge[node][i] && !isVisited[i]) {
				// 노드 방문하기
				visit(i, BFS);
			}
		}
		
		// 리스트에서 탐색 완료한 노드 꺼내기
		bfsList.poll();
		
		// 다음 BFS 탐색 수행하기
		if(!bfsList.isEmpty()) {
			bfs();
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

- 아래 코드는 큐에 중복된 값이 들어가 메모리가 낭비될 수 있다.

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

<br>

# 📖 메모리 초과가 발생하는 경우 <a id="memory-excess"></a>

[목차로 이동](#index)

## 리스트에 이웃 노드가 중복 등록되는 경우
- 이웃 노드를 리스트로 등록할 때 방문 여부를 고려하지 않아서 중복으로 등록되는 경우 메모리 초과가 발생할 수 있다.
- 방문 여부를 고려하더라도 방문하는 타이밍이 잘못되어 방문 여부 체크가 무의미해지는 경우도 있다. 


<br><br><br>

[목차로 이동](#index)

[메인으로 이동](../README.md)
