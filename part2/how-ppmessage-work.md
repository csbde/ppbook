# PPMessage 如何工作

PPMessage客服平台包括 PPKefu，PPCom，PPMessage服务器。

--------

#### PPKefu, PPConfig, PPCom 与 PPMessage Server

+--------------------+     3      +-----------------------+      4      +--------------------+
|  Customer Client   +----------->+        Server         +------------>+   Service Client   |
|                    |  message   |                       |   message   |                    |
|     [PPCom]        +<-----------+      [PPMessage]      +<------------+      [PPKefu]      |
+--------+-----------+     6      +--------+----+---------+      5      +----------+---------+
                                           |    ^
                                           |    |
                                         1 |    | 2
                                           |    |
                                           v    |
                                  +--------+----+---------+
                                  |   Service Management  |
                                  +                       +
                                  |      [PPConfig]       |
                                  +-----------------------+


1. 通过 PPConfig 初始化 PPMessage 系统

2. 已经初始化的 PPMessage 会自动定向到 PPkefu。

3. 网站用户通过 PPCom 在网站上发消息，到 PPMessage。

4. 客服通过 PPKefu 接收消息。

5. 客服通过 PPKefu 发送消息，PPMessage 服务器接收消息并进行处理。

6. 服务器给网站用户推送消息，用户通过 PPCom 接收消息。
