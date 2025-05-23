```
键必须是字符串，用双引号包裹，不可重复
值支持的数据类型：字符串（双引号），数字，布尔值（true或false），对象（即{...}），数组（[...]），null

键值对间用逗号分隔
所有键和字符串必须用双引号
最后一个键值对后不能有逗号
不支持注释
数组可以包含混合类型，例如["text",123,false,null,{"key": "value"},[1, 2, 3]]
允许对象和数组为空，即{}和[]合法
```
## 示例
```json
{
  "string": "Hello World",
  "number": 42,
  "float": 3.14,
  "booleanTrue": true,
  "booleanFalse": false,
  "nullValue": null,
  "object": {
    "nestedString": "Nested Value",
    "nestedNumber": 100
  },
  "array": [
    "text",
    123,
    false,
    null,
    {"key": "value"},
    [1, 2, 3]
  ],
  "emptyObject": {},
  "emptyArray": []
}
```