**预备工作：**  
1. client/server分别生成公钥私钥  
2. server添加信任的clients的用户名,密码密文,公钥,存储在本地;client添加server公钥

**开始：**  
**登录：**  
1. client提交用户名,server检查是否在信任的clients中;若是则返回密码加密算法K,否则拒绝
2. client用K加密密码,得到P'然后依次用client私钥,server公钥加密P'得到P'';返回P''给server
3. server依次用server私钥,client公钥解码P''得到结果后与本地存储的密码密文进行比较；相等则进入下一阶段，不等则拒绝登录  
**传输准备：**  
1. client查询共享区域,server返回文件列表  
2. client请求传输A
3. server拒绝或者允许;允许则返回A的大小,传输段数量,传输段大小  
**传输：**  
1. server发送数据,client接收数据并对统一数据段内的数据包进行校验整合,校验失败时立即请求重传(注：多次重传失败记录数据缺失)  
2. 若属于同一数据段内的数据包id连续,则进入下一步,否则开启重传计时器  
3. client收到最后一个数据包并且不再有重传请求时返回传输结束信号,client断开连接;server接收到结束信号后断开连接  
注：双方的连接活性由心跳保持决定