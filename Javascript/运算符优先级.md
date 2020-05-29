| 优先级                                                       | 运算类型                                                     | 关联性        | 运算符      |
| :----------------------------------------------------------- | :----------------------------------------------------------- | :------------ | :---------- |
| 20                                                           | [`圆括号`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Grouping) | n/a（不相关） | `( … )`     |
| 19                                                           | [`成员访问`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Property_Accessors#点符号表示法) | 从左到右      | `… . …`     |
| [`需计算的成员访问`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Property_Accessors#括号表示法) | 从左到右                                                     | `… [ … ]`     |             |
| [`new`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/new) (带参数列表) | n/a                                                          | `new … ( … )` |             |
| [函数调用](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Functions) | 从左到右                                                     | `… ( … )`     |             |
| [可选链（Optional chaining）](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Optional_chaining) | 从左到右                                                     | `?.`          |             |
| 18                                                           | [new](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/new) (无参数列表) | 从右到左      | `new …`     |
| 17                                                           | [后置递增](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Arithmetic_Operators#Increment)(运算符在后) | n/a           | `… ++`      |
| [后置递减](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Arithmetic_Operators#Decrement)(运算符在后) | `… --`                                                       |               |             |
| 16                                                           | [逻辑非](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Logical_Operators#Logical_NOT) | 从右到左      | `! …`       |
| [按位非](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Bitwise_Operators#Bitwise_NOT) | `~ …`                                                        |               |             |
| [一元加法](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Arithmetic_Operators#Unary_plus) | `+ …`                                                        |               |             |
| [一元减法](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Arithmetic_Operators#Unary_negation) | `- …`                                                        |               |             |
| [前置递增](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Arithmetic_Operators#Increment) | `++ …`                                                       |               |             |
| [前置递减](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Arithmetic_Operators#Decrement) | `-- …`                                                       |               |             |
| [typeof](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/typeof) | `typeof …`                                                   |               |             |
| [void](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/void) | `void …`                                                     |               |             |
| [delete](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/delete) | `delete …`                                                   |               |             |
| [await](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/await) | `await …`                                                    |               |             |
| 15                                                           | [幂](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Arithmetic_Operators#Exponentiation) | 从右到左      | `… ** …`    |
| 14                                                           | [乘法](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Arithmetic_Operators#Multiplication) | 从左到右      | `… * …`     |
| [除法](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Arithmetic_Operators#Division) | `… / …`                                                      |               |             |
| [取模](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Arithmetic_Operators#Remainder) | `… % …`                                                      |               |             |
| 13                                                           | [加法](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Arithmetic_Operators#Addition) | 从左到右      | `… + …`     |
| [减法](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Arithmetic_Operators#Subtraction) | `… - …`                                                      |               |             |
| 12                                                           | [按位左移](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Bitwise_Operators) | 从左到右      | `… << …`    |
| [按位右移](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Bitwise_Operators) | `… >> …`                                                     |               |             |
| [无符号右移](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Bitwise_Operators) | `… >>> …`                                                    |               |             |
| 11                                                           | [小于](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Comparison_Operators#Less_than_operator) | 从左到右      | `… < …`     |
| [小于等于](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Comparison_Operators#Less_than__or_equal_operator) | `… <= …`                                                     |               |             |
| [大于](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Comparison_Operators#Greater_than_operator) | `… > …`                                                      |               |             |
| [大于等于](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Comparison_Operators#Greater_than_or_equal_operator) | `… >= …`                                                     |               |             |
| [in](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/in) | `… in …`                                                     |               |             |
| [instanceof](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/instanceof) | `… instanceof …`                                             |               |             |
| 10                                                           | [等号](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Comparison_Operators#Equality) | 从左到右      | `… == …`    |
| [非等号](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Comparison_Operators#Inequality) | `… != …`                                                     |               |             |
| [全等号](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Comparison_Operators#Identity) | `… === …`                                                    |               |             |
| [非全等号](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Comparison_Operators#Nonidentity) | `… !== …`                                                    |               |             |
| 9                                                            | [按位与](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Bitwise_Operators#Bitwise_AND) | 从左到右      | `… & …`     |
| 8                                                            | [按位异或](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Bitwise_Operators#Bitwise_XOR) | 从左到右      | `… ^ …`     |
| 7                                                            | [按位或](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Bitwise_Operators#Bitwise_OR) | 从左到右      | `… | …`     |
| 6                                                            | [逻辑与](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Logical_Operators#Logical_AND) | 从左到右      | `… && …`    |
| 5                                                            | [逻辑或](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Logical_Operators#Logical_OR) | 从左到右      | `… || …`    |
| 4                                                            | [条件运算符](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Conditional_Operator) | 从右到左      | `… ? … : …` |
| 3                                                            | [赋值](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Assignment_Operators) | 从右到左      | `… = …`     |
| `… += …`                                                     |                                                              |               |             |
| `… -= …`                                                     |                                                              |               |             |
| `… *= …`                                                     |                                                              |               |             |
| `… /= …`                                                     |                                                              |               |             |
| `… %= …`                                                     |                                                              |               |             |
| `… <<= …`                                                    |                                                              |               |             |
| `… >>= …`                                                    |                                                              |               |             |
| `… >>>= …`                                                   |                                                              |               |             |
| `… &= …`                                                     |                                                              |               |             |
| `… ^= …`                                                     |                                                              |               |             |
| `… |= …`                                                     |                                                              |               |             |
| 2                                                            | [yield](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/yield) | 从右到左      | `yield …`   |
| [yield*](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/yield*) | `yield* …`                                                   |               |             |
| 1                                                            | [展开运算符](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Spread_operator) | n/a           | `...` …     |
| 0                                                            | [逗号](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Comma_Operator) | 从左到右      | `… , …`     |