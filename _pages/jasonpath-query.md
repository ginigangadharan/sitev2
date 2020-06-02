---
layout: post
title: JSON PATH Query
#author: gini
categories: [ json, yaml ]
#image: "assets/images/gini-redhat-cloudevent-2019-2.jpg"
tags: []
show-avatar: false
permalink: jasonpath-query
featured: false
hidden: false
---

Ref: https://github.com/json-path/JsonPath

## Basic Dictionary

```
$.car.wheels[2]                   # 2rd item
$.[0,3]                           # 1st and 4th elements from array
$.[0:4]                           # 1st to 4th elements from array
                                    excluding 4th element
$.[0:6:2]                         # 1st to 4th  elements with steps 2 (skip one element each time)                        
$.[-1:0]                          # last item
$.[-8:-2]                         # from bottom
$.[0:5].phone                     # first five   
$.[-5:].age                       # last 5 entries    
```

## Wild Card

```
$.[*]                             
$.[*].color                       # color property of any parent items
$.car.wheels[*].model
$.[*].wheels[*].model
$.employee.payslips[*].amount
$.prizes[*].laureates[*].firstname
$.prizes[?(@.year == 2014)].laureates[*].firstname 
                                  # all firstname but for year 2014
                          
```

## JSON PATH in kubernetes
