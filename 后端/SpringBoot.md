![[Pasted image 20230925145854.png]]
![[Pasted image 20230925145931.png]]
![[Pasted image 20230925145946.png]]
![[Pasted image 20230925152005.png]]
![[Pasted image 20230925152022.png]]
![[Pasted image 20230925152047.png]]

## 注解
@SpringBootApplication扫描具有@Component的类交给ioc管理，范围是启动类所在包及其子包
@Configuration[@Configuration的作用-CSDN博客](https://blog.csdn.net/qiuz1024/article/details/100530260)
（为了实现配置类只有单例）


## 配置优先级
![[Pasted image 20231013144825.png]]
![[Pasted image 20231013145237.png]]
![[Pasted image 20231013145339.png]]

## SpringBoot原理
##### 起步依赖
![[Pasted image 20231013152813.png]]

##### 自动配置
![[Pasted image 20231013153222.png]]
引入第三方依赖扫描不到进不去ioc怎么办？
![[Pasted image 20231013153915.png]]
![[Pasted image 20231013154403.png]]
第三方注解：![[Pasted image 20231013154436.png]]
第三方类：![[Pasted image 20231013154502.png]]
启动类只用加上第三方注解就可以使用ioc代理单例了：![[Pasted image 20231013154558.png]]

原理
![[Pasted image 20231013163858.png]]
![[Pasted image 20231013165316.png]]
![[Pasted image 20231013165202.png]]
![[Pasted image 20231013165236.png]]


## 案例
![[Pasted image 20231013171235.png]]![[Pasted image 20231013171505.png]]
[Day14-11. SpringBoot原理-自动配置-案例(自定义starter实现)_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1m84y1w7Tb?p=193&vd_source=4a300872c081074c2464fd7d2f49ed35)

