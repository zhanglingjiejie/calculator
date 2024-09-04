# typescript-treasure

Elegant and pragmatic collection of typescript tools

## Installation
```bash
npm install typescript-treasure -S
```

## Document
<a href="https://autumn-one.github.io/typescript-treasure-doc/" target="_blank">中文文档</a>

## ts type code specification
I call this code specification：**MuGuaTS**（Chinese interpretation：**木瓜TS**）

**All judgment tools are named using If_Xxx**
For example, `If_Num<T>` is a generic tool used to determine if it is a number.

**Short generic tools can be written on one line**

```typescript
type SomeTool<T> =  T extends any ? true : false;
```

**More complex generic tools need to wrap lines after the equal sign and follow the following wording**

```typescript
type ComplexTool<T1, T2> =
    If_NumStr<T1> extends true ?
    (
        If_Includes<T2, 2> extends true ?
        (
            SomeThing
        )
        :
        (
           SomeThing
        )
    )
    :
    (
        SomeThing
    )
```

**The generic parameter part of a generic function should follow the equal sign**

```typescript
type SomeFn = <T1, T2>
    (arg1: T1, arg2: T2) =>
    (
        T1 extends any ?
        (
            SomeThing
        )
        :
        (
            SomeThing
        )
    )
```



