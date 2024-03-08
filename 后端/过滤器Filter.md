javaweb三大组件servlet，filter和license。
我们使用的是spring boot项目，要想使用filter要在启动类上加上@ServletCompentScan才行
![[Pasted image 20231011101707.png]]
![[Pasted image 20231011101001.png]]
导包
![[Pasted image 20231011101037.png]]
放行使用chain.doFilter
![[Pasted image 20231011101726.png]]
拦截路径
![[Pasted image 20231011102259.png]]

过滤器链
![[Pasted image 20231011102724.png]]