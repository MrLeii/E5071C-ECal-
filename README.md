# E5071C-ECal-
5071C special points

8 channels and two trace per channel畫面分割法(From page 57 of E5071B programmer guide)
:DISP:SPL D1234_5678
Calc1:par:coun 2
:DISP:WIND1:SPL D1_2
Calc1:par1:sel
Calc1:par1:def s11
Calc1:form pol
Calc1:par2:sel
Calc1:par2:def s22
Calc1:form pol
Calc2:par:coun 2
:DISP:WIND2:SPL D1_2
Calc2:par1:sel
Calc2:par1:def s11
Calc2:form pol
Calc2:par2:sel
Calc2:par2:def s22
Calc2:form pol
Calc3:par:coun 2
:DISP:WIND3:SPL D1_2
Calc3:par1:sel
Calc3:par1:def s11
Calc3:form pol
Calc3:par2:sel
Calc3:par2:def s22
Calc3:form pol
Calc4:par:coun 2
:DISP:WIND4:SPL D1_2
Calc4:par1:sel
Calc4:par1:def s11
Calc4:form pol
Calc4:par2:sel
Calc4:par2:def s22
Calc4:form pol
Calc5:par:coun 2
:DISP:WIND5:SPL D1_2
Calc5:par1:sel
Calc5:par1:def s11
Calc5:form pol
Calc5:par2:sel
Calc5:par2:def s22
Calc5:form pol
Calc6:par:coun 2
:DISP:WIND6:SPL D1_2
Calc6:par1:sel
Calc6:par1:def s11
Calc6:form pol
Calc6:par2:sel
Calc6:par2:def s22
Calc6:form pol
Calc7:par:coun 2
:DISP:WIND7:SPL D1_2
Calc7:par1:sel
Calc7:par1:def s11
Calc7:form pol
Calc7:par2:sel
Calc7:par2:def s22
Calc7:form pol
Calc8:par:coun 2
:DISP:WIND8:SPL D1_2
Calc8:par1:sel
Calc8:par1:def s11
Calc8:form pol
Calc8:par2:sel
Calc8:par2:def s22
Calc8:form pol

Special trick
1.	開4個channel或2個channel , 每個channel 2 trace 且都hold時,不能使用TRIG 及TRIG:SING 會出現trigger ignored, 但可使用init1,2,3,4.以sweep single channel
2.	sweep mode 分stepped 及 swept 兩種, reset 後default為 stepped, 在201點70KHz時 swept 需13.497ms, stepped 需 10.798ms
3.	開8個channel , 每個channel 2 trace 且都hold時,init1,2,3,4,5,6,7,8都可使用, 但init3;*OPC?在還未sweep 完成即立刻回覆1.故不能使用
4.	在init 時必須配合 :STAT:OPER:PTR 0 , :STAT:OPER:NTR 16 , :STAT:OPER:ENAB 16 , *SRE 128
:init6 ,  :STAT:OPER:COND? 才能知到已sweep完成
5. MOBI calibration 程序, sweep time 設為5 second 按第一個 open時, 還未sweep完成, :STAT:OPER:COND? 立刻回覆 0, 有錯誤, 接著按 short 時:STAT:OPER:COND?便等到sweep完成才回覆 0, 但calibration程序打開後, 尚未按open前, 使用interactive IO 下 init1, 再 :STAT:OPER:COND?, 會等到 sweep 完成才回覆 0

Sweep point is 5, set format to polar real and imag. No need to do calibration. SDAT and FDAT get same result. 須set 到某trace才能讀出資料. 在correction off 仍可讀資料
 
SEL trace 2
 
SEL trace 1

Set frequency to start 1GHz
 
Set points to 801
 
Single sweep command

Channel 1 sweep one time

Let trace 1 measue S11. 
 
SET trace 2 measure S22

*OPC? 須先 write 再 read
 
ONLY TURN OFF FREQ DATA

螢幕關掉

螢幕打開

 Ecal参数获取程序:
首先定义4端口Ecal名称为PortA，PortB，PortC，PortD。
对网分做完整的双端口校正，（其余的AC、AD、BC、BD一样算法）
然后切换到Ecal的PortA的open得到
Γ（OpenA）--此数值为Ecal出厂后固定参数，不会改变。
切换到Ecal的PortA的short得到
Γ（ShortA）--此数值为Ecal出厂后固定参数，不会改变。
切换到Ecal的PortA的Load得到
Γ（LoadA）--此数值为Ecal出厂后固定参数，不会改变。
切换到Ecal的PortB的Open得到
Γ（OpenB）--此数值为Ecal出厂后固定参数，不会改变。
切换到Ecal的PortB的short得到
Γ（ShortB）--此数值为Ecal出厂后固定参数，不会改变。
切换到Ecal的PortB的Load得到
Γ（LoadB）--此数值为Ecal出厂后固定参数，不会改变。
PortA thru PortB的时候量测到
S11_thru(AB) [ 式子中的 ],
S12_thru(AB) [ 式子中的 ],
S21_thru(AB)[ 式子中的 ],
S22_thru(AB) [ 式子中的 ],



Ecal早期测试：网分+N转sma线（在这个末端做单端口校正，或者做双端口校正）
然后+sma转接头（母头对母头）+sma cable线（2米或者3米）+sma转接头（母头对母头）+N转sma线+（N头校正器）
N头校正器接Open、short、load分别量测到的值即为Γ（OpenA），Γ（ShortA），Γ（LoadA）
 
软件部分：
网分设定成32bit：viPrintf (vi, "form:data real32\n");//real32
然后手动修改网分的起止频率和量测点数。
接下来点port1 open按钮：
 
这部分可以手动在网分上面点击校正，也可以投过软件按钮执行校正。
抓取数值的时候：
	char result[80000];//20001点的时候  这个数组起码要20001*8。
	CString tmpCMD,WriteCMD;

	WriteCMD=":calc1:par1:sel\n";//:calc4:par1:sel\n
	WriteCMDsToNA(WriteCMD);锁定channel1 trc1的数据
	WriteCMD=":calc1:data:fdat?\n";//抓取锁定的trc的数据
	WriteCMDsToNA(WriteCMD);

	BYTE arrBin[80000];
	viRead(vi, (ViBuf)arrBin, 80000, &actual);
	CString   strTemp2;   
	for(int l=8,p=0;l<(8*AnalyzerPoint+8);l++,p++)//AnalyzerPoint
	{
		strTemp2.Format("%02x",arrBin[l]); 
		char*   lp=strTemp2.GetBuffer(0);///转char* str.GetLength()  
		strTemp2.ReleaseBuffer();   
		result[p]=lp[0];
		result[p+1]=lp[1];
		p++;
	}
Result里面存储的就是16进制的那些字符。
	
抓取出来后可以透过如下函数直接存储在ini文件中
WritePrivateProfileString;



初始内部测试的时候，我们先存储两种类型，一种double型的，一种是二进制字符的存储数据，我们查看下数据的大小，以及后期能否直接透过数据看出Ecal校正的数值是否正确。




Ecal校正程序：
客户在使用Ecal校正的时候，portA锁上open  后会得到T（o_m）
Short和load同样会得到T（s_m），T（L_m）
这个时候可以借助Ecal参数获取程序得到的单端口数据，运算得到单端口的校正参数。得到E(DF)、E(SF)、E(RF) 、E(DR)、E(SR)、E(RR)
然后根据下面矩阵运算公式2.13和2.14，得到PortA的E_DF(A),E_SF(A),E_RF(A)
转换成接网分的port2，得到PortA的E_DR(A),E_SR(A),E_RR(A)

Thru的时候，量测到4个参数：S11_thru_m, S12_thru_m, S21_thru_m, S22_thru_m代入2.28,2.29.2.30.2.31公式就可以算出ELF，ELR，ETF、ETR。
 
代入下面式子中的
 
先运算出上面中转数据，然后透过下列公式
 运算出E_LF(AB),E_TF(AB),E_TR(AB),E_LR(AB).

客户量测时候直接跟以前的双端口数据一样，因为已经有了完整的10个参数数值。

Ecal校正需要提取出Ecal校正参数中临近的两个点，透过内差来处理出准确的校正参数。
这个要考虑到到时候做完一次校正花费的时间。
