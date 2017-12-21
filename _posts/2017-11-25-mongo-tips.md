---
layout: post
title: Tips for Mongodb
date: 2017-11-25 10:30:00
categories: mongoose study
---

# Useful queries

+ **$ne**  
desc != \'\'
```
{ desc: { $ne : '' } }
```

+ **$in**,  **$nin**  
desc is in [] (or not in [])
```
{ desc: { $nin : ['', null] } }
```

+ **$regex** : 정규 표현식  
해당 단어를 포함하는지 확인 가능
```
{ 'title' : { $regex : 'shaver' } }
```

# Data export / import

+ export
```
mongoexport -h 127.0.0.1:27000 -d dbName -c collectionName -o filename.json
```

+ import
```
mongoimport -h 127.0.0.1:27017 -d dbName -c collectionName --file filename.json
```
