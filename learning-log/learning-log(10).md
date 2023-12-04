### 요약 : 별도로 Hash값을 계산 및 해싱 충돌작업에 대비하는 과정이 필요한 HashMap과 다르게, EnumMap은 Enum의 ‘선언 순서’를 인덱스로 활용하므로, 모든 CRUD과정이 O(1)의 시간을 확보한다.

### 따라서, Map을 사용하는데 Key가 Enum이라면, EnumMap을 고려하자!

### EnumMap의 Put() VS HashMap의 Put()

- EnumMap

```java
public V put(K key, V value) {
    typeCheck(key);

    int index = key.ordinal();
    Object oldValue = vals[index];
    vals[index] = maskNull(value);
    if (oldValue == null)
        size++;
    return unmaskNull(oldValue);
}
```

- HashMap

```java
final V putVal(int hash, K key, V value, boolean onlyIfAbsent, boolean evict) {
    Node<K,V>[] tab; Node<K,V> p; int n, i;
    if ((tab = table) == null || (n = tab.length) == 0)
        n = (tab = resize()).length;
    if ((p = tab[i = (n - 1) & hash]) == null)
        tab[i] = newNode(hash, key, value, null);
    else {
        Node<K,V> e; K k;
        if (p.hash == hash &&
            ((k = p.key) == key || (key != null && key.equals(k))))
            e = p;
        else if (p instanceof TreeNode)
            e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);
        else {
            for (int binCount = 0; ; ++binCount) {
                if ((e = p.next) == null) {
                    p.next = newNode(hash, key, value, null);
                    if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st
                        treeifyBin(tab, hash);
                    break;
                }
                if (e.hash == hash &&
                    ((k = e.key) == key || (key != null && key.equals(k))))
                    break;
                p = e;
            }
        }
        if (e != null) { // existing mapping for key
            V oldValue = e.value;
            if (!onlyIfAbsent || oldValue == null)
                e.value = value;
            afterNodeAccess(e);
            return oldValue;
        }
    }
++modCount;
    if (++size > threshold)
        resize();
    afterNodeInsertion(evict);
    return null;
}
```

**HashMap은 키의 Hash값을 계산해서 인덱스로 사용, EnumMap은 Enum의 선언순서를 인덱스로 사용**, HashMap은 해당 인덱스가 비어있지 않다면, 연결리스트로 다음노드로 저장 및 해당 인덱스가 비어있다면, 그대로 값을 추가함. EnumMap은 동일한 인덱스(키)로 값을 삽입한다면, 이전 데이터 제거.

### EnumMap의 Get() VS HashMap의 Get()

- EnumMap

```java
public V get(Object key) {
    return (isValidKey(key) ? unmaskNull(vals[((Enum<?>)key).ordinal()]) : null);
}
```

- HashMap

```java
final Node<K,V> getNode(int hash, Object key) {
    Node<K,V>[] tab; Node<K,V> first, e; int n; K k;
    if ((tab = table) != null && (n = tab.length) > 0 &&
        (first = tab[(n - 1) & hash]) != null) {
        if (first.hash == hash && // always check first node
            ((k = first.key) == key || (key != null && key.equals(k))))
            return first;
        if ((e = first.next) != null) {
            if (first instanceof TreeNode)
                return ((TreeNode<K,V>)first).getTreeNode(hash, key);
            do {
                if (e.hash == hash &&
                    ((k = e.key) == key || (key != null && key.equals(k))))
                    return e;
            } while ((e = e.next) != null);
        }
    }
    return null;
}
```

HashMap의 Get()은 해시충돌이 잦으면 O(logN)의 근접, EnumMap의 Get()은 항상 O(1)에 근접

- 참조 블로그
    - https://velog.io/@kasania/Java-EnumMap
    - https://siyoon210.tistory.com/142
    - [https://velog.io/@msung99/자바공부-EnumMap-가-뭐고-왜-HashMap-보다-성능이-더-빠르지](https://velog.io/@msung99/%EC%9E%90%EB%B0%94%EA%B3%B5%EB%B6%80-EnumMap-%EA%B0%80-%EB%AD%90%EA%B3%A0-%EC%99%9C-HashMap-%EB%B3%B4%EB%8B%A4-%EC%84%B1%EB%8A%A5%EC%9D%B4-%EB%8D%94-%EB%B9%A0%EB%A5%B4%EC%A7%80)
