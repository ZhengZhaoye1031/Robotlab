## 串级PID

### 前言：

在我们进行电机的位置控制时，如果当前值与实际值较远的话，电机在运动过程中的速度会很快，同时与总会导致较大的超调，且无论怎么修改参数都很难让系统表现得更好一些，这时就需要用到串级PID了。

### 串级PID控制电机

1. 打开工程文件；将6020ID设置为1

2. 在Motor_cmd_Task.c文件中添加控制代码
```
   
   int16_t angle = 0;
   float max_angle=8192; 
   uint8_t Xnum=0;     //简单的标志位来控制串级PID内外环权重比
   int n=3;
   float  in_XP=0;
   float in_XI=0;
   float  in_XD=0;
   float Xp=0;
   float Xd=0;
   float text = 0;
   
   void Motor_cmd_task(void const * argument)
   {
   	can_filter_init();
   	PID_Total_Init();
   
   	for(;;){
   
   		Xnum++;
   		if(Xnum > n){
   			Xnum = 0 ;
   			text = PID(Motor_measure[4].angle,angle);
   		}
   		PID_Init(&Motor_pid[4],PID_POSITION_SPEED,in_XP,in_XI,in_XD,30000,30000,0);
   		Motor_measure[4].Output = PID_calc(&Motor_pid[4],Motor_measure[4].speed,text);
   		Set_motor_cmd(&hcan1,Last_STDID,Motor_measure[4].Output,0,0,0);
   
   		osDelay(10);
   	}
   }
   
   float PID(float now,float target)
   {
   
   	 static int err=0;
   
   	float out_put;
   	if(target<0) target=8191+target;//不加的话电机会乱转
   	
   	 err=target-now;                //计算偏差
   	
   		if( err>(max_angle/2)){  err= err-max_angle;}//解决360度问题让电机走最优路径
   	else if( err<-(max_angle/2)){ err= err+max_angle;}
   	
    out_put=Xp/100*err-Xd/100*(Motor_measure[4].speed);   //PI控制器
   
     
   //	if(out_put>max_output)out_put=max_output;
   //	else if(out_put<-max_output)out_put=-max_output;//限制输出，加上后改max_output可调节电机回弹的力气
   
     
   	 return out_put;                         //输出
   	
   }
   
```
简单介绍一下这几行代码的作用

1.将内环（速度环）初始化放进循环，内外环PID参数置零且设置全局变量方便debug调参
```      
      float in_XP=0;
      float in_XI=0;
      float in_XD=0;
      float Xp=0;
      float Xd=0;
      
      PID_Init(&Motor_pid[4],PID_POSITION_SPEED,in_XP,in_XI,in_XD,30000,30000,0);      
```
2.设置标志位，控制内外环权重比
```
      uint8_t Xnum=0;     //简单的标志位来控制串级PID内外环权重比
      int n=3;
      
      Xnum++;
      if(Xnum > n){
      	Xnum = 0 ;
      	text = PID(Motor_measure[4].angle,angle);
      }
```
3.外环（位置环）PID计算函数
```
	float PID(float now,float target)
	  {
	  	static int err=0;
	  
	  	float out_put;
	  	if(target<0) target=8191+target;//不加的话电机会乱转
	  	
	    err=target-now;                //计算偏差
	  	
	    if( err>(max_angle/2)){  err= err-max_angle;}//解决360度问题让电机走最优路径
	  	else if( err<-(max_angle/2)){ err= err+max_angle;}
	  	
	    out_put=Xp/100*err-Xd/100*(Motor_measure[4].speed);   //PI控制器
	    
	  //	if(out_put>max_output)out_put=max_output;
	  //	else if(out_put<-max_output)out_put=-max_output;//限制输出，加上后改max_output可调节电机回弹的力气
	  return out_put;                         //输出
	  }
```
      max_output为外环输出的最大绝对值，等于内环的最大目标值，可以理解为电机运动的最大速度值

4.硬件准备好后，将代码烧录进MCU，之后进入debug模式进行PID的调参，完成串级的控制

   [PID调参教程1](https://blog.csdn.net/qq_45396672/article/details/118057838?spm=1001.2014.3001.5506)

   [PID调参教程2](https://blog.csdn.net/qq_45396672/article/details/118057838?spm=1001.2014.3001.5506)

5.串级PID运行逻辑

   与位置环相似，输入目标位置值，外环（位置环）计算出达到目标值所需的速度，作为内环（速度环）的目标值输入，计算出达到目标速度所需的加速度，也就是电流值，最后输出电流实现控制电机。

<img src="https://raw.githubusercontent.com/ZhengZhaoye1031/Robotlab/master/docs/img/cascade.png"/>

