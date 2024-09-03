# 并查集 (Disjoint sets)


## 1.三种思路
### 1.1 listOfSetsDS

基于List 
| connect | isConnect |
|------|--------|
| O(n) | O(n) |

### 1.2 QuickFindDs
基于数组 
| connect | isConnect |
|------|--------|
| O(n) | O(1) |

### 1.3 QuickUnionDs //最常用 最佳方案
基于数组 
| connect | isConnect |
|------|--------|
| O(n) | O(n) |


## 2.实现 QuickUnionDS

#### 优化一 加权并查集
保证让小树合并到大树上

追踪每个树的大小（在root上用负号树节点数表示）
**保证了查到根节点的时间复杂度 O(log N)**
| connect | isConnect |
|------|--------|
| O(logn) | O(logn) |

#### 优化二 路径压缩 
爬树的同时展平树，将节点连接到root上，将树扁平化

Java 实现  from cs61b2024

```public class UnionFind {
    private final int[] s;
    // TODO: Instance variables

    /* Creates a UnionFind data structure holding N items. Initially, all
       items are in disjoint sets. */
    public UnionFind(int N) {
        // TODO: YOUR CODE HERE 初始化数组都置-1
        s = new int[N];
        for (int i=0;i < s.length;i++){
            s[i] = -1;
        }
    }

    /* Returns the size of the set V belongs to. */
    public int sizeOf(int v) {
        // TODO: YOUR CODE HERE
        int root = find(v);
        return -s[root];

    }

    /* Returns the parent of V. If V is the root of a tree, returns the
       negative size of the tree for which V is the root. */
    public int parent(int v) {
        // TODO: YOUR CODE HERE
        if(s[v] > 0){
            return s[v];
        }else {
            return -sizeOf(v);
        }

    }

    /* Returns true if nodes/vertices V1 and V2 are connected. */
    public boolean connected(int v1, int v2) {
        // TODO: YOUR CODE HERE
        if (find(v1) == find(v2)){
            return true;
        }
        return false;
    }

    /* Returns the root of the set V belongs to. Path-compression is employed
       allowing for fast search-time. If invalid items are passed into this
       function, throw an IllegalArgumentException. */
    public int find(int v) {
        // TODO: YOUR CODE HERE
        if(v < 0 || v > s.length){
            throw new IllegalArgumentException("非法的索引");
        }
        if(s[v] < 0){
            return v;
        }else {
            s[v] = find(s[v]);//递归实现路径压缩
            return s[v];
        }
    }

    /* Connects two items V1 and V2 together by connecting their respective
       sets. V1 and V2 can be any element, and a union-by-size heuristic is
       used. If the sizes of the sets are equal, tie break by connecting V1's
       root to V2's root. Union-ing an item with itself or items that are
       already connected should not change the structure. */
    public void union(int v1, int v2) {
        // TODO: YOUR CODE HERE
        int root1 = find(v1);
        int root2 = find(v2);
        if (root1 == root2){
            return;
        }

        if (s[root1] >= s[root2]){//由于存的是负数 实际上树2的质量大于树1

            s[root2] = s[root2]+s[root1];
            s[root1] = root2;

        }else {
            s[root1] = s[root1]+s[root2];
            s[root2] = root1;
        }
    }

} 
```