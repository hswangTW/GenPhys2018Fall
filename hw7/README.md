# Homework 7: Spring-Ball Wave and Dispersion of 1D phonon [numpy array]

這是作業七的說明，但是本學期是不需要交這份作業的，大家可以自由練習。（別寫錯作業了！）

## 影片以及官方PDF  

+ [影片](https://goo.gl/a8ezdM)   
+ [官方PDF](VP7.pdf)  

## 作業繳交格式
無須繳交

### 繳交期限
無須繳交  

## Contents  

- [I. Numpy Array](#i-numpy-array)  
- [II. Practice: Longitudinal Spring-Ball Wave](#ii-practice:-longitudinal-spring-ball-wave)  
- [III. Homework](#iii-homework)  

## I. Numpy Array  
Python has a very powerful module called `numpy`, in which there is a type called array. Array can help us to speed up program dramatically (30 times to 300 timesfaster). First, let’s see some operations about array.  

![nparray](pic/numpy_array.png)

Notice the index starts from 0. The following is to compare code execution speed for different methods.  

```python
from timeit import default_timer as timer
from numpy import *

start = timer()
for x in range(100):
    j = []
    for i in range(10000): j.append(i**2)
end = timer()
print(end-start)

start = timer()
for x in range(100):
    j=[i**2 for j in range(10000)]
end = timer()
print(end-start)

start = timer()
for x in range(100):
    j=arange(10000)**2
end = timer()
print(end-start)
```

> #### 助教註:
> Professor may think it would be simpler for you to include `numpy` by `from numpy import *` here, but I have to remind you that **it's a very dangerous way to import a module**, because it will mess up our namespace.
> 
> Actually, there are **627 names in `numpy`**, and it may cause the conflict between the same names. For example, there's another python module called `array`, and if we do this:
> ```python
> from numpy import *
> from array import *
> ```
> then the name `array` means `array.array`. But if we do this:
> ```python
> from array import *
> from numpy import *
> ```
> then the name `array` means `numpy.array`, and you could never call  `numpy.array` in the same program. Besides, naming our own variables with names already in `numpy`, like `number`, `angle`, `average`, etc, could cause similar problems.
> 
> To prevent this problem, it's better to keep only the variables defined by ourselves in the namespace. To achieve this, we usually import `numpy` by this way (in fact, almost everyone does it this way):
> ```python
> import numpy as np
> 
> a = np.arange(4)
> b = np.array(range(10))
> ```
> You can change the alias (the `np` after `as`) to anything you want, but `np` is a common alias for `numpy`.
> 
> Actually, in my code, I also use `import vpython as vp` to import `vpython`, so that I don't have to worry about the names. (Yeah the code may be more complicated, so it's important to use a short but clear alias.) 
> 
> Anyways, you are familiar with `vpython` names, so you can still use `from vpython import *` for convenience, but it's highly recommended to use `import numpy as np`.

## II. Practice: Longitudinal Spring-Ball Wave  
```python
from vpython import *

A, N, omega = 0.10, 50, 2*pi/1.0
size, m, k, d = 0.06, 0.1, 10.0, 0.4

scene = canvas(title='Spring Wave', width=800, height=300, background=vec(0.5,0.5,0), center = vec((N-1)*d/2, 0, 0))
balls = [sphere(radius=size, color=color.red, pos=vector(i*d, 0, 0), v=vector(0,0,0)) for i in range(N)]
springs = [helix(radius = size/2.0, thickness = d/15.0, pos=vector(i*d, 0, 0), axis=vector(d,0,0)) for i in range(N-1)]

t, dt = 0, 0.001
while True:
    rate(1000)
    t += dt
    
    balls[0].pos = vector(A * sin(omega * t ), 0, 0)
    for i in range(N-1):
        springs[i].pos = balls[i].pos
        springs[i].axis = balls[i+1].pos - balls[i].pos
        
    for i in range(1, N):
        if i == N-1:
            balls[-1].v += -k * vector((springs[-1].axis.mag-d), 0, 0)/m*dt
        else:
            balls[i].v += k * vector((springs[i].axis.mag-d), 0, 0)/m*dt - k * vector((springs[i-1].axis.mag-d), 0, 0)/m*dt
        balls[i].pos += balls[i].v*dt
```
1. **Array based simulation**  

   Change the above codes to the following array-based codes. We also need to complete the 2 lines commented by `##`. Notice that, in the very first line, we import `numpy` as a shortened name ‘`np`’. Later at the line marked \#5, we use `numpy`’s class (`arange`) to generate the arrays to store the positions of the balls, the original positions of the balls, the velocity of the balls, and the spring lengths. We need to use `np.arange()` as the proper syntax for such purpose.  

    ```python
    import numpy as np
    from vpython import *

    A, N, omega = 0.10, 50, 2*pi/1.0
    size, m, k, d = 0.06, 0.1, 10.0, 0.4
    
    scene = canvas(title='Spring Wave', width=800, height=300, background=vec(0.5,0.5,0), center = vec((N-1)*d/2, 0, 0))
    balls = [sphere(radius=size, color=color.red, pos=vector(i*d, 0, 0), v=vector(0,0,0)) for i in range(N)]                 #3
    springs = [helix(radius = size/2.0, thickness = d/15.0, pos=vector(i*d, 0, 0), axis=vector(d,0,0)) for i in range(N-1)]  #3
    #1
    ball_pos, ball_orig, ball_v, spring_len = np.arange(N)*d, np.arange(N)*d, np.zeros(N), np.ones(N)*d                      #5

   t, dt = 0, 0.001
    while True:
        rate(1000)
        t += dt
        
        ball_pos[0] = A * sin(omega * t )           #4
        ## spring_len[:-1] =
        ## ball_v[1:] +=                            #6
        ball_pos += ball_v*dt

        for i in range(N): balls[i].pos.x = ball_pos[i]     #3
        for i in range(N-1):                                #3
            springs[i].pos = balls[i].pos                   #3
            springs[i].axis = balls[i+1].pos - balls[i].pos #3
        #2
    ```

2. To get a clearer view of the wave, we plot the longitudinal displacement of the balls from their original positions by a transverse displacement.  

   At \#1, add code  
    ```python
    c = curve([vector(i*d, 1.0, 0) for i in range(N)], color=color.black)
    ```
   
   At \#2, add code  
    ```python
        ball_disp = ball_pos - ball_orig
        for i in range(N):
            c.modify(i, y = ball_disp[i]*4+1)
    ```
    
   ![hw7-1.png](pic/hw7-1.png)  

3. Since we already have the clear representation of the wave, we can remove the ball-spring plotting, marked by \#3. Notice that although the wave shown is with transverse motion, but it is indeed a longitudinal wave with a transverse wave representation.  

4. You may have seen such things in a video game that an object moves out of the right end of the screen and comes back from the left end of the screen. This is called “periodically boundary condition”, a very often used assumption in physics and engineering. In our system here, it means that the last ball and the zeroth ball is also connected by a spring in a fashion that looks like the zeroth ball is to the right of the last ball, or the last ball is to the left of the zeroth ball.  

    Now, modify the codes marked by `##`, and add one or two more lines to achieve such “periodically boundary condition”. You will see that the wave is generated from both ends towards the center because now the last one is also connected to the zeroth one, which is the source of the wave.  

    ![hw7-2.png](pic/hw7-2.png)  
    ![hw7-3.png](pic/hw7-3.png)  

5. As you saw in Practice 4, the wave amplitude at times gets larger and at times gets smaller. This means that this wave we try to create is not a “normal mode” of the system. The normal modes of the system are those waves whose amplitude can keep constant. This means that after going a full circle (Notice that we have the tail connected to the head, so we can view this system as a circle), the wave repeats itself, or we can say that wave satisfies the periodically boundary condition. These are waves with wavelength <img src="https://latex.codecogs.com/gif.latex?\dpi{150}&space;$\lambda = Nd/n$" title="$\lambda = Nd/n$" height=16/>, where <img src="https://latex.codecogs.com/gif.latex?\dpi{150}&space;$n=1, 2, 3,...$" title="$n=1, 2, 3,...$" height=16/> (i.e. wavenumber <img src="https://latex.codecogs.com/gif.latex?\dpi{150}&space;$K=2\pi/\lambda=2\pi n/(Nd)$" title="$K=2\pi/\lambda=2\pi n/(Nd)$" height=16/> ). We can observe such wave by initially assigning balls to their proper positions, removing the external source, and letting the system to go by itself. This means changing \#5 to  

    ```python
    Unit_K, n = 2 * pi/(N*d), 10
    Wavevector = n * Unit_K
    phase = Wavevector * arange(N) * d
    ball_pos, ball_orig, ball_v, spring_len = np.arange(N)*d + A*np.sin(phase), np.arange(N)*d, np.zeros(N), np.ones(N)*d
    ```

   ![hw7-4.png](pic/hw7-4.png)  

   Notice in the last line, `ball_pos` is set to `np.arange(N)*d + A*np.sin(phase)`, in which `np.arange(N)*d` is the array for the balls’ original positions at balance points. `A` in `A*np.sin(phase)` is the amplitude of the longitudinal wave, and each ball is originally displaced by `A*sin(phase)` before the simulation begins. `phase` in `phase = Wavevector * arange(N) * d` is the array for the phase of each ball’s position in the longitudinal wave. We use `np.sin(phase)` because we want an array, elements of which are the sin function of phase, which is also an array. Then this array can be added to another array, `np.arange(N)*d`.  
    
   You also need to remove \#4 and add after \#6 a line to handle zeroth ball’s velocity `ball_v[0]`.  

In the simulation, you will see clearly the oscillating standing wave with `n = 10`. Try to obtain the period (T) of the standing wave and the corresponding angular frequency (`omega = 2*pi / T`). For higher accuracy, please change `dt = 0.001` to `dt = 0.0003`.  

## III. Homework  
### MUST (5%)

In a crystal, only certain normal modes of waves can exist. These modes are called “Phonons”. Practice 5. shows one of such modes in a simplified 1-dimensional crystal consisted of only 50 balls (atoms). As you already see, when the wavevector is given, the angular frequency (`omega =2 pi/T`) is decided by the system.  

Now we want to know the relationship between the angular frequency and the wavevector, which is called the dispersion relationship. Modify your program from Practice 5. such that you can obtain the angular frequency (`omega`) for n from 1 to N/2. Use the graph plotting similar to homework 5 (must) to plot the the angular frequency versus the magnitude of wavevector (which is the wavenumber, or angular wavenumber).  
    
