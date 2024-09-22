# 哈希表 （HashMap）

以空间换时间，实现o(1)查找的时间复杂度
通过hash函数确定存储位置，避免了遍历查找。

### hashcode 与 equals
    java默认.hashcode返回内存地址
    equals "==" 比较内存地址


```import java.util.*;

import static java.util.Objects.hash;

/**
 *  A hash table-backed Map implementation.
 *
 *  Assumes null keys will never be inserted, and does not resize down upon remove().
 *  @author YOUR NAME HERE
 */
public class MyHashMap<K, V> implements Map61B<K, V> {

    /**
     * Protected helper class to store key/value pairs
     * The protected qualifier allows subclass access
     */
    public class Node {
        K key;
        V value;

        Node(K k, V v) {
            key = k;
            value = v;
        }
    }

    private int initialCapacity;
    private double loadFactor;//负载因子
    private int size=0;
    /* Instance Variables */
    private Collection<Node>[] buckets;
    // You should probably define some more!

    /** Constructors */
    public MyHashMap() {
        this(2,1.75);
    }

    public MyHashMap(int initialCapacity) {
        this(initialCapacity,1.75);
    }

    /**
     * MyHashMap constructor that creates a backing array of initialCapacity.
     * The load factor (# items / # buckets) should always be <= loadFactor
     *
     * @param initialCapacity initial size of backing array
     * @param loadFactor maximum load factor
     */
    public MyHashMap(int initialCapacity, double loadFactor) {
        this.initialCapacity = initialCapacity;
        this.loadFactor = loadFactor;
        this.buckets = new LinkedList[initialCapacity];
        for (int i = 0; i < initialCapacity; i++) {
            buckets[i] = createBucket();
        }
       
    }

    /**
     * Returns a data structure to be a hash table bucket
     *
     * The only requirements of a hash table bucket are that we can:
     *  1. Insert items (`add` method)
     *  2. Remove items (`remove` method)
     *  3. Iterate through items (`iterator` method)
     *  Note that that this is referring to the hash table bucket itself,
     *  not the hash map itself.
     *
     * Each of these methods is supported by java.util.Collection,
     * Most data structures in Java inherit from Collection, so we
     * can use almost any data structure as our buckets.
     *
     * Override this method to use different data structures as
     * the underlying bucket type
     *
     * BE SURE TO CALL THIS FACTORY METHOD INSTEAD OF CREATING YOUR
     * OWN BUCKET DATA STRUCTURES WITH THE NEW OPERATOR!
     *
     * 可选用不同的数据结构来实现bucket
     */
    private Collection<Node> createBucket() {
        // TODO: Fill in this method.
        return new LinkedList<Node>();
    }

    // TODO: Implement the methods of the Map61B Interface below
    // Your code won't compile until you do so!





    @Override
    public void put(K key, V value) {
        //判断负载因子是否达到上限
        if((double) size /initialCapacity >= loadFactor){
            resize();
        }

       int index = hash(key);
        Collection<Node> bucket = buckets[index];
        for (Node t:bucket){
            if (t.key == key){
                t.value = value;
                return;
            }
        }
        bucket.add(new Node(key,value));
        size++;
    }

    @Override
    public V get(K key) {
        int index = hash(key);
        Collection<Node> bucket = buckets[index];
        for (Node t:bucket){
            if (t.key.equals(key)){
                return t.value;
            }
        }
        return null;
    }

    @Override
    public boolean containsKey(K key) {
        int index = hash(key);
        Collection<Node> bucket = buckets[index];
        for (Node t:bucket){
            if (t.key.equals(key)){
                return true;
            }
        }
        return false;
    }

    /**
     * 当负载系数达到上限时扩容buckets
     *
     *
     */
    public void resize(){
        initialCapacity = initialCapacity*2;
        Collection<Node>[] newBuckets = new LinkedList[initialCapacity];
        for(int i=0;i < initialCapacity;i++){
            newBuckets[i] = createBucket();
        }
       for (Collection<Node> bucket:buckets){
           for (Node t:bucket){
               int index = hash(t.key);
               newBuckets[index].add(t);
           }

       }
        buckets = newBuckets;
    }

    @Override
    public int size() {
        return size;
    }

    @Override
    public void clear() {
        size = 0;
        for (int i=0;i < buckets.length;i++){
            buckets[i] = createBucket();
        }
    }

    @Override
    public Set<K> keySet() {
       throw new UnsupportedOperationException();
    }

    @Override
    public V remove(K key) {
       throw new UnsupportedOperationException();
    }

    @Override
    public Iterator<K> iterator() {
       throw new UnsupportedOperationException();
    }

    /*
    * 哈希函数
    * 根据此时的buckets容量取模返回index
    * */
    public int hash(K key){
        if (key == null){
            return 0;
        }
        int index = Math.floorMod(key.hashCode(),initialCapacity);
        return index;
    }

}
```
