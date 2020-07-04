## netlogo 建模



    setxy random-xcor random-ycor
    set color green
    set shape "circle"
    set size 0.75
处在同一瓦片

### SEIR

```netlogo
extensions [ gogo sound gis]
turtles-own
[
  decubated-period ;康复期
  incubated-period ;潜伏期
]

to setup
  ca
  crt pop
  [
    setxy random-xcor random-ycor
    set color green
    set shape "circle"
    set size 0.75
  ]

  ask one-of turtles
  [
    set color red
    set decubated-period time
  ]
  reset-ticks
end

to go
  ask turtles
  [
    lt 40 - random 80
    fd 1
  ]
  infecting
  fallill
  recover
  tick
end

to infecting
  ask turtles with [color = red]
  [
    ask turtles-here with [color = green]
    [
      set color yellow
      set incubated-period time2
    ]
  ]
end

to fallill
  ask turtles with [color = yellow]
  [
    ifelse incubated-period <= 0
    [
      set color red
      set decubated-period time
    ]
    [set incubated-period incubated-period - 1]
  ]
end

to recover
  ask turtles with [color = red]
  [
    ifelse decubated-period <= 0
    [set color blue]
    [set decubated-period decubated-period - 1]
  ]

end
```





### 显示gis

* 去掉z轴
* `gis:set-world-envelope (gis:envelope-union-of (gis:envelope-of hankou-dataset))`更新范围

https://stackoverflow.com/questions/56613062/netlogo-gis-unsupported-shape-type-file



*  网络距离

```netlogo
to go
  ask turtle 1
  [ ask other turtles in-radius 3
    [ create-link-with turtle 1] ]
end 
```



* 

to go
  ask turtle 1
  [ ask other turtles in-radius 3
    [ create-link-with turtle 1] ]
end 

```netlogo
  ask turtles
  [show [who] of other turtles in-radius 3]
```

```netlogo
  ask turtles [
    ask other turtles in-radius 4 [
      create-link-with myself
    ]
  ]
```

* 度分布绘图

```netlogo
let max-degree max [count link-neighbors] of turtles
plot-pen-reset  ;; erase what we plotted before
set-plot-x-range 1 (max-degree + 1)  ;; + 1 to make room for the width of the last bar
histogram [count link-neighbors] of turtles
```

* long link 1

```netlogo
to ctLongLink
  ask turtles with [count link-neighbors < 9]
  [create-link-with [one-of both-ends] of one-of links ]
end
```

对于度小于9的节点，每个随机依附到一个链接

* long link2 

```netlogo
to ctLongLink
  set lowDTur turtles with [count link-neighbors > 15]
  foreach (list lowDTur) [node -> 
    set endsTur [who] of [one-of both-ends] of one-of links
    if  (any? node with [who != endsTur])
    [ask node [ create-link-with one-of turtles with [who = endsTur]]]
  ]
  tick
end
```



* 改变视图



## reference

* https://mp.weixin.qq.com/s/V1zo19TV4VihuPHqtncvZA?from=singlemessage&scene=1&subscene=10000&clicktime=1592802411&enterid=1592802411