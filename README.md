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

 
