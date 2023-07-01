# [如何fork yihong的gitblog](https://github.com/kenwoodjw/gitblog/issues/1)

1. fork 仓库
2. 修改github action的用户名和邮箱
3.  创建secret
![image](https://github.com/kenwoodjw/gitblog/assets/10386710/64a1f874-9b88-405c-bc7c-5f3297b8faa2)

4. 设置action权限和secret
**tips** : 是Repository secrets，不是environment secrets，配错踩坑了
![image](https://github.com/kenwoodjw/gitblog/assets/10386710/f7cd4c17-fd7b-415d-9e9a-327aa7a8e13b)
要勾选read and write权限，不然提示403
![image](https://github.com/kenwoodjw/gitblog/assets/10386710/fa229d08-0121-4991-9707-e904a11e7991)
5. 开启issue权限
![image](https://github.com/kenwoodjw/gitblog/assets/10386710/a53d3de4-f944-4905-8103-c8d8d8e7d8f5)
往下拉，勾选issue
![image](https://github.com/kenwoodjw/gitblog/assets/10386710/66486b80-27f7-4852-a9f8-b227f2724091)

