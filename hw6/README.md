# Homework 6: Damped Oscillator and Coupled Oscillator [empty class, list representation]

因為有同學大概是直接把 Mac 的 .pages 檔案直接改檔名成 .txt 了，提醒你們一下就算改了副檔名，內在的檔案格式還是可能不一樣的喔。不過這個格式剛好不會造成我太大困擾就是了，所以就提醒一下。 

## 影片以及官方pdf

+ [影片](https://goo.gl/xcZafB)  
+ [官方pdf](VP6.pdf)  

## 作業繳交格式

請上傳一個zip檔（壓縮檔，請注意副檔名要是zip）到CEIBA，zip檔內需要包含一個**名稱是自己學號的資料夾**，裡面包含一個或兩個py檔。請將必作部份取名為`must.py`，選作部份取名為`optional.py`。如果這次作業有拍攝影片，**請將影片連結寫在video.txt裡面，並一併放入學號資料夾中**。

範例：
```
the_zip_file.zip
└── r07222060
    ├── must.py
    ├── optional.py
    └── video.txt
```

## 繳交期限

`2018/11/29 THU 21:00`

## Contents

+ [List Representation](#i-list-representation)
+ [Practice](#ii-practice)
+ [Homework](#iii-homework)

## I. List Representation

1. range(). Notice the number in range should be integer (int)

   ```python
   L = range(5)        # list(L) = [0, 1, 2, 3, 4]
   L = range(4, 9)     # list(L) = [4, 5, 6, 7, 8]
   L = range(1, 6, 2)  # list(L) = [1, 3, 5] 1 to 6 every other 2 numbers
   ```

   > ##### 助教註:
   > Notice that the `range` object in Python is **not a list**. Actually it's something called generator, so Professor said `list(L) = [...]` here instead of `L = [...]`. Although you can't directly use it as a list, you can turn it into a list like what Professor did here, e.g. `list(range(5))`.

2. list representation. Sometimes we want to generate a list with some conditions, e.g.

   ```python
   L = [i**2 for i in range(5)]            # = [0, 1, 4, 9, 16]
   L = [0.1*i*pi for i in range(-3, 3)]    # = [-0.3*pi, -0.2*pi, -0.1*pi, 0, 0.1*pi, 0.2*pi]
   L = [i**2 for i in range (5) if i != 3] # = [0, 1, 4, 16]
   ```

3. List representation can be used in a nested structure, or for dictionary or tuple. e.g.
   ```python
   L = [i*10 + j for i in range(3) for j in range(5)] # = [0, 1, 2, 3, 4, 10, 11, 12, 13, 14, 20, 21, 22, 23, 24]
   D = {i:i**2 for i in [0, 1, 2]}                     # = {0:0, 1:1, 2:4}
   ```

> ##### 助教註:
> Both 2. and 3. are just about a simpler way to construct a list, a dictionary, or a tuple. For example, I wrote some more "traditional" ways equivalent to the above here:
> ```python
> # The following is equivalent to L = [i**2 for i in range (5) if i != 3]
> L = []
> for i in range(5):
>     if i != 3:
>         L.append(i**2)
>         
> # The following is equivalent to L = [i*10 + j for i in range(3) for j in range(5)]
> L = []
> for i in range(3):
>     for j in range(5):
>         L.append(i*10 + j)
>         
> # The following is equivalent to D = {i:i**2 for i in [0, 1, 2]}
> D = {}
> for i in [0, 1, 2]:
>     D[i] = i**2
> ```

## II. Practice

```python
from vpython import *
size, m = 0.02, 0.2  # ball size = 0.02 m, ball mass = 0.2kg
L, k = 0.2, 20       # spring original length = 0.2m, force constant = 20 N/m
amplitude = 0.03
b = 0.05 * m * sqrt(k/m)

# =========== delete below in practice 3 ===========
scene = canvas(width=600, height=400, fov=0.03, align='left', center=vec(0.3, 0, 0), background=vec(0.5, 0.5, 0))
wall_left = box(length=0.005, height=0.3, width=0.3, color=color.blue) # left wall
ball = sphere(radius=size, color=color.red)                            # ball
spring = helix(radius=0.015, thickness=0.01)
oscillation = graph(width=400, align='left', xtitle='t', ytitle='x', background=vec(0.5, 0.5, 0))
x = gcurve(color=color.red, graph=oscillation)
# =========== delete above in practice 3 ===========

ball.pos = vector(L+amplitude, 0 , 0)  # ball initial position
ball.v = vector(0, 0, 0)               # ball initial velocity
ball.m = m
spring.pos = vector(0, 0, 0)
t, dt = 0, 0.001

while True:
    # =========== delete below in practice 3 ===========
    rate(1000)
    # =========== delete above in practice 3 ===========
    
    spring.axis = ball.pos - spring.pos # spring extended from spring endpoint A to ball
    spring_force = -k * (mag(spring.axis) - L) * norm(spring.axis) # spring force vector
    ball.a = spring_force / ball.m     # ball acceleration = spring force /m - damping
    ball.v += ball.a*dt
    ball.pos += ball.v*dt
    t += dt
    
    # =========== delete below in practice 3 ===========
    x.plot(pos=(t, ball.pos.x - L))
    # =========== delete above in practice 3 ===========
```

Modified from hw3, the above code simulates the horizontal oscillation of a given **amplitude**.

<img src="pic/practice_example_merged.png" height=300/>

1. Now, in addition to the restoring force from the spring, add the air resistance force <img src="https://latex.codecogs.com/gif.latex?\dpi{150}&space;\vec{f}&space;=&space;-b\vec{v}" title="\vec{f} = -b\vec{v}" height=24/> to the ball with **damping factor** <img src="https://latex.codecogs.com/gif.latex?\dpi{150}&space;b&space;=&space;0.05&space;m&space;\sqrt{k/m}" title="b = 0.05 m \sqrt{k/m}" height=24/>.

2. Follow Practice 1, but instead of letting the ball to oscillate from the initial **amplitude**, i.e. `ball.pos = vector(L+amplitude, 0, 0)`, now let the ball to be initially at rest at <img src="https://latex.codecogs.com/gif.latex?\dpi{150}&space;x&space;=&space;L" title="x = L" height=13/> , i.e. `ball.pos = vector(L, 0, 0)`, and allow a sinusoidal force <img src="https://latex.codecogs.com/gif.latex?\dpi{150}&space;\vec{F}&space;=&space;f_a\sin(\omega_dt)\hat{x}" title="\vec{F} = f_a\sin(\omega_dt)\hat{x}" height=20/> ( <img src="https://latex.codecogs.com/gif.latex?\dpi{150}&space;\hat{x}" title="\hat{x}" height=12/> is the unit vector in x-axis) applied on the ball, with <img src="https://latex.codecogs.com/gif.latex?\dpi{150}&space;f_a&space;=&space;0.1" title="f_a = 0.1" height=16/> and <img src="https://latex.codecogs.com/gif.latex?\dpi{150}&space;\omega_d&space;=&space;\sqrt{k/m}" title="\omega_d = \sqrt{k/m}" height=20/>. We know when a force <img src="https://latex.codecogs.com/gif.latex?\dpi{150}&space;\vec{F}" title="\vec{F}" height=18/> exerts on an object of velocity <img src="https://latex.codecogs.com/gif.latex?\dpi{150}&space;\vec{v}" title="\vec{v}" height=14/>, the power on the object by the force is <img src="https://latex.codecogs.com/gif.latex?\dpi{150}&space;P&space;=&space;\vec{F}&space;\cdot&space;\vec{v}" title="P = \vec{F} \cdot \vec{v}" height=18/> . 
   
   Find by your simulation <img src="https://latex.codecogs.com/gif.latex?\dpi{150}&space;P_T" title="P_T" height=16/>, the power averaged over a period <img src="https://latex.codecogs.com/gif.latex?\dpi{150}&space;T&space;=&space;2\pi&space;/&space;\omega_d" title="T = 2\pi / \omega_d" height=18/> at the end of each period. In additional to the curve graph for ball’s position versus time, plot a dot graph for <img src="https://latex.codecogs.com/gif.latex?\dpi{150}&space;P_T" title="P_T" height=16/> versus time. Observe the results with different settings, such as <img src="https://latex.codecogs.com/gif.latex?\dpi{150}&space;\omega_d&space;=&space;0.8&space;\sqrt{k/m},&space;0.9&space;\sqrt{k/m},&space;1.1&space;\sqrt{k/m},&space;1.2&space;\sqrt{k/m}" title="\omega_d = 0.8 \sqrt{k/m}, 0.9 \sqrt{k/m}, 1.1 \sqrt{k/m}, 1.2 \sqrt{k/m}" height=20/> and/or with different `b` values. Think about the results. Before proceeding to Practice 3, change <img src="https://latex.codecogs.com/gif.latex?\dpi{150}&space;\omega_d" title="\omega_d" height=12/> and `b` back to the original values.

   ![practice_2](pic/practice_2_merged.png)

3. Often, we do not want an animated simulation, which is slow due to the animation, but only the calculation results. We can modify the above computer codes easily for such purpose. We can just delete the code that creates the canvas, the plot, and the graphs (marked in the codes), delete `rate(1000)`, and replace codes that generate visual objects by the following codes that generate objects from an empty class. 

   ```python
   class obj:
       pass
       
   wall_left, ball, spring = obj(), obj(), obj()
   ```

   With these, we still do the same simulation but do not animate them. This will speed up the simulation. Instead of plotting <img src="https://latex.codecogs.com/gif.latex?\dpi{150}&space;P_T" title="P_T" height=16/> , now only print <img src="https://latex.codecogs.com/gif.latex?\dpi{150}&space;P_T" title="P_T" height=16/> every period. You can see after certain number of periods, the system reaches a **steady state** and the <img src="https://latex.codecogs.com/gif.latex?\dpi{150}&space;P_T" title="P_T" height=16/> is almost a constant. Also notice that how much faster the simulation can run without the animation. (NOTICE: for this technique to work, you need to have all the proper parameters set for every object after they have been created by the empty class `obj`.)

## III. Homework

### MUST

Let `omega = [0.1*i + 0.7*sqrt(k/m) for i in range(1, int(0.5*sqrt(k/m)/0.1))]` and by using `for omega_d in omega:`, perform the calculation for steady-state <img src="https://latex.codecogs.com/gif.latex?\dpi{150}&space;P_T" title="P_T" height=16/> like practice 3 for different <img src="https://latex.codecogs.com/gif.latex?\dpi{150}&space;\omega_d" title="\omega_d" height=12/> . Do not print the result. Instead, for each <img src="https://latex.codecogs.com/gif.latex?\dpi{150}&space;\omega_d" title="\omega_d" height=12/> , when the system reaches steady state, add the latest result of the steady-state <img src="https://latex.codecogs.com/gif.latex?\dpi{150}&space;P_T" title="P_T" height=16/> to the plot of the “steady-state <img src="https://latex.codecogs.com/gif.latex?\dpi{150}&space;P_T" title="P_T" height=16/> versus `omega_d`” and then calculate the steady-state <img src="https://latex.codecogs.com/gif.latex?\dpi{150}&space;P_T" title="P_T" height=16/> for the next <img src="https://latex.codecogs.com/gif.latex?\dpi{150}&space;\omega_d" title="\omega_d" height=12/> . You will get something similar to the figure shown here, which shows clearly the system’s response to different driving frequency <img src="https://latex.codecogs.com/gif.latex?\dpi{150}&space;\omega_d" title="\omega_d" height=12/> . In additional to plotting steady-state <img src="https://latex.codecogs.com/gif.latex?\dpi{150}&space;P_T" title="P_T" height=16/> versus <img src="https://latex.codecogs.com/gif.latex?\dpi{150}&space;\omega_d" title="\omega_d" height=12/> , also print the optimal <img src="https://latex.codecogs.com/gif.latex?\dpi{150}&space;\omega_d" title="\omega_d" height=12/> such that steady-state <img src="https://latex.codecogs.com/gif.latex?\dpi{150}&space;P_T" title="P_T" height=16/> has the highest value.

<img src="pic/must.png" height=300/>

### OPTIONAL

Based on your complete code for practice 2, add an addition ball, two more springs, and a right wall.

<img src="pic/optional.png" height=300/>

All the parameters are the same as in the original program except for the following: 

1. the middle spring is of constant <img src="https://latex.codecogs.com/gif.latex?\dpi{150}&space;K&space;=&space;5.0" title="K = 5.0" height=15/>.
2. The damping factor for ball 1 (the left one) is <img src="https://latex.codecogs.com/gif.latex?\dpi{150}&space;b_1&space;=&space;0.05m\sqrt{k/m}" title="b_1 = 0.05m\sqrt{k/m}" height=23/>.
3. The damping factor for ball 2 (the right one) is <img src="https://latex.codecogs.com/gif.latex?\dpi{150}&space;b_2&space;=&space;0.0025m\sqrt{k/m}" title="b_2 = 0.0025m\sqrt{k/m}" height=23/>.
4. The external force <img src="https://latex.codecogs.com/gif.latex?\dpi{150}&space;\vec{F}&space;=&space;f_a\sin(\omega_dt)\hat{x}" title="\vec{F} = f_a\sin(\omega_dt)\hat{x}" height=22/> is exerted on ball 1 with <img src="https://latex.codecogs.com/gif.latex?\dpi{150}&space;f_a&space;=&space;0.1" title="f_a = 0.1" height=17/> and <img src="https://latex.codecogs.com/gif.latex?\dpi{150}&space;\omega_d&space;=&space;\sqrt{(k&plus;K)/m}" title="\omega_d = \sqrt{(k+K)/m}" height=21/>. 

This is similar to Practice 2 except that now the driving frequency is corresponding to the resonance frequency of the combining effect due to the left and the middle springs. As in Practice 2, simulate the system and calculate by your simulation <img src="https://latex.codecogs.com/gif.latex?\dpi{150}&space;P_T" title="P_T" height=16/> the power averaged over a period <img src="https://latex.codecogs.com/gif.latex?\dpi{150}&space;T&space;=&space;2\pi/\omega_d" title="T = 2\pi/\omega_d" height=20/> at the end of each period. In this optional homework, you need to show:

1. the simulation animation
2. the curve graph for ball’s position versus time
3. and a dot graph for period-averaged power <img src="https://latex.codecogs.com/gif.latex?\dpi{150}&space;P_T" title="P_T" height=16/> versus time.

In the simulation, you will see a very interesting phenomena, that the periodic force <img src="https://latex.codecogs.com/gif.latex?\dpi{150}&space;\vec{F}&space;=&space;f_a&space;\sin(\omega_dt)\hat{x}" title="\vec{F} = f_a \sin(\omega_dt)\hat{x}" height=22/> is exerted on ball 1, but in the end, ball 1 barely oscillates, think about what is the physics behind this.

Working out the optional part, we will observe the principle behind "EIT" (Electromagnetically Induced Transparency), a very interesting and advanced research topic in optics that won many awards. You may read [the wiki](https://en.wikipedia.org/wiki/Electromagnetically_induced_transparency) for the mechanism of EIT and see the similarity between the EIT and our simulation here. 

topic in optics that won many awards. You may read [the wiki](https://en.wikipedia.org/wiki/Electromagnetically_induced_transparency) for the mechanism of EIT and see the similarity between the EIT and our simulation here. 

## IV. Grading Policy

### Must

* There is a peak in the plot. (20%)
* The peak is located correctly. (20%)
* The shape of the plot is correct. (40%)
* The width of the peak is correct. (20%)

### Optional

* Correctly built up the system. (20%)
* The program plots the position of the ball versus time. (20%)
* The program plots the period-averaged power versus time. (20%)
* The ball 1 does barely oscillate. (40%)
