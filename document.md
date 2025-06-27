
# 📘 SimpleDB 사용법

`SimpleDB`는 Python의 `sqlite3`를 기반으로 한 **간단한 SQLite 래퍼 클래스**입니다.  
복잡한 SQL 코드를 줄이고 직관적으로 데이터베이스를 사용할 수 있도록 도와줍니다.

---

## 초기화 및 연결

```python
from simpledb import SimpleDB

db = SimpleDB("example.db")
db.connect()
```

---

## 테이블 생성

```python
db.table("users", {
    "id": "INTEGER PRIMARY KEY AUTOINCREMENT",
    "name": "TEXT",
    "age": "INTEGER"
})
```

---

## 데이터 삽입

```python
db.insert("users", {
    "name": "Alice",
    "age": 25
})
```

---

## 데이터 조회

### 전체 조회

```python
rows = db.select("users")
```

### 조건 + 정렬 + 개수 제한

```python
rows = db.select(
    table="users",
    columns=["id", "name"],
    where="age >= ?",
    where_params=(20,),
    order_by="age DESC",
    limit=3
)
```

---

## 데이터 수정

```python
db.update(
    table="users",
    data={"age": 30},
    where="name = ?",
    where_params=("Alice",)
)
```

---

## 데이터 삭제

```python
db.delete(
    table="users",
    where="age < ?",
    where_params=(20,)
)
```

---

## 특정 값 찾기

```python
age = db.find("users", key_column="name", key_value="Alice", target_column="age")
```

> `find()`는 값이 없을 경우 `ValueError` 예외를 발생시킵니다.

---

## 연결 종료

```python
db.close()
```

---

## 예제 전체 흐름

```python
db = SimpleDB("test.sqlite3")
db.connect()

db.table("items", {
    "id": "INTEGER PRIMARY KEY",
    "name": "TEXT",
    "price": "REAL"
})

db.insert("items", {"name": "apple", "price": 0.99})
db.insert("items", {"name": "banana", "price": 0.59})

print(db.select("items"))

db.update("items", {"price": 1.49}, where="name = ?", where_params=("apple",))
print(db.find("items", "name", "apple", "price"))

db.delete("items", where="price < ?", where_params=(1,))
db.close()
```
