# 30. LINQ

## LINQ 의 개요
> LINQ는 Languate INtegrated Query의 약어로, C# 에서 컬렉션 형태의 데이터를 가공할 떄 유용한 메서드를 제공

###  확장 메서드 사용하기
```csharp
using System;
using System.Linq; // Linq 사용하기 위한 네임스페이스 선언

class LinqSum
{
    static void Main()
    {
        int[] numbers = {1, 2, 3};

        int sum = numbers.Sum();

        Console.WriteLine($"numbers 배열 요소 합 : {sum}");
    }
}
```
