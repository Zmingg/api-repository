# API Specification Repository
## 前言

本文档基于 swagger 2.0 编写，旨在提供一些规范和命名参考准则，请务必在编写或修改
API定义之前仔细阅读本文档。

## 文件命名规则

定义API文件应当遵循：

* yaml 定义文件该统一使用小写字母命名
* 多个单词之间以中划线分割

形如：
```
example.yaml
```

```
some-module.yaml
```

## 定义命名规范

### 路径名

URL 全部以小写字母和中划线命名，禁止出现大写字母：

```
/examinations
```

资源名以复数形式命名，其他约定请参见 Restful 命名风格

### 实体名

使用 CamelCase，形如：

```
SomeEntiy
```



### 属性命名

使用 lowerCamelCase，形如：

```
someProperty
```

代码生成工具会自动生成符合语言命名规则的代码，比如在 JavaScript、Java 中是 
‘someProperty’；在 C# 中则变成了 ‘SomeProperty’。

最终输出的 JSON 应当仍是 lowerCamelCase 形式。

### 资源操作

#### Http 方法

* 新建: post，请求参数在 body 中定义
* 修改：put，请求参数在 body 中定义
* 删除：delete，请求参数在 body 中定义
* 查询：get，请求参数在 query 中定义

####  操作标识符

每个接口必须要定义operationId，常见的命名规则如下：

###### 新建：
 create 前缀 + 资源单数形式，如：createPet
 
#### 修改：
 * 修改单个模型时使用 update 前缀 + 资源单数形式，如：updatePet
 * 批量修改多个模型时使用 update 前缀 + 资源单数形式，如：updatePets

#### 删除：
 * 删除单个模型时使用 update 前缀 + 资源单数形式，如：deletePet
 * 批量删除多个模型时使用 update 前缀 + 资源单数形式，如：deletePets
 
#### 查询：
 * 返回结果是多个模型时使用 find 前缀 + 资源复数形式，如：findPets，表示从集合查询多个对象
 * 返回结果是单一模型时使用 get 前缀 + 资源单数形式，如：getPet，表示查询单个对象

## 参数及模型属性

请求参数和模型中的 id 属性必须要设置 type 为 integer，format 则指定为 int64 
以对应静态语言中的 long 型

```
- name: id
  in: path
  type: integer
  format: int64
  description: Primary key
```

### 输入数据传输模型
~~DTO 即 Data Transfer Object，用于传输数据，不应该包含业务逻辑~~

~~用以客户端向服务端提交数据的模型：资源名 + DTO，即数据传输模型，如：PetDTO~~


### 输出视图模型
VO 即 View Object，属于DTO的一种，用于视图呈现使用

用以服务端向客户端返回数据的模型
资源名 + VO，即视图模型，如：PetVO

### 响应模型

以：前缀 + Response 命名。

## 跨文件引用

Swagger 定义文件支持跨文件引用，例如：

```
$ref: 'http://apispec.aecg.com.cn/common.yaml#/definitions/Response'
```

请求参数，响应体，实体对象和对象属性等都可以跨文件引用。

Swagger 的解析器会自动请求 URL 定义文件合并处理。

__请注意引用的URL必须是完整的 URL 地址，不支持相对路径形式的文件引用。__

公共定义尽可能抽取出来，避免重复编所写带来的维护成本。
所有跨项目的公共定义在 /common.yaml 中定义。

## 文件编码

所有 yaml 文件必须使用 UTF-8编码，如果中文字符出现乱码请检查文件的编码是否是UTF-8.
