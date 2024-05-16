# Code by GPCoder

link:
`https://gpcoder.com/4193-tranh-loi-nullpointerexception-trong-java-nhu-the-nao/`

# Null Pointer Exception

## Định nghĩa

    Chúng ta thường gặp phải "Null Pointer Exception" khi chúng ta truyền tham số, trả về một kết quả null.

### 1. Return giá trị.

#### 1.1 Return một EMPTY collection thay vì giá trị NULL.

Sai:

```
Không nên return về một giá trị null:
        class UserDao {
            public List<User> getUsers () {
                // Query database and convert to list
                boolean hasData = false;
                if (hasData) {
                    // return list of users
                }
                // There is no user
                return null;
            }
        }

        class UserService {
            private UserDao dao = new UserDao();
            public void showUsers () {
                List<User> users = dao.getUsers();
                if (users != null) { // Must be checked null
                    // Show user information
                }
            }

            public void exportToCsv () {
                List<User> users = dao.getUsers();
                if (users != null) { // Must be checked null
                    // Export to csv file
                }
            }
        }
```

Đúng:

```
Chúng ta nên return về những thứ sau:
    List: Collections.emptyList()
    Set: Collections.emptySet()
    Map: Collections.emptyMap()
```

#### 1.2. Return một EMPTY String

```
org.apache.commons.lang.StringUtils.EMPTY
```

#### 1.3 Return một giá trị Unknown/ Default thay vì Null

Sử dụng Null Object Pattern:

```
    public User getUser(UserType type){
    switch(type) {
        case ADMIN:
            return getAdmin();
        case MANAGER:
            return getManager();
        default:
            break;
    }
    return null;
}
```

Có thể viết tránh lỗi NULL như sau:

```
public User getUser(UserType type){
    switch(type) {
        case ADMIN:
            return getAdmin();
        case MANAGER:
            return getManager();
        default:
        break;
    }
    return NullUser ();
}

class NullUser extends User {
    // Implement all abstract methods of User
    // Override neccessary methods to drive to do "Nothing" or "Default" action.
}
```

### 2. Luôn kiểm tra NULL trước khi sử dụng.

### 3. Kiểm tra Null-safe.

Ví dụ:

```
Đúng:
    if("Hello".equals(hello)) { }

Sai:
    if(hello != null) {
        if(hello.equals("Hello")) { }
    }

    // hoặc

    if(hello.equals("Hello")) { }
```

### 4. Hạn chế sử dụng multi-dot syntax

```
Sai:
    getLoggedinUser().getUser().getRole().setRoleName("Developer");

Đúng:
    - nên check null trước khi qua bước tiếp theo:

                LoggedInUser loggedInUser = getLoggedInUser();
                if (loggedInUser != null) {
                    User user = loggedInUser.getUser();
                    if (user != null) {
                        Role role = user.getRole();
                        if (role != null) {
                            role.setRoleName("Developer");
                        }
                    }
                }
```

### 5. Khởi tạo giá trị trước khi sử dụng

#### Khởi tạo ngay khi khai báo property :

```
private List<User> users = new ArrayList<>();
```

#### Khởi tạo trong hàm constructor:

```
public UserManager() {
    users = new ArrayList<>();
}
```

#### Khởi tạo trong Getter:

```
public List<User> getUsers() {
    if(users == null) {
        users = new ArrayList<>();
    }
    return users ;
}
```

### 6. Sử dụng tính năng mới trong Java 8 – Optional
