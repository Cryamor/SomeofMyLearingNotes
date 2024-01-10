# C# 学习

官方文档：[C#文档](https://learn.microsoft.com/zh-cn/dotnet/csharp)

## C#程序结构

```csharp
using System;
namespace Program
{
	class Hello
    {
        static void Main(string[] args){
            Console.WrieLine("Hello World");
            Console.ReadKey();
        }
    }
}
```

- `namespace` 关键词用于声明一个命名空间，用 `using` 来引入，命名空间包括类和其他命名空间。例如，`System`命名空间包含`Console`类和其他命名空间（如`IO`），由于`System`的引入，可以将`System.Console.WriteLine`简写为`Console.WriteLine`。
- 约定`Main` 静态方法是整个C#程序的入口，`static`关键字表明为静态方法，可以在不引入特定对象的情况下运行。

## 数据类型

### 值类型

#### 简单类型

| 数据类型 | 描述                                       | 范围                                   |
| -------- | ------------------------------------------ | -------------------------------------- |
| bool     | 布尔值                                     | True or False                          |
| byte     | **8**位无符号整数                          | 0 ~ 255                                |
| sbyte    | **8**位有符号整数                          | -128 ~ 127                             |
| char     | **16**位Unicode字符                        | U+0000 ~ U+FFFF                        |
| short    | **16**位有符号整数                         | -32768 ~ 32767                         |
| ushort   | **16**位无符号整数                         | 0 ~ 65535                              |
| int      | **32**位有符号整数                         | -2147483648 ~ 2147483647               |
| uint     | **32**位无符号整数                         | 0 ~ 4294967295                         |
| long     | **64**位有符号整数                         | -2^63^ ~ 2^63^-1                       |
| ulong    | **64**位无符号整数                         | 0 ~ 2^64^-1                            |
| float    | **32**位单精度浮点数                       | -3.4\*10^38^ ~ 3.4*10^38^              |
| double   | **64**位双精度浮点数                       | (+/-)5.0\*10^-324^ ~ (+/-)1.7\*10^308^ |
| decimal  | **128**位精确十进制值，**28~29**个有效位数 | -7.9\*10^28^ ~ 7.9\*10^28^ / 10^0~28^  |

#### 枚举类型

`enum E {...}`格式的用户定义类型，其基础类型必须是八种整型之一

#### 结构类型

`struct S {...}`格式的用户定义类型，同C/C++

#### 元组类型

`(T1, T2, T3, ...)`格式的用户定义类型

#### 可为`null`的值类型

任何类型变量都可被声明为“可为null“或”不可为null“，前者在变量类型后加一个`?`，其取值包含一个额外的null值，即没有值，如：

```csharp
int? a = 10;
int? b = null;
a = a + b;         // a is null
int c = b ?? a;    // c is 10
int d = a ?? 20;   // d is 10
bool? flag = null;
// An array of a nullable value type:
int?[] arr = new int?[10];
```

- `??`是null合并运算符，若左操作数不为null则返回左操作数，否则返回右操作数。
- 对于比较运算符，若一个或全部两个操作数都为 `null`，则结果为 `false`；否则，将比较操作数的包含值。
- 对于`==`，如果两个操作数都为 `null`，则结果为 `true`；如果只有一个操作数为 `null`，则结果为 `false`；否则，将比较操作数的包含值。
- 对于`!=`，如果两个操作数都为 `null`，则结果为 `false`；如果只有一个操作数为 `null`，则结果为 `true`；否则，将比较操作数的包含值。

### 引用类型

#### 类类型

- 所有类的最终基类为`object`
- `class C {...}`格式的用户定义类型

#### 字符串`String`

从 `System.Object` 派生出的 `System.String` 类，有两种声明方式：

```csharp
//使用引号的声明方式
String str = "123";
```

```csharp
//使用@加引号的声明形式
@"123";
/* 使用@" "形式声明的字符串称为“逐字字符串”，逐字字符串会将转义字符\当作普通字符对待，例如String str = @"C:\Windows";等价于String str = "C:\\Windows";
而且可以任意使用换行，换行符及缩进空格等都会计算在字符串的长度之中
*/
```

#### 数组类型

##### 声明与赋值

```c#
int[] arr1;
arr1 = new int[10];
int[] arr2 = new int[5]{0,1,2,3,4};
int[] arr3 = new inr[]{0,1,2};
```

##### `foreach`遍历

```csharp
foreach(int i in arr2){
	Console.WriteLine(i);
}
```

##### 多维数组

```csharp
int[,] arr = new int[3,3];
int[,,] arr2 = new int[3,3,3];
```

##### 交错数组

交错数组即元素为数组的数组，每个元素可以为大小和维度不同的数组。

```csharp
//声明一个具有三个元素的一维交错数组，每个元素都是一个一维int数组
int[][] jaggedArray = new int[3][];
//初始化
jaggedArray[0] =  new int[4];
jaggedArray[1] =  new int[]{1,2,3};
jaggedArray[2] =  new int[2];

//定义一个包含三个二维数组元素的一维数组
int[][,] jarr = new int[3][,]{
    new int[,]{{1,1},{2,2}},
    new int[,]{{3,3},{4,4}},
    new int[,]{{5,5},{6,6}}
};
int jarr[0][1,1] = 1;
```

#### 指针 *

同C/C++

#### 动态类型`dynamic`

存储任何类型的值，类型检查在程序运行时进行的

```csharp
dynamic d = 10
```

## 基础操作

#### 声明、初始化变量

同C/C++

#### 输入输出

```csharp
Console.Write("Hello World!"); //不自动换行
Console.WriteLine("Hello World!"); //自动换行
Console.Read(); //读取输入流下一个字符，返回int，若没有下一个字符则返回-1
Console.ReadKey(); //读取用户下一个按下的键（显示在窗口中），返回ConsoleKeyInfo对象
Console.Readline(); //返回string
```

#### 字符串内插

用`$`声明内插字符串，计算`{`和`}`之间的表达式，将结果转换为`string`并替换，例如：

```csharp
Console.WriteLine($"The low and high temperature on {weatherData.Date:MM-dd-yyyy}");
Console.WriteLine($"was {weatherData.LowTemp} and {weatherData.HighTemp}.");
// Output (similar to):
// The low and high temperature on 08-11-2020
// was 5 and 30.
```

#### 判断语句与循环语句

- `if else`,`switch case`,`while`,`do while`,`break`,`continue`,`goto`用法同C/C++
- `foreach`循环见“数组类型”部分

### 函数/方法

#### 可访问性

- `public`：访问不受限制。
- `private`：访问仅限于此类。
- `protected`：访问仅限于此类或派生自此类的类。
- `internal`：仅可访问当前程序集（`.exe` 或 `.dll`）。
- `protected internal`：仅可访问此类、从此类中派生的类，或者同一程序集中的类。
- `private protected`：仅可访问此类或同一程序集中从此类中派生的类。

#### 方法

**方法**是实现对象或类可执行的计算或操作的成员。

 **静态方法**是通过类进行访问，使用 `static` 修饰符声明。 静态方法不对特定的实例起作用，只能直接访问静态成员。在静态方法中引用 `this` 会生成错误。

**实例方法**是通过类实例进行访问，对特定的实例起作用，并能够访问静态和实例成员。 其中调用实例方法的实例可以作为 `this` 显式访问。

当方法主体是单个表达式时，可使用紧凑表达式格式定义方法，如：

```csharp
public override string ToString() => "This is an object";
```

#### 参数

 有四类参数：值参数、引用参数、输出参数和参数数组。

##### 引用参数

引用参数指出的存储位置与自变量相同，使用 `ref` 修饰符进行声明。

```c#
static void Swap(ref int x, ref int y)
{
    int temp = x;
    x = y;
    y = temp;
}

public static void SwapExample()
{
    int i = 1, j = 2;
    Swap(ref i, ref j);
    Console.WriteLine($"{i} {j}");    // "2 1"
}
```

##### 输出参数

输出参数与引用参数类似，但不要求向调用方提供的自变量显式赋值。 输出参数使用 `out` 修饰符进行声明。

```c#
static void Divide(int x, int y, out int quotient, out int remainder)
{
    quotient = x / y;
    remainder = x % y;
}

public static void OutUsage()
{
    Divide(10, 3, out int quo, out int rem);
    Console.WriteLine($"{quo} {rem}");	// "3 1"
}
```

##### 参数数组

向方法传递数量不定的自变量。 参数数组使用 `params` 修饰符进行声明。 参数数组只能是方法的最后一个参数，且参数数组的类型必须是一维数组类型。如`System.Console`类的 `Write` 和 `WriteLine` 方法：

```c#
public class Console
{
    public static void Write(string fmt, params object[] args) { }
    public static void WriteLine(string fmt, params object[] args) { }
    // ...
}
```

### 文件和IO流

#### 流操作

##### 常用流类

- [FileStream](https://learn.microsoft.com/zh-cn/dotnet/api/system.io.filestream) - 用于对文件进行读取和写入操作。
- [IsolatedStorageFileStream](https://learn.microsoft.com/zh-cn/dotnet/api/system.io.isolatedstorage.isolatedstoragefilestream) - 用于对独立存储中的文件进行读取或写入操作。
- [MemoryStream](https://learn.microsoft.com/zh-cn/dotnet/api/system.io.memorystream) - 用于作为后备存储对内存进行读取和写入操作。
- [BufferedStream](https://learn.microsoft.com/zh-cn/dotnet/api/system.io.bufferedstream) - 用于改进读取和写入操作的性能。
- [NetworkStream](https://learn.microsoft.com/zh-cn/dotnet/api/system.net.sockets.networkstream) - 用于通过网络套接字进行读取和写入。
- [PipeStream](https://learn.microsoft.com/zh-cn/dotnet/api/system.io.pipes.pipestream) - 用于通过匿名和命名管道进行读取和写入。
- [CryptoStream](https://learn.microsoft.com/zh-cn/dotnet/api/system.security.cryptography.cryptostream) - 用于将数据流链接到加密转换。

##### `FileStream`类

详见[官方文档](https://learn.microsoft.com/zh-cn/dotnet/api/system.io.filestream)

###### 创建`FileStream`对象

```c#
FileStream <object_name> = new FileStream(<file_name>, <FileMode>, <FileAccess>, <FileShare>);
```

参数：

- `file_name`：文件的相对或绝对路径。
- `FileMode`：文件打开方式，可选：
  - `FileMode.Append`：查找到文件末尾，或创建一个新文件，只能与 `FileAccess.Write` 一起使用。
  - `FileMode.Create`：创建新文件。 如果此文件已存在，则会将其覆盖。如果该文件已存在但为隐藏文件，则将引发 `UnauthorizedAccessException`异常。
  - `FileMode.CreateNew`：指定操作系统应创建新文件。如果文件已存在，则将引发 `IOException`异常。
  - `FileMode.Open`：打开文件，如果文件不存在，引发一个 `FileNotFoundException` 异常。
  - `FileMode.OpenOrCreate`：打开文件，如果文件不存在，则创建新文件并打开。
  - `FileMode.Truncate`：打开文件，如果文件不存在，引发一个 `FileNotFoundException` 异常。该文件被打开时，将被截断为零字节大小（即清空）。 尝试从使用 `FileMode.Truncate` 打开的文件中进行读取将导致 `ArgumentException` 异常。
- `FileAccess`：文件访问方式，有`FileAccess.Read`，`FileAccess.Write`和`FileAccess.ReadWrite`。
- `FileShare`：控制其他`FileStream`对象对同一文件可以具有的访问类型，可选：
  - `FileShare.Delete`：允许随后删除文件。
  - `FileShare.Inheritable`：使文件句柄可由子进程继承。 Win32 不直接支持此功能。
  - `FileShare.Read`：允许随后打开文件读取。 如果未指定此标志，则文件关闭前，任何打开该文件以进行读取的请求（由此进程或另一进程发出的请求）都将失败。 但是，即使指定了此标志，仍可能需要附加权限才能够访问该文件。
  - `FileShare.Write`：允许随后打开文件读取或写入。
  - `FileShare.ReadWrite`：允许随后打开文件写入。 
  - `FileShare.None`：文件关闭前，打开该文件的任何请求（由此进程或另一进程发出的请求）都将失败。

###### 常用方法

| 方法                                            | 描述                                             |
| ----------------------------------------------- | ------------------------------------------------ |
| Close()                                         | 关闭流，释放关联的所有资源                       |
| CopyTo(Stream destination)                      | 当前流中读取字节并写入另一流中                   |
| Dispose()                                       | 释放由占用的非托管资源，还可以另外再释放托管资源 |
| Flush()                                         | 清除缓冲区，使得缓冲数据全部写入文件             |
| Lock(long position, long length)                | 防止其他进程读写FileStream                       |
| Unlock(long position, long length)              | 允许其他进程访问之前所锁定的文件的全部或部分     |
| Read(byte[] buffer, int offset, int count)      | 从流中读取字节块并将该数据写入给定缓冲区中       |
| ReadByte()                                      | 从文件中读取一个字节，并将读取位置提升一个字节   |
| Write(byte[] buffer, int offset, int count)     | 将字节块写入文件流                               |
| WriteByte (byte value)                          | 一个字节写入文件流中的当前位置                   |
| Seek (long offset, System.IO.SeekOrigin origin) | 将该流的当前位置设置为给定值                     |

###### 示例

```c#
using System;
using System.IO;

namespace Project1
{
    class Class1
    {
        static void Main(string[] args)
        {
           
            FileStream file = new FileStream("test.txt", FileMode.OpenOrCreate, FileAccess.ReadWrite);
            for (int i = 0; i < 10; i++)
                file.WriteByte((byte)i);
            file.Seek(0,SeekOrigin.Begin); //same as file.Position = 0;
            for (int i = 0; i < 10; i++)
                Console.Write(file.ReadByte() + " ");
            file.Close();
            Console.ReadKey();
        }

    }
}
```

##### 二进制文件读写

可使用`BinaryReader`和`BinaryWriter`，见[BinaryReader官方文档](https://learn.microsoft.com/zh-cn/dotnet/api/system.io.binaryreader)和[BinaryWriter官方文档](https://learn.microsoft.com/zh-cn/dotnet/api/system.io.binarywriter)。

#### 文件和目录操作

##### 常用文件和目录类

- [File](https://learn.microsoft.com/zh-cn/dotnet/api/system.io.file) - 提供用于创建、复制、删除、移动和打开文件的静态方法，并可帮助创建 [FileStream](https://learn.microsoft.com/zh-cn/dotnet/api/system.io.filestream) 对象。
- [FileInfo](https://learn.microsoft.com/zh-cn/dotnet/api/system.io.fileinfo) - 提供用于创建、复制、删除、移动和打开文件的实例方法，并可帮助创建 [FileStream](https://learn.microsoft.com/zh-cn/dotnet/api/system.io.filestream) 对象。
- [Directory](https://learn.microsoft.com/zh-cn/dotnet/api/system.io.directory) - 提供用于创建、移动和枚举目录和子目录的静态方法。
- [DirectoryInfo](https://learn.microsoft.com/zh-cn/dotnet/api/system.io.directoryinfo) - 提供用于创建、移动和枚举目录和子目录的实例方法。
- [Path](https://learn.microsoft.com/zh-cn/dotnet/api/system.io.path) - 提供用于以跨平台的方式处理目录字符串的方法和属性。

#### 压缩操作

[System.IO.Compression](https://learn.microsoft.com/zh-cn/dotnet/api/system.io.compression) 命名空间包含用于对文件和流进行压缩或解压缩的类型：

- [ZipArchive](https://learn.microsoft.com/zh-cn/dotnet/api/system.io.compression.ziparchive) - 用于在 zip 存档中创建和检索条目。
- [ZipArchiveEntry](https://learn.microsoft.com/zh-cn/dotnet/api/system.io.compression.ziparchiveentry) - 用于表示压缩文件。
- [ZipFile](https://learn.microsoft.com/zh-cn/dotnet/api/system.io.compression.zipfile) - 用于创建、提取和打开压缩包。
- [ZipFileExtensions](https://learn.microsoft.com/zh-cn/dotnet/api/system.io.compression.zipfileextensions) - 用于创建和提供压缩包中的条目。
- [DeflateStream](https://learn.microsoft.com/zh-cn/dotnet/api/system.io.compression.deflatestream) - 用于使用 Deflate 算法对流进行压缩和解压缩。
- [GZipStream](https://learn.microsoft.com/zh-cn/dotnet/api/system.io.compression.gzipstream) - 用于采用 gzip 数据格式对流进行压缩和解压缩。



## 一些新操作

### Lambda 表达式

Lambda 表达式使用`=>`符号（lambda声明运算符）

#### 表达式 lambda

```
(input-parameters) => expression
```

#### 语句 lambda

语句要在大括号中

```c#
Action<string> greet = name =>
{
    string greeting = $"Hello {name}!";
    Console.WriteLine(greeting);
};
greet("World");
// Output:
// Hello World!
```

#### 输入参数

参数在小括号内，用逗号分隔。

使用空括号指定零个输入参数：

```c#
Action line = () => Console.WriteLine();
```

若只有一个输入参数，则括号是可选的：

```c#
Func<double, double> cube = x => x * x * x;
```

```c#
Func<int,int,int> Add = (x,y) => x+y;
Console.WriteLine(Add(2,3));
//Output: 5
```

```c#
Func<int, string, bool> isTooLong = (int x, string s) => s.Length > x;
```

### LINQ

