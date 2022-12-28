# Value Types and Refrenece Types 알아보기

Cost of Heap vs Stack Allocation
- Heap allocations and deallocations costs are way larger than the stack ones

Cost of Copying
- Since reference types do not directly store their data, we only incur reference counting costs when copying them. 
- Things become interesting when we mix value and reference types.


Struct vs. Class
- 생성 퍼포먼스: Struct > Class (약 100배?)
- 대입(assignment) 퍼포먼스: Struct < Class

## 출처
- [Value Types and Reference Types in Swift](https://www.vadimbulavin.com/value-types-and-reference-types-in-swift/)