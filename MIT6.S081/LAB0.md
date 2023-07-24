# [Lab util: Unix utilities](https://pdos.csail.mit.edu/6.828/2022/labs/util.html)

使用別人建好的 docker

``` shell
sudo docker pull linxi177229/mit6.s081

sudo docker run --name mit6.s081 -itd linxi177229/mit6.s081
sudo docker attach mit6.s081 
```

Build and run xv6:

``` sh
make qemu
```

To quit qemu！！

``` sh
ctrl + a # release
x
```



## sleep(easy)

The solution is in the `user/sleep.c` file.

**Target** 

Implement the UNIX program `sleep` for `xv6`; 





Run the program
``` sh
make qemu
sleep 10
```

get grade

``` sh
./grade-lab-util sleep
# or
make GRADEFLAGS=sleep grade
```









