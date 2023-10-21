# Basic JAVA

## 更改变量和值
### 可变与不可变值
- `String` : 不可变
```java
String s = "a";
s = s + "b";
```
![](./images/2023-10-21-11-36-13.png)

- `StringBuilder` : 可变
```java
StringBuilder sb = new StringBuilder("a");
sb.append("b");`
```
![](./images/2023-10-21-11-39-29.png)


## JAVA Set / Map / List
### List
![](./images/2023-10-21-11-51-52.png)
```java
list.size() //len(lst)
list.add(e) //lst.append(e)
if(list.isEmpty()) // if not lst
list.contains(e) // e in lst
```
### Map
![](./images/2023-10-21-11-52-03.png)
```java
map.put(key, value) // map[key] = value
map.get(key) // map[key]
map.containsKey(key) // key in map
map.remove(key) // del map[key]
```
### Set
![](./images/2023-10-21-11-52-14.png)
```java
set.contains(e) // e in set
set.containsAll(set2) // set.issuperset(set2)    set >= set2
set.removeAll(set2) // set.difference(set2)      set -= set2
```


