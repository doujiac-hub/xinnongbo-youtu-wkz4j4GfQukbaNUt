
### 一、前言


大家好！我是付工。


西门子PLC是工控领域使用非常多的一种PLC品牌，对于上位机开发人员来说，对于西门子PLC的通信，我们一般可以采取哪些通信方式呢？


今天跟大家分享一下上位机实现与西门子PLC的通信方案。


### 二、串口通信


西门子PLC早期主要以S7\-200、S7\-300/400为主，后面逐步被S7\-200Smart、S7\-1200/1500所替代。


目前只有S7\-200与S7\-200Smart是自带串口，接口标准为RS485。


如果是其他型号，想要实现串口通信，需要增加相应的串口通信模块。


西门子PLC的串口通信协议主要有2种，一种是西门子的PPI协议，另一种是ModbusRTU协议。


* 西门子PPI协议是一个不开放的协议，可以通过抓包来进行报文分析，但是一般不推荐使用，因为PPI协议是一个需要二次确认的协议，使用时比较麻烦。
* 西门子PLC也支持ModbusRTU协议，提供Modbus库指令，直接调用即可，但是会涉及到一定的编程。
* 如果选择ModbusRTU协议，需要区分主站从站，一般来说，上位机与PLC通信，上位机作为主站，PLC作为从站。


### 三、以太网通信


以太网是西门子PLC主要的通信方式，目前主流的S7\-200Smart、S7\-1200/1500都内置以太网接口，上位机与西门子PLC实现以太网通信主要有S7通信、ModbusTCP通信、开放式TCP通信以及OPCUA通信。


* S7协议是西门子PLC的私有协议，虽然不开放，但是目前有很多开源免费的通信库可以使用，.Net框架下比较推荐使用s7netPlus、sharp7。如果想要实现标签通信，S7协议有个升级版叫做S7Plus协议，可以实现标签通信，目前尚未成熟，项目上使用较少。
* 西门子PLC同样支持ModbusTCP通信，会有对应的Server和Client库指令，一般来说，上位机与PLC通信，西门子PLC作为ModbusTCPServer，上位机作为ModbusTCPClient。
* 开放式TCP通信，就是我们常说的自由口通信，这个主要是针对一些自定义协议的情景，自由度较高，但是对开发人员的要求也较高。上位机与PLC之间进行开放式TCP，上位机可以作为TCPClient或者TCPServer，PLC需要编写对应的TCPServer和TCPClient程序。
* OPCUA通信，对于S7\-1200、S7\-1500的部分型号，可以支持OPCUA通信，一般来说，PLC作为OPCUA服务器，上位机作为OPCUA客户端，OPCUA也是一种基于标签名称的通信方式。


### 四、OPC通信


OPC通信是工业控制中常用的一种通信方式，OPC相当于是中间件，由OPC软件对接PLC，然后开放一个OPC接口给上位机进行使用。西门子PLC常用的OPC通信方案有以下几种：


* PC Access系列：西门子针对S7\-200提供PC\-Access软件，针对S7\-200 Smart提供PC\-Access Smart软件，可以直接通过这些软件实现OPC通信。
* Simatic Net系列：Simatic Net是西门子主推的OPC软件，支持西门子全系列，通过Simatic Net可以支持OPCUA和OPCDA通信接口。
* KepServer软件：KepServer同样作为一款商业OPC软件，在国内使用率非常高，同样也支持西门子全系列，通过KepServer也可以支持OPCUA和OPCDA通信接口。


### 五、如何选择


在以上众多的通信方式中，我们该如何选择？


如果我们要实现串口通信，优先选择ModbusRTU，其次考虑PPI，最后考虑OPC方式。


如果我们要实现以太网通信，优先选择S7协议，其次考虑ModbusTCP和开放式TCP，最后考虑OPC方式。


S7通信协议最大的优势在于不用编写PLC程序，且覆盖面较广，只要是西门子PLC，无论是S7\-200/300/400，还是S7\-200Smart/1200/1500，只要PLC具备以太网接口，均支持S7通信协议。


虽然S7通信协议不需要编写PLC程序，但仍然需要进行一定的配置，具体如下：


* 勾选允许Put/Get：PLC侧需要设置勾选允许来自远程对象的Put/Get通信访问
* DB块去除优化访问：如果要与DB块数据通信，需要要去除DB的优化的块访问
* 务必保证通信地址是有效地址：如果你要读取DB存储区，必须要提前创建好DB存储区，必须保证读取的必须是有效地址，其他存储区也不能超过范围。
* 调整通信负载：如果以上均没问题，可以适当调整一下通信负载参数。


 本博客参考[悠兔机场官网订阅](https://5tutu.com)。转载请注明出处！
