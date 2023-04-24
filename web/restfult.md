REST接口风格（统一接口）.面向资源，而不是面向服务。前者更加抽象，但也更加通用。
比如：面向服务编程需要两个接口：
```python
1. login()
2. logout()
```

REST风格面向资源编程只需要一个接口：
```python
PUT /order/v1/session/1
DELETE /order/v1/session/1
```


