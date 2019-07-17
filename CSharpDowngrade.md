# C# .Net Framework 降级

> So begins a new age of knowledge

---

## 前言

  我们的项目是基于`.Net Framework 4.6`构建的，现在需要将程序运行在xp系统上，所以需要将.Net Framework从4.0降级到4.6

---

## 实现方式

  我们的项目是基于vs2015或者vs2017构建的项目，项目文件(*.csproj)默认为如下格式

### *Class Library 项目*

```xml
<?xml version="1.0" encoding="utf-8"?>
<Project ToolsVersion="15.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
  <Import Project="$(MSBuildExtensionsPath)\$(MSBuildToolsVersion)\Microsoft.Common.props" Condition="Exists('$(MSBuildExtensionsPath)\$(MSBuildToolsVersion)\Microsoft.Common.props')" />
  <PropertyGroup>
    <Configuration Condition=" '$(Configuration)' == '' ">Debug</Configuration>
    <Platform Condition=" '$(Platform)' == '' ">AnyCPU</Platform>
    <ProjectGuid>dd544cb8-b1da-4c6b-a434-ab9e3e63eefb</ProjectGuid>
    <OutputType>Library</OutputType>
    <AppDesignerFolder>Properties</AppDesignerFolder>
    <RootNamespace>ClassLibraryProject</RootNamespace>
    <AssemblyName>ClassLibraryProject</AssemblyName>
    <TargetFrameworkVersion>v4.6</TargetFrameworkVersion>
    <FileAlignment>512</FileAlignment>
    <Deterministic>true</Deterministic>
  </PropertyGroup>
  <PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Debug|AnyCPU' ">
    <DebugSymbols>true</DebugSymbols>
    <DebugType>full</DebugType>
    <Optimize>false</Optimize>
    <OutputPath>bin\Debug\</OutputPath>
    <DefineConstants>DEBUG;TRACE</DefineConstants>
    <ErrorReport>prompt</ErrorReport>
    <WarningLevel>4</WarningLevel>
  </PropertyGroup>
  <PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Release|AnyCPU' ">
    <DebugType>pdbonly</DebugType>
    <Optimize>true</Optimize>
    <OutputPath>bin\Release\</OutputPath>
    <DefineConstants>TRACE</DefineConstants>
    <ErrorReport>prompt</ErrorReport>
    <WarningLevel>4</WarningLevel>
  </PropertyGroup>
  <ItemGroup>
    <Reference Include="System"/>
    <Reference Include="System.Core"/>
    <Reference Include="System.Xml.Linq"/>
    <Reference Include="System.Data.DataSetExtensions"/>
    <Reference Include="Microsoft.CSharp"/>
    <Reference Include="System.Data"/>
    <Reference Include="System.Net.Http"/>
    <Reference Include="System.Xml"/>
  </ItemGroup>
  <ItemGroup>
    <Compile Include="Class1.cs" />
    <Compile Include="Properties\AssemblyInfo.cs" />
  </ItemGroup>
  <Import Project="$(MSBuildToolsPath)\Microsoft.CSharp.targets" />
 </Project>

```

我们降级，主要的就是修改这个项目文件为新的组织方式

尝试如下操作

1. 项目右键->卸载项目
2. 项目右键->编辑ClassLibraryProject.csproj
3. 备份ClassLibraryProject.csproj
4. 将ClassLibraryProject.csproj文件内容删除，并贴入一下代码
```xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <TargetFrameworks Condition="'$(LibraryFrameworks)'==''">net46;net40</TargetFrameworks>
  </PropertyGroup>
</Project>
```
5. 右键项目->重新加载项目

此时，打开项目，你可以看到你的项目发生了如下变化
![project-change](https://cdn.sinaimg.cn.52ecy.cn/large/005BYqpgly1g51tku4btqj30l309h74p.jpg)
此时，项目已经是一个40、46共存的项目了

## 剩余问题

### 1. AssemblyInfo.cs

新的项目组织方式，相比于旧的组织方式，去掉了很多


