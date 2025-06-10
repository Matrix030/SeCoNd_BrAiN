![[Pasted image 20250610100433.png]]

![[Pasted image 20250610100438.png]]

SVR has a tube

the width of the tube - Epsilon
![[Pasted image 20250610100513.png]]
The Tube is called - Epsilon Insensitive Tube i.e Any point in our dataset that fall inside the tube, their error will be disregarded. we do not care about the error in the tube.
![[Pasted image 20250610100950.png]]

Gives a little bit of buffer to our model and at the same time the points which are outside (we do care about the error) -  **AND** the error here is the distance between the point and the tube and **NOT** the line.


![[Pasted image 20250610100827.png]]
`*` is used if the point is below the tube.

Sum of the distances should be minimal.
![[Pasted image 20250610101121.png]]

Effectively any point in the plot can be a vector, the one which are outside the tube are the support vector as they are supporting the formation of the tube.

![[Pasted image 20250610101138.png]]


![[Pasted image 20250610101933.png]]
Non Linear SVR
