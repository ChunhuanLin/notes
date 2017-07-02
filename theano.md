## 使用GPU  
theano需要手工设定使用GPU，方法如下:  
`vim ~/.theanorc`

add these content
[global]
device=gpu
floatX=float32
