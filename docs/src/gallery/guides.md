# Guides

## [`Guide.annotation`](@ref)

```@example
using Gadfly, Compose
set_default_plot_size(14cm, 8cm)
plot(sin, 0, 2pi, Guide.annotation(compose(context(),
     Shape.circle([pi/2, 3*pi/2], [1.0, -1.0], [2mm]),
     fill(nothing), stroke("orange"))))
```


## [`Guide.colorkey`](@ref)

```@example
using Gadfly, RDatasets
set_default_plot_size(14cm, 8cm)
Dsleep = dataset("ggplot2", "msleep")[:,[:Vore,:BrainWt,:BodyWt,:SleepTotal]]
DataFrames.dropmissing!(Dsleep)
Dsleep.SleepTime = Dsleep.SleepTotal .> 8
plot(Dsleep, x=:BodyWt, y=:BrainWt, Geom.point, color=:SleepTime, 
     Guide.colorkey(title="Sleep", labels=[">8","≤8"]),
     Scale.x_log10, Scale.y_log10 )
```

```@example
using Gadfly, Compose, RDatasets
set_default_plot_size(21cm, 8cm)
iris = dataset("datasets","iris")
pa = plot(iris, x=:SepalLength, y=:PetalLength, color=:Species, Geom.point,
          Theme(key_position=:inside) )
pb = plot(iris, x=:SepalLength, y=:PetalLength, color=:Species, Geom.point, 
          Guide.colorkey(title="Iris", pos=[0.05w,-0.28h]) )
hstack(pa, pb)
```


## [`Guide.manual_color_key`](@ref)

```@example
using Gadfly, DataFrames
set_default_plot_size(14cm, 8cm)
points = DataFrame(index=rand(0:10,30), val=rand(1:10,30))
line = DataFrame(val=rand(1:10,11), index = collect(0:10))
pointLayer = layer(points, x=:index, y=:val, color=[colorant"green"])
lineLayer = layer(line, x=:index, y=:val, Geom.line)
plot(pointLayer, lineLayer,
    Guide.manual_color_key("Legend", ["Points", "Line"], ["green", "deepskyblue"]))
```


## [`Guide.shapekey`](@ref)

```@example
using Compose, Gadfly, RDatasets
set_default_plot_size(16cm, 8cm)
Dsleep = dataset("ggplot2", "msleep")
Dsleep = dropmissing!(Dsleep[:,[:Vore, :Name,:BrainWt,:BodyWt, :SleepTotal]])
Dsleep.SleepTime = Dsleep.SleepTotal .> 8
plot(Dsleep, x=:BodyWt, y=:BrainWt, Geom.point, color=:Vore, shape=:SleepTime,
    Scale.x_log10, Scale.y_log10, Guide.colorkey,
    Guide.shapekey(title="Sleep (hrs)", labels=[">8","≤8"]),
    Theme(point_size=2mm, key_swatch_color="slategrey", 
            point_shapes=[Shape.utriangle, Shape.dtriangle]) )
```


## [`Guide.sizekey`](@ref)

```@example
using Compose, Gadfly, RDatasets
set_default_plot_size(14cm, 8cm)

Titanic = dataset("datasets", "Titanic")
Class =  by(Titanic, :Class, :Freq=>sum)
Titanic = join(Titanic[Titanic.Survived.=="Yes",:], Class, on=:Class)
Titanic.prcnt = 100*Titanic.Freq./Titanic.Freq_sum
sizemap = n->range(3pt, 8pt, length=n)

plot(Titanic, Scale.x_log10,  Scale.y_log10,
    x=:Freq, y=:prcnt, color=:Age, shape=:Sex,  size=:Class,
    Scale.size_discrete2(sizemap), Guide.sizekey(title="Passenger\n Class"),
    Guide.colorkey(pos=[0.1, -0.3h]), Guide.shapekey(pos=[0.5, -0.31h]),
    Guide.ylabel("% of Passenger Class"),
 Theme(discrete_highlight_color=identity, alphas=[0.1], key_swatch_color="grey",
    key_swatch_shape=Shape.circle, point_size=3pt) )
```


## [`Guide.title`](@ref)

```@example
using Gadfly, RDatasets
set_default_plot_size(14cm, 8cm)
plot(dataset("ggplot2", "diamonds"), x="Price", Geom.histogram,
     Guide.title("Diamond Price Distribution"))
```


## [`Guide.xlabel`](@ref), [`Guide.ylabel`](@ref)

```@example
using Gadfly
set_default_plot_size(21cm, 8cm)
p1 = plot(cos, 0, 2π, Guide.xlabel("Angle"));
p2 = plot(cos, 0, 2π, Guide.xlabel("Angle", orientation=:vertical));
p3 = plot(cos, 0, 2π, Guide.xlabel(nothing));
hstack(p1,p2,p3)
```


## [`Guide.xrug`](@ref), [`Guide.yrug`](@ref)

```@example
using Gadfly
set_default_plot_size(14cm, 8cm)
plot(x=rand(20), y=rand(20), Guide.xrug)
```


## [`Guide.xticks`](@ref), [`Guide.yticks`](@ref)

```@example
using Gadfly
set_default_plot_size(21cm, 8cm)
ticks = [0.1, 0.3, 0.5]
p1 = plot(x=rand(10), y=rand(10), Geom.line, Guide.xticks(ticks=ticks))
p2 = plot(x=rand(10), y=rand(10), Geom.line, Guide.xticks(ticks=ticks, label=false))
p3 = plot(x=rand(10), y=rand(10), Geom.line,
          Guide.xticks(ticks=ticks, orientation=:vertical))
hstack(p1,p2,p3)
```

```@example
using Gadfly
set_default_plot_size(14cm, 8cm)
plot(x=rand(1:10, 10), y=rand(1:10, 10), Geom.line, Guide.xticks(ticks=1:9))
```
