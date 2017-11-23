---
layout: post
title: Markdown syntax
date: 2017-11-23 12:30:00
categories: study markdown
---

## Table of Contents

1. [Headers](#headers)
2. [Block Quote](#block-quote)
3. [List](#list)
  + [Ordered List](#ordered-list)
  + [Unordered List](#unordered-list)
4. [Code](#code)
5. [수평선](#hr)
6. [Links](#links)
7. [강조](#emphasis)
8. [Images](#images)

# 1. Headers <a name="headers" />

```
# This is a H1
## This is a H2
### This is a H3
#### This is a H4
##### This is a H5
###### This is a H6
```

# This is a H1
## This is a H2
### This is a H3
#### This is a H4
##### This is a H5
###### This is a H6

# 2. Block Quote <a name="block-quote" />

```
> This is a blockquote.
>> This is a blockquote.
>>> This is a blockquote.
```

> This is a blockquote.
>> This is a blockquote.
>>> This is a blockquote.

# 3. List <a name="list" />

## 3-1 Ordered list <a name="ordered-list" />

```
1. 첫번째
2. 두번째
3. 세번째
```

1. 첫번째
2. 두번째
3. 세번째

## 3-2 Unordered list <a name="unordered-list" />

```
* 빨강
  * 녹색
    * 파랑

+ 빨강
  + 녹색
    + 파랑

- 빨강
- 녹색
- 파랑
```

* 빨강
  * 녹색
    * 파랑

+ 빨강
  + 녹색
    + 파랑

- 빨강
- 녹색
- 파랑

# 4. code <a name="code" />

\` 3개 연속으로 입력 후 코드 입력하면 됨. (코드 끝나면 ` 3개)

\`\`\`  
print('hello world')  
\`\`\`

```
print('hello world')
```

# 5. 수평선 <a name="hr" />

아래 줄은 모두 수평선을 만든다. 마크다운 문서를 미리보기로 출력할 때 페이지 나누기 용도로 많이 사용한다.

```
* * *
또는,
***
또는,
*****
```

***

# 6. Links <a name="links" />

```
[link keyword][id]
[id]: URL
```
[url][test-url]

[test-url]: http://m.naver.com

새 탭에서 여는 방법:

```
[link keyword][id]{:target='_blank'}
[id]: URL
```
[new tab][test-url]{:target='_blank'}  

```
인라인 링크 : [title](link url)
```

[inline link](http://www.naver.com)

# 7. 강조 <a name="emphasis" />

```
*single asterisks*
_single underscores_
**double asterisks**
__double underscores__
++underline++
~~cancelline~~
```

*single asterisks*  
_single underscores_  
**double asterisks**  
__double underscores__  
++underline++  
~~cancelline~~

# 8. Images <a name="images" />

```
![Alt text](/public/img/machine.jpg)
![Alt text](/public/img/machine.jpg "Optional title")
```

!["maschine"](/public/img/maschine.jpg "maschine")
