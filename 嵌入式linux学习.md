
## 什么是交叉编译工具链
![image.png](https://raw.githubusercontent.com/KrealHtz/NoteImage/master/data/202412242205021.png)

- 本地工具链: build == host == target
- 交叉编译工具链: build == host != target
- 对应于 -build，--host 和 --target通过 autoconf 脚本配置参数默认情况下，由 autoconf 自动猜测配置为当前机器
