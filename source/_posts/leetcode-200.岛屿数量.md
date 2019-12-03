---
title: leetcode-200.岛屿数量
date: 2019-11-20 12:55:59
tags: [leetcode,面试]
---



本质上就是求图的连通分量的个数。利用栈实现图的遍历。没有使用图的递归 <!-- more --> 



```java
import java.util.Scanner;
import java.util.Stack;

public class Main {
    static class Node {
        int x;
        int y;
        int value;

        public Node(int x, int y, int value) {
            this.x = x;
            this.y = y;
            this.value = value;
        }
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int m = sc.nextInt();
        int n = sc.nextInt();
        int[][] map = new int[m][n];
        int[][] visited = new int[m][n];
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                map[i][j] = sc.nextInt();
                visited[i][j] = 0;
            }
        }

        int count = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (visited[i][j] != 1 && map[i][j] == 1) {
                    count++;
                    visited[i][j] = 1;
                    Node beginNode = new Node(i, j, map[i][j]);
                    Stack<Node> stack = new Stack<>();
                    stack.push(beginNode);

                    while (!stack.isEmpty()) {
                        Node node = stack.peek();
                        // 下
                        if (node.x + 1 < m && map[node.x + 1][node.y] == 1 && visited[node.x + 1][node.y] == 0) {
                            visited[node.x + 1][node.y] = 1;
                            Node node1 = new Node(node.x + 1, node.y, 1);
                            stack.push(node1);
                            // 右
                        } else if (node.y + 1 < n && map[node.x][node.y + 1] == 1 && visited[node.x][node.y + 1] == 0) {
                            visited[node.x][node.y + 1] = 1;
                            Node node1 = new Node(node.x, node.y + 1, 1);
                            stack.push(node1);
                            // 上
                        } else if (node.x - 1 >= 0 && map[node.x - 1][node.y] == 1 && visited[node.x - 1][node.y] == 0) {
                            visited[node.x - 1][node.y] = 1;
                            Node node1 = new Node(node.x - 1, node.y, 1);
                            stack.push(node1);
                            // 左
                        } else if (node.y - 1 >= 0 && map[node.x][node.y - 1] == 1 && visited[node.x][node.y - 1] == 0) {
                            visited[node.x][node.y - 1] = 1;
                            Node node1 = new Node(node.x, node.y - 1, 1);
                            stack.push(node1);
                        } else {
                            stack.pop();
                        }
                    }
                }
            }
        }
        System.out.println(count);
    }
}

```

