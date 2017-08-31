---
layout: post
sitemap: false
noindex: true
title: C# 7 Tuples
categories: charp tuples valuetuples
---

```
dotnet new console -o TestValueTuple
cd TestValueTuple
dotnet add package System.ValueTuple
```

```
C:\codeExamples\TestValueTuple>dir

 Directory of C:\codeExamples\TestValueTuple

15/06/2017  17:54    <DIR>          .
15/06/2017  17:54    <DIR>          ..
15/06/2017  17:54               196 Program.cs
15/06/2017  17:56               276 TestValueTuple.csproj
               2 File(s)            472 bytes
               2 Dir(s)  10.289.864.704 bytes free

C:\codeExamples\TestValueTuple>
```

```
C:\codeExamples\TestValueTuple>type TestValueTuple.csproj
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>netcoreapp1.1</TargetFramework>
  </PropertyGroup>
  <ItemGroup>
    <PackageReference Include="System.ValueTuple" Version="4.3.1" />
  </ItemGroup>
</Project>
C:\codeExamples\TestValueTuple>
```

```
dotnet restore
```

```csharp
using System.ValueTuple;
```