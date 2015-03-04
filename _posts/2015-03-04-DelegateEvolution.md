---
layout: post
title:Evolution of the C# Delegate
---

After some recent technical conversations I was prompted to create some notes on delegates and how they have changed through the evolution of the C# language.

*What is a delegate?*

A reference type variable the holds a reference to a method and this reference can be changed at run-time. A function pointer. They are type-safe.

*C# 1.0*

In the earliest version of the C# language the syntax for writing a simple delegate was  verbose and hard to understand.  A delegate type declaration, a named method and delegate instance were all required.

```csharp
    public class DelegateDemo
    {
        delegate int Sum( int a, int b);
        public int AddNumbers( int a, int b)
        {
            return a + b;
        }

        public void DoWork()
        {
            Sum d1 = new Sum(AddNumbers);
            var result = d1(2, 2);
        }
    }
```

*C# 2.0*

C# 2.0 introduced Method Group Conversions and Anonymous Methods. 

Method Group Conversions allowed the code to simplified by removing the requirement for the new keyword during the instantiation  (the compiler looks after this)

```csharp
public class DelegateDemo
{
    delegate int Sum( int a, int b);
    public int AddNumbers( int a, int b)
    {
        return a + b;
    }

    public void DoWork()
    {
        Sum d1 = AddNumbers;
        var result = d1(2, 2);
    }
}
```

Anonymous methods simplified the code by allowing business logic to be implemented as part of the delegate body when the delegate is instantiated removing the requirement for the named method.  

```csharp
public class DelegateDemo
{
    private delegate int Sum (int a, int b);

    public void DoWork()
    {
        Sum d1 = delegate ( int a, int b) { return a + b; };
        var result = d1(2, 2);
    }
}    
```

*C# 3.5*

C# 3.5 introduced Lambda expressions which are further evolution of the anonymous method syntax.  The steps to achieve the required functionality do not change but the code is less verbose with the compiler determining all the required information (parameter types, return types etc.)

```csharp
public class DelegateDemo
{
    private delegate int Sum (int a, int b);

    public void DoWork()
    {
        Sum d1 = (a, b) => a + b;
        var result = d1(2, 2);
    }
}
```

 C# 3.5 also introduced a set of Func and Action delegates which can be used instead of a custom delegate.  

```csharp
public class DelegateDemo
{
    public void DoWork()
    {
        Func <int , int, int > d1 = (a, b) => a + b;
        var result = d1(2, 2);
    }
}
```

