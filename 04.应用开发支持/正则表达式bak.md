
要在Java中有效地使用正则表达式，需要了解其语法。语法非常广泛，能够使我们编写非常高级的正则表达式。要完全掌握语法可能需要大量的练习。

这里不讨论语法的每一个细节，而是关注使用正则表达式需要理解的主要概念。要获得完整的解释，请参见[JavaDoc页面`Pattern`类](http://docs.oracle.com/javase/8/docs/api/java/util/regex/Pattern.html)。

## 基本语法
在讲述Java正则表达式的特性之前，这里将简要介绍一下正则表达式语法基础知识。

### 字符

正则表达式的最基本形式是只匹配某些字符的表达式。下面是一个例子:

```
John
```

这个简单的正则表达式将匹配给定输入文本中的字符串"John"

您可以在正则表达式中使用字母表中的任何字符。还可以通过八进制、十六进制或unicode代码引用字符。如下:

```
\0101
\x41
\u0041
```

这三个表达都指的是大写的"A"字符。第一个使用八进制代码(`101`)来表示`A`，第二个使用十六进制代码(`41`)，第三个使用unicode代码(`0041`)。

以下是字符匹配的说明

| Construct | Matches                                                      |
| --------- | ------------------------------------------------------------ |
| `x`       | 字符x。字母表中的任何字符都可以用来代替x。 |
| `\\`      | 反斜杠字符。一个反斜杠用作转义字符，与其他字符一起表示特殊的匹配，因此要仅匹配反斜杠字符本身，需要使用一个反斜杠字符进行转义。因此使用双反斜杠来匹配单个反斜杠字符。 |
| `\0n`     | 具有八进制值的字符。n必须在0和7之间。 |
| `\0nn`    | 具有八进制值的字符。n必须在0和7之间。 |
| `\0mnn`   | 具有八进制值的字符。m必须在0和3之间，n必须在0和7之间。 |
| `\xhh`    | 具有十六进制值的字符。h必须为16进制的字符0~9 A~F           |
| `\uhhhh`  | 具有十六进制值'0xhhhh'的字符。这个结构用于匹配unicode字符。 |
| `\t`      | 制表符                                          |
| `\n`      | 换行符 (unicode: `'\u000A'`).     |
| `\r`      | 回车字符(unicode: `'\u000D'`)。        |
| `\f`      | 换页符 (unicode: `'\u000C'`).               |
| `\a`      | 警报(铃声)字符 (unicode: `'\u0007'`).            |
| `\e`      | esc (unicode: `'\u001B'`).                  |
| `\cx`     | 对应于`x`的控制字符，表示 control + x对应的字符。例如`\cI` 匹配 `control + I`，等价于 `\t`               |


### 字符类

构造字符类可以使我们能够针对多个字符指定匹配，而不仅仅是一个字符。换句话说，字符类将输入文本中的单个字符与字符类中允许的多个字符进行匹配。例如，可以像这样匹配字母`a`、`b`或`c`:

```
[abc]
```

字符类嵌套在一对方括号`[]`中。括号本身不是要匹配的内容的一部分。

使用字符类查找单词"John"或"john":

```
[Jj]ohn
```

字符类`[Jj]`将匹配`J`或`J`，表达式的其余部分将匹配该序列中的字符`ohn`。

以下是一系列的字符类的语法。


| Construct       | Matches                                                      |
| --------------- | ------------------------------------------------------------ |
| `[abc]`         | 匹配`a`, `b`, `c`。这被称为简单匹配(simple class)，它匹配`[]`中出现的任何字符。 |
| `[^abc]`        | 匹配除`a`、`b`和`c`之外的任何字符。这是一个否定匹配(negation)。 |
| `[a-zA-Z]`      | 匹配从`a`到`z`或从`A`到`Z`的任何字符，包括`a`、`a`、`z`和`z`。叫做范围匹配(range)。 |
| `[a-d[m-p]]`    | 匹配从`a`到`d`或从`m`到`p`的任何字符。这叫做并集匹配(union)。 |
| `[a-z&&[def]]`  | 匹配`d`，` e`或`f`这称为交集（intersection）(这里是范围`a-z`和字符`def`之间的交集)。 |
| `[a-z&&[^bc]]`  | 匹配从`a`到`z`的所有字符，除了`b`和`c`。这叫做减法（subtraction）。 |
| `[a-z&&[^m-p]]` | 匹配从`a`到`z`的所有字符，除了从`m`到`p`的字符。这也叫做减法（subtraction）。 |

### 预定义的字符类

Java正则表达式语法有一些可以使用的预定义字符类。例如，`\d`字符类匹配任何数字，`\s`字符类匹配任何空白字符，`\w`字符匹配任何单词字符。

预定义的字符类不必用方括号括起来，但是如果想将它们组合起来，可以这样做:

```
\d
[\d\s]
```

第一个示例匹配任何数字字符。第二个示例匹配任何数字或任何空白字符。

以下是预定义字符类的说明
| Construct | Matches                                                      |
| --------- | ------------------------------------------------------------ |
| `.`       | 匹配任何单个字符。可能匹配也可能不匹配换行符，取决于匹配模式的配置。 |
| `\d`      | 匹配任何数字 [0-9]                                      |
| `\D`      | 匹配任何非数字字符 [^0-9]                       |
| `\s`      | 匹配任何空格字符(空格、制表符、换行符、回车符) |
| `\S`      | 匹配任何非空白字符。                      |
| `\w`      | 匹配任何单词字符。  [A-Za-z0-9_]                                |
| `\W`      | 匹配任何非单词字符。      [^A-Za-z0-9_]                    |

### 数量匹配

量词允许多次匹配给定的表达式或子表达式。例如，下面的表达式匹配字母`A`零或更多次:

```
A*
```

`*`字符是一个量词，表示"零次或多次"。还有一个`+`量词，意思是"一次或多次"，`?`量词的意思是"零或一次"，还有一些其他的量词，可以在本文后面的量词表中看到。

量词可以是"勉强"（reluctant）、"贪婪的"（greedy）或"占有的"（possesive）。

"勉强"使用的量词将尽可能少地匹配输入文本。
"贪婪"量词会尽可能多地匹配输入文本，如果匹配第一条规则后其余部分匹配不到剩余规则，则减少第一条规则的匹配字符，再次尝试匹配后续规则
"占有"量词将尽可能地匹配第一条规则，如果它使表达式的其余部分匹配不到剩余规则，则返回匹配不成功。

例如

```
John went for a walk, and John fell down, and John hurt his knee.
```

"勉强"量词的表达式匹配:

```
John.*?
```

该表达式将匹配单词`John`后面跟着0个或多个字符。`.`表示"任意字符"，而`*`表示"零次或多次"。`*`后面的`?`就成了"勉强"的量词。

作为一个勉强的量词，量词将尽可能少地匹配。因此，该表达式将在上述输入文本中匹配到三次单词“John”。

如果我们把量词换成"贪婪"量词，表达式会是这样的:

```
John.*
```

贪婪量词将匹配尽可能多的字符。现在表达式将只匹配第一个出现的"John"，贪婪量词将尽可能多的匹配输入文本中的其余字符。因此，只找到一个匹配项。

最后，让我们稍微改变一下表达式，让它包含一个"占有"量词:

```
John.*+hurt
```

`*`后面的`+`使它成为一个"占有"量词。

这个表达式匹配上述文本将不会匹配到任何结果，虽然在输入文本中同时找到"John"和"hurt"，但是因为`.*`后面的`+`表示为"占有"匹配。与贪婪量词所做的尽可能多地匹配表达式不同，占有量词尽可能多地匹配，而不管后续表达式是否匹配成功。

`.*+`将匹配输入文本中第一次出现`John`之后的所有字符，包括单词`hurt`。因此，当占有量词后续规则继续匹配时，就没有需要匹配的`hurt`的内容了。

如果将量词更改为贪婪量词，则表达式将匹配输入文本一次


可以尝试使用不同的量词和类型来理解它们是如何工作的。有关量词的完整列表，请参阅以下内容。


| Greedy    | Reluctant  | Possessive | Matches                                         |
| --------- | ---------- | ---------- | ----------------------------------------------- |
| `X?`      | `X??`      | `X?+`      | 匹配X一次，或者根本不匹配(0次或1次)。    |
| `X*`      | `X*?`      | `X*+`      | 匹配X 0次或更多次。                  |
| `X+`      | `X+?`      | `X++`      | 匹配X一次或多次。                  |
| `X{n}`    | `X{n}?`    | `X{n}+`    | 正好匹配X n次。                    |
| `X{n,}`   | `X{n,}?`   | `X{n,}+`   | 至少匹配X n次。                     |
| `X{n, m)` | `X{n, m)?` | `X{n, m)+` | 匹配X至少n次，最多m次。 |

### 边界匹配器
边界匹配器可以匹配如单词之间的边界、输入文本的开头和结尾等。例如，`\w`匹配单词之间的边界，`^`匹配行首，`$`匹配行尾。

下面是一个边界匹配器的例子:

```
^This is a single line$
```

这个表达式只使用"This is a single line"文本匹配一行文本。请注意表达式中的行开始匹配器和行结束匹配器。这些规则规定，除了行首和行尾之外，文本前后不能有任何内容。

#### 多行匹配和单行匹配

边界匹配的列表如下

| Construct | Matches                                                      |
| --------- | ------------------------------------------------------------ |
| `^`       | 匹配一行的开头。                          |
| `$`       | 然后匹配一行结束。                              |
| `\b`      | 匹配单词边界。                               |
| `\B`      | 匹配非单词边界。                                |
| `\A`      | 匹配输入文本的开头。                   |
| `\G`      | Matches the end of the previous match                        |
| `\Z`      | 匹配输入文本的结尾(如果有终止符的话)。 |
| `\z`      | 匹配输入文本的结尾。                        |

### 逻辑操作

Java正则表达式语法还支持一些逻辑运算符(and, or, not)。

and运算符是隐式的。当表达式为`John`时，它的意思是`J`、`o`、`h`和`n`。

or运算符是显式的，使用`|`来编写。例如，表达`John|hurt`会匹配单词`John`或单词`hurt`。

not请参考字符类中的`^`

| Construct | Matches                            |
| --------- | ---------------------------------- |
| `XY`      | 匹配 X 和 Y (X后面必须紧跟着Y). |
| `X|Y`     | 匹配 X 或者 Y.                    |


## Java正则表达式

正则表达式是用于在文本中搜索的文本模式。可以通过将正则表达式与文本"匹配"来做到这一点。将正则表达式与文本相匹配的结果是:

- `true` /`false`指定正则表达式是否匹配文本。
- 一组匹配项——在文本中找到的正则表达式的每个匹配项。

例如，可以使用正则表达式在中搜索电子邮件地址、url、电话号码、日期等。这可以通过对字符串匹配不同的正则表达式来实现。每个正则表达式有一组匹配(每个正则表达式可能匹配不止一次)。

## Java正则表达式核心类

Pattern类用于创建模式（正则表达式）。模式是以对象形式（作为“pattern”实例）预编译的正则表达式，能够将自身与目标文本匹配。

Matcher用于在文本中查找正则表达式的匹配结果。Matcher将告诉我们在文本中找到匹配项的位置。可以从“Pattern”实例获取“Matcher”实例。

## Java正则表达式示例
Java regex API可以告诉您正则表达式是否匹配某个字符串，或者返回该字符串中该正则表达式的所有匹配项。

### Pattern的示例

下面是一个简单的java正则表达式示例，它使用正则表达式检查文本是否包含子字符串`http://`:

```
String text    =
        "This is the text to be searched " +
        "for occurrences of the http:// pattern.";

String regex = ".*http://.*";

boolean matches = Pattern.matches(regex, text);

System.out.println("matches = " + matches);
```

变量`text`表示需要被检查的文本。

`pattern`变量以"字符串"的形式接收正则表达式。正则表达式匹配包含一个或多个字符(`.*`)，然后紧接着文本`http://`，之后再紧接着一个或多个字符(`.*`)的所有文本。

第三行使用`pattern .matches()`静态方法检查正则表达式(pattern)是否与文本匹配。如果正则表达式与文本匹配，则`Pattern.matches()`返回true。如果正则表达式与文本不匹配，`Pattern.matches()`返回false。

正则表达式目的是为了检查字符串`http://`是否出现。

### Matcher Example

下面是另一个Java正则表达式的例子，使用`Matcher`类来定位文本中多次出现的子字符串"is":

```
String text    =
        "This is the text which is to be searched " +
        "for occurrences of the word`is'.";

String regex = "is";

Pattern pattern = Pattern.compile(regex);
Matcher matcher = pattern.matcher(text);

int count = 0;
while(matcher.find()) {
    count++;
    System.out.println("found: " + count + " : "
            + matcher.start() + " - " + matcher.end());
}
```

从“Pattern”实例中获得一个“Matcher”实例。通过这个“Matcher”实例，以上示例将查找文本中所有匹配到正则表达式的片段。

## Java正则表达式语法

正则表达式的一个关键方面是正则表达式的语法。Java不是唯一支持正则表达式的编程语言。大多数现代编程语言都支持正则表达式。但是，每种语言中定义正则表达式所用的语法并不完全相同。因此，需要学习Java中使用的语法。

## 匹配字符

首先要看的是如何编写正则表达式来匹配给定文本中的字符。例如，这里定义的正则表达式:

```
String regex = "http://";
```

将匹配与正则表达式完全相同的所有字符串。`http://`之前或之后不能有字符—否则正则表达式将与文本不匹配。例如，上面的正则表达式将匹配此文本:

```
String text1 = "http://";
```

但不能匹配这段文字:

```
String text2 = "The URL is: http://mydomain.com";
```

第二个字符串在`http://`之前和之后包含了字符。

## 转义字符

如上所述，Java正则表达式中的元字符有特殊的含义。如果想匹配这些字符则必须“转义”想要匹配的元字符。要转义元字符，可以使用Java正则表达式转义字符—`\`。转义字符意味着在它之前使用反斜杠字符。例如，像这样:

```
\.
```

在这个例子中是`.`字符前加上`\`字符(转义)。转义后，`.`不再与输入文本中的任意字符进行匹配，而是仅和文本中的`.`匹配。

Java正则表达式语法使用反斜杠字符作为转义字符，就像Java字符串一样。这给在Java字符串中编写正则表达式带来了一点挑战。看看这个正则表达式的例子:

```
String regex = "\\.";
```

请注意，正则表达式字符串分别包含两个反斜杠和一个`.`。原因是，首先Java编译器将两个`\\`字符解释为转义的Java字符串字符。完成Java编译之后，只剩下一个`\`，因此在java的正则字符串中`\\`表示字符`\`。字符串看起来是这样的:

```
\.
```

现在Java正则表达式解释器开始工作，并将其余的反斜杠解释为转义字符。以下字符'。'现在被解释为表示实际的句号，而不是具有特殊的正则表达式。因此，其余的正则表达式只匹配句号字符。

Several characters have a special meaning in the Java regular expression syntax. If you want to match for that explicit character and not use it with its special meaning, you need to escape it with the backslash character first. For instance, to match for the full stop character, you need to write:

```
String regex = "\\.";
```

To match for the backslash character itself, you need to write:

```
String regex = "\\\\";
```

Getting the escaping of characters right in regular expressions can be tricky. For advanced regular expressions you might have to play around with it a while before you get it right.



## Matching Any Character

So far we have only seen how to match specific characters like "h", "t", "p" etc. However, you can also just match any character without regard to what character it is. The Java regular expression syntax lets you do that using the `.` character (period / full stop). Here is an example regular expression that matches any character:

```
String regex = ".";
```

This regular expression matches a single character, no matter what character it is.

The `.` character can be combined with other characters to create more advanced regular expressions. Here is an example:

```
String regex = "H.llo";
```

This regular expression will match any Java string that contains the characters "H" followed by any character, followed by the characters "llo". Thus, this regular expression will match all of the strings "Hello", "Hallo", "Hullo", "Hxllo" etc.



## Matching Any of a Set of Characters

Java regular expressions support matching any of a specified set of characters using what is referred to as character classes. Here is a character class example:

```
String regex = "H[ae]llo";
```

The character class (set of characters to match) is enclosed in the square brackets - the `[ae]` part of the regular expression, in other words. The square brackets are not matched - only the characters inside them.

The character class will match one of the enclosed characters regardless of which, but no mor than one. Thus, the regular expression above will match any of the two strings "Hallo" or "Hello", but no other strings. Only an "a" or an "e" is allowed between the "H" and the "llo".

You can match a range of characters by specifying the first and the last character in the range with a dash in between. For instance, the character class `[a-z]` will match all characters between a lowercase `a` and a lowercase `z`, both `a` and `z` included.

You can have more than one character range within a character class. For instance, the character class `[a-zA-Z]` will match all letters between `a` and `z` or between `A` and `Z` .

You can also use ranges for digits. For instance, the character class `[0-9]` will match the characters between 0 and 9, both included.

If you want to actually match one of the square brackets in a text, you will need to escape them. Here is how escaping the square brackets look:

```
String regex = "H\\[llo";
```

The `\\[` is the escaped square left bracket. This regular expression will match the string "H[llo".

If you want to match the square brackets inside a character class, here is how that looks:

```
String regex = "H[\\[\\]]llo";
```

The character class is this part: `[\\[\\]]`. The character class contains the two square brackets escaped (`\\[` and `\\]`).

This regular expression will match the strings "H[llo" and "H]llo".



## Matching a Range of Characters

The Java regex API allows you to specify a range of characters to match. Specifying a range of characters is easier than explicitly specifying each character to match. For instance, you can match the characters a to z like this:

```
String regex = "[a-z]";
```

This regular expression will match any single character from a to z in the alphabet.

The character classes are case sensitive. To match all characters from a to z regardless of case, you must include both uppercase and lowercase character ranges. Here is how that looks:

```
String regex = "[a-zA-Z]";
```



## Matching Digits

You can match digits of a number with the predefined character class with the code `\d`. The digit character class corresponds to the character class `[0-9]`.

Since the `\` character is also an escape character in Java, you need two backslashes in the Java string to get a `\d` in the regular expression. Here is how such a regular expression string looks:

```
String regex = "Hi\\d";
```

This regular expression will match strings starting with "Hi" followed by a digit (`0` to `9`). Thus, it will match the string "Hi5" but not the string "Hip".



## Matching Non-digits

Matching non-digits can be done with the predefined character class `[\D]` (uppercase D). Here is an regular expression containing the non-digit character class:

```
String regex = "Hi\\D";
```

This regular expression will match any string which starts with "Hi" followed by one character which is not a digit.



## Matching Word Characters

You can match word characters with the predefined character class with the code `\w` . The word character class corresponds to the character class `[a-zA-Z_0-9]`.

```
String regex = "Hi\\w";
```

This regular expression will match any string that starts with "Hi" followed by a single word character.



## Matching Non-word Characters

You can match non-word characters with the predefined character class `[\W]` (uppercase W). Since the `\` character is also an escape character in Java, you need two backslashes in the Java string to get a `\w` in the regular expression. Here is how such a regular expression string looks:

Here is a regular expression example using the non-word character class:

```
String regex = "Hi\\W";
```



## 边界匹配

### Beginning of Line (or String)

The `^` boundary matcher matches the beginning of a line according to the Java API specification. However, in practice it seems to only be matching the beginning of a String. For instance, the following example only gets a single match at index 0:

```
String text = "Line 1\nLine2\nLine3";

Pattern pattern = Pattern.compile("^");
Matcher matcher = pattern.matcher(text);

while(matcher.find()){
    System.out.println("Found match at: "  + matcher.start() + " to " + matcher.end());
}
```

Even if the input string contains several line breaks, the `^` character only matches the beginning of the input string, not the beginning of each line (after each line break).

The beginning of line / string matcher is often used in combination with other characters, to check if a string begins with a certain substring. For instance, this example checks if the input string starts with the substring `http://` :

```
String text = "http://jenkov.com";

Pattern pattern = Pattern.compile("^http://");
Matcher matcher = pattern.matcher(text);

while(matcher.find()){
    System.out.println("Found match at: "  + matcher.start() + " to " + matcher.end());
}
```

This example finds a single match of the substring `http://` from index 0 to index 7 in the input stream. Even if the input string had contained more instances of the substring `http://` they would not have been matched by this regular expression, since the regular expression started with the `^` character.



### End of Line (or String)

The `$` boundary matcher matches the end of the line according to the Java specification. In practice, however, it looks like it only matches the end of the input string.

The beginning of line (or string) matcher is often used in combination with other characters, most commonly to check if a string ends with a certain substring. Here is an example of the end of line / string matcher:

```
String text = "http://jenkov.com";

Pattern pattern = Pattern.compile(".com$");
Matcher matcher = pattern.matcher(text);

while(matcher.find()){
    System.out.println("Found match at: "  + matcher.start() + " to " + matcher.end());
}
```

This example will find a single match at the end of the input string.



### Word Boundaries

The `\b` boundary matcher matches a word boundary, meaning a location in an input string where a word either starts or ends.

Here is a Java regex word boundary example:

```
String text = "Mary had a little lamb";

Pattern pattern = Pattern.compile("\\b");
Matcher matcher = pattern.matcher(text);

while(matcher.find()){
    System.out.println("Found match at: "  + matcher.start() + " to " + matcher.end());
}
```

This example matches all word boundaries found in the input string. Notice how the word boundary matcher is written as `\\b` - with two `\\` (backslash) characters. The reason for this is explained in the section about [escaping characters](http://tutorials.jenkov.com/java-regex/index.html#escaping-characters). The Java compiler uses `\` as an escape character, and thus requires two backslash characters after each other in order to insert a single backslash character into the string.

The output of running this example would be:

```
Found match at: 0 to 0
Found match at: 4 to 4
Found match at: 5 to 5
Found match at: 8 to 8
Found match at: 9 to 9
Found match at: 10 to 10
Found match at: 11 to 11
Found match at: 17 to 17
Found match at: 18 to 18
Found match at: 22 to 22
```

The output lists all the locations where a word either starts or ends in the input string. As you can see, the indices of word beginnings point to the first character of the word, whereas endings of a word points to the first character after the word.

You can combine the word boundary matcher with other characters to search for words beginning with specific characters. Here is an example:

```
String text = "Mary had a little lamb";

Pattern pattern = Pattern.compile("\\bl");
Matcher matcher = pattern.matcher(text);

while(matcher.find()){
    System.out.println("Found match at: "  + matcher.start() + " to " + matcher.end());
}
```

This example will find all the locations where a word starts with the letter `l` (lowercase). In fact it will also find the ends of these matches, meaning the last character of the pattern, which is the lowercase `l` letter.

### Non-word Boundaries

The `\B` boundary matcher matches non-word boundaries. A non-word boundary is a boundary between two characters which are both part of the same word. In other words, the character combination is not word-to-non-word character sequence (which is a word boundary). Here is a simple Java regex non-word boundary matcher example:

```
String text = "Mary had a little lamb";

Pattern pattern = Pattern.compile("\\B");
Matcher matcher = pattern.matcher(text);

while(matcher.find()){
    System.out.println("Found match at: "  + matcher.start() + " to " + matcher.end());
}
```

This example will give the following output:

```
Found match at: 1 to 1
Found match at: 2 to 2
Found match at: 3 to 3
Found match at: 6 to 6
Found match at: 7 to 7
Found match at: 12 to 12
Found match at: 13 to 13
Found match at: 14 to 14
Found match at: 15 to 15
Found match at: 16 to 16
Found match at: 19 to 19
Found match at: 20 to 20
Found match at: 21 to 21
```

Notice how these match indexes corresponds to boundaries between characters within the same word.

## Quantifiers

Quantifiers can be used to match characters more than once. There are several types of quantifiers which are listed in the [Java Regex Syntax](http://tutorials.jenkov.com/java-regex-/syntax.html). I will introduce some of the most commonly used quantifiers here.

The first two quantifiers are the `*` and `+` characters. You put one of these characters after the character you want to match multiple times. Here is a regular expression with a quantifier:

```
String regex = "Hello*";
```

This regular expression matches strings with the text "Hell" followed by zero or more `o` characters. Thus, the regular expression will match "Hell", "Hello", "Helloo" etc.

If the quantifier had been the `+` character instead of the `*` character, the string would have had to end with 1 or more `o` characters.

If you want to match any of the two quantifier characters you will need to escape them. Here is an example of escaping the `+` quantifier:

```
String regex = "Hell\\+";
```

This regular expression will match the string "Hell+";

You can also match an exact number of a specific character using the `{n}` quantifier, where `n` is the number of characters you want to match. Here is an example:

```
String regex = "Hello{2}";
```

This regular expression will match the string "Helloo" (with two `o` characters in the end).

You can set an upper and a lower bound on the number of characters you want to match, like this:

```
String regex = "Hello{2,4}";
```

This regular expression will match the strings "Helloo", "Hellooo" and "Helloooo". In other words, the string "Hell" with 2, 3 or 4 `o` characters in the end.

## Logical Operators

The Java Regex API supports a set of logical operators which can be used to combine multiple subpatterns within a single regular expression. The Java Regex API supports two logical operators: The *and* operator and the *or* operator.

The *and* operator is implicit. If two characters (or other subpatterns) follow each other in a regular expression, that means that both the first *and* the second subpattern much match the target string. Here is an example of a regular expression that uses an implicit *and* operator:

```
String text = "Cindarella and Sleeping Beauty sat in a tree";

Pattern pattern = Pattern.compile("[Cc][Ii].*");
Matcher matcher = pattern.matcher(text);

System.out.println("matcher.matches() = " + matcher.matches());
```

Notice the 3 subpatterns `[Cc]`, `[Ii]` and `.*`

Since there are no characters between these subpatterns in the regular expression, there is implicitly an *and* operator in between them. This means, that the target string must match all 3 subpatterns in the given order to match the regular expression as a whole. As you can see from the string, the expression matches the string. The string should start with either an uppercase or lowercase `C`, followed by an uppercase or lowercase `I` and then zero or more characters. The string meets these criteria.

The *or* operator is explicit and is represented by the pipe character `|`. Here is an example of a regular expression that contains two subexpression with the logical *or* operator in between:

```
String text = "Cindarella and Sleeping Beauty sat in a tree";

Pattern pattern = Pattern.compile(".*Ariel.*|.*Sleeping Beauty.*");
Matcher matcher = pattern.matcher(text);

System.out.println("matcher.matches() = " + matcher.matches());
```

As you can see, the pattern will match either the subpattern `Ariel` or the subpattern `Sleeping Beauty` somewhere in the target string. Since the target string contains the text `Sleeping Beauty`, the regular expression matches the target string.



## Java String Regex Methods

The Java String class has a few regular expression methods too. I will cover some of those here:



### matches()

The Java String `matches()` method takes a regular expression as parameter, and returns `true` if the regular expression matches the string, and `false` if not.

Here is a `matches()` example:

```
String text = "one two three two one";

boolean matches = text.matches(".*two.*");
```



### split()

The Java String `split()` method splits the string into N substrings and returns a String array with these substrings. The `split()` method takes a regular expression as parameter and splits the string at all positions in the string where the regular expression matches a part of the string. The regular expression is not returned as part of the returned substrings.

Here is a `split()` example:

```
String text = "one two three two one";

String[] twos = text.split("two");
```

This example will return the three strings "one", " three" and " one".



### replaceFirst()

The Java String `replaceFirst()` method returns a new String with the first match of the regular expression passed as first parameter with the string value of the second parameter.

Here is a `replaceFirst()` example:

```
String text = "one two three two one";

String s = text.replaceFirst("two", "five");
```

This example will return the string "one five three two one".



### replaceAll()

The Java String `replaceAll()` method returns a new String with all matches of the regular expression passed as first parameter with the string value of the second parameter.

Here is a `replaceAll()` example:

```
String text = "one two three two one";

String t = text.replaceAll("two", "five");
```

This example will return the string "one five three five one".

