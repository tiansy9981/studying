# YAML文件格式及编写注意事项

## **1、YAML文件格式**

- YAML是一种标记性语言，它可以很直观的展示数据序列化格式，可读性高。
- 类似于json数据描述语言，但是语法要比json简单很多。
- YAML数据结构通过缩进来表示，连续的项目通过减号来表示，键值对用冒号分隔，数组用中括号[ ] 括起来，bash用花括号{ } 括起来。

## **2、YAML格式的注意事项**

- 不支持制表符tab键缩进，只能使用空格缩进
- 通常开头缩进2个空格
- 字符后缩进1个空格，如冒号【：】、逗号【，】、横杠【-】
- 用#号表示注释
- 如果包含特殊字符用单引号【’ '】引起来作为普通字符，如果用双引号【“ ”】表示特殊字符本身的意思，
- 布尔值必须用【“ ”】括起来
- YAML区分大小写

## **3、YAML数据结构案例**

```yaml
#键值对表示

animal:pets

#数组:一组按次序排列的列表

- cat
- dog
- goldfish

#布尔值
debug: "true"
debug: "false"

#yaml实例
languages:        #序列的映射 

  - java
  - Golang
  - Python

websites:         #映射的映射
  Baidu: www.baidu.com
  Wangyi: www.163.com
  Souhu: www.souhu.com

#或者
languages: ["java","Golong","Python"]
websites:
  Baidu:
    www.baidu.com
  Wangyi:
    www.163.com
  Souhu:
    www.souhu.com

#Json格式
{
  languages: [
    'Java',
    'Golong',
    'Python',
  ],
  websites: [
    Baidu: 'www.baidu.com',
    Wangyi: 'www.163.com',
    Souhu: 'www.souhu.com',
  ]
}
```

