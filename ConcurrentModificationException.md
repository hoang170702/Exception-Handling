# Code by GPCoder 
link: 
    ```
    https://gpcoder.com/4289-tranh-loi-concurrentmodificationexception-trong-java-nhu-the-nao/
    ```
# Concurrent Modification Exception

## Định nghĩa

    Chúng ta thường gặp phải "Concurrent Modification Exception" khi cố xóa một phần tử trong mảng khi đang lọc mảng đó.

### 1. Ví dụ

#### 1.1 Thêm/ xóa phần tử khi sử dụng ArrayList.remove() khi duyệt qua for-each.
Code: 
```
package com.gpcoder.collection.list.ConcurrentModificationException;
import java.util.ArrayList;
import java.util.List;
public class ConcurrentModificationException1 {

    public static void main(String[] args) {
        List<String> languages = new ArrayList<>();
        languages.add("Java");
        languages.add("C#");
        languages.add("PHP");
        languages.add("C++");
        languages.add("Ruby");

        // Using forEach loop to iterate and add/ removing element during iteration will
        // throw ConcurrentModificationException in Java
        for (String language : languages) {
            if (language.equals("C#")) {
                languages.remove(language);
            }
        }
    }
}
```
Exception:
```
Exception in thread "main" java.util.ConcurrentModificationException
    at java.util.ArrayList$Itr.checkForComodification(ArrayList.java:901)
    at java.util.ArrayList$Itr.next(ArrayList.java:851)
    at com.gpcoder.collection.list.ConcurrentModificationException.ConcurrentModificationException1.main(ConcurrentModificationException1.java:18)

```
#### 1.2 Thêm/ xóa phần tử sử dụng ArrayList.remove() khi duyệt qua Iterator.
Code:
```
Iterator<String> iterator = languages.iterator();
while (iterator.hasNext()) {
    String language = iterator.next();
    if (language.equals("C#")) {
        languages.remove(language);
    }
}
```
Excetion:
```
Exception in thread "main" java.util.ConcurrentModificationException
    at java.util.ArrayList$Itr.checkForComodification(ArrayList.java:901)
    at java.util.ArrayList$Itr.next(ArrayList.java:851)
    at com.gpcoder.collection.list.ConcurrentModificationException.ConcurrentModificationException2.main(ConcurrentModificationException2.java:19)
```

## 2. Cách phòng tránh
### 2.1 Sử dụng vòng lặp for-index
```
    for (int i = 0; i < languages.size(); i++) {
    String language = languages.get(i);
    if (language.equals("C#")) {
        languages.remove(language);
    }
}
```
### 2.2 Sử dụng phương thức remove() được hỗ trợ bởi Iterator
```
Iterator<String> iterator = languages.iterator();
while (iterator.hasNext()) {
    String language = iterator.next();
    if (language.equals("C#")) {
        // languages.remove(language); // Don't use ArrayList.remove()
        iterator.remove();
    }
}
```
### 2.3 Không remove trong khi duyệt các phần tử
```
List<String> toRemove = new ArrayList<>();
for (String language : languages) {
    if (language.equals("C#") || language.equals("Ruby")) {
        toRemove.add(language);
    }
}
languages.removeAll(toRemove); // [Java, PHP, C++]
```
### 2.4 Sử dụng phương thức removeIf() trong Java 8
```
languages.removeIf(language -> language.equals("C#") || 
language.equals("Ruby"));
```
### 2.5 Sử dụng Stream filter() trong Java 8
```
List<String> removedList = languages.stream()
    .filter(language -> language.equals("C#") || language.equals("Ruby"))
    .collect(Collectors.toList());
``` 
