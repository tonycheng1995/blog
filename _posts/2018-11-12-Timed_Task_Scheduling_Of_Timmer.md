---
layout: post
title: "定时任务调度之Timer"
date:   2018-10-30 +0800
toc: true
category: timer
tags: 定时任务 timer
excerpt: 本篇文章主要讲解使用Timer实现定时任务调度。
---
#### 定时任务调度：基于给定的时间点,给定的时间间隔或者给定的执行次数自动执行的任务。这里主要介绍两种任务调度方式：
#### 1.timer:由JDK提供，不需要其他jar包
#### 2.quartz：比timer更强大，完善

## 1.timer任务Demo
#### 任务类MyTimerTask.java
{% highlight java %}
public class MyTimerTask extends TimerTask{
	private String name;
	public MyTimerTask(String inputName) {
		name=inputName;
	}
	@Override
	public void run() {
		// 打印当前name的内容
		System.out.println("当前name为："+name);
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
}
{% endhighlight java %}

#### 执行任务调度类MyTimer.java
{% highlight java %}
public class MyTimer {
	public static void main(String[] args) {
		//1.创建一个timer实例
		Timer timer=new Timer();
		//2.创建一个MyTimerTask实例
		MyTimerTask myTimerTask=new MyTimerTask("No.1");
		//3.通过timer定时定频率调用myTimerTask的业务逻辑
		//即第一次执行是在当前时间的两秒后，之后每隔一秒执行一次
		timer.schedule(myTimerTask, 2000L,1000L);
	}
}
{% endhighlight java %}
#### 运行任务调度类，控制台每隔1s打印出一条数据。

## 2.讲解Timmer中的方法
### 2.1schedule(task,time)
#### 在时间等于或超过time的时候执行且仅执行一次task
#### *task：所要安排的任务
#### *time：执行任务的时间
#### MyTimerTask.java
{% highlight java %}
public class MyTimerTask extends TimerTask{
	private String name;
	public MyTimerTask(String inputName) {
		name=inputName;
	}
	@Override
	public void run() {
		//以yyyy-MM-dd HH:mm:ss的格式打印当前执行时间
		//如2016-11-11 00:00:00
		Calendar calendar=Calendar.getInstance();
		SimpleDateFormat sf=new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
		System.out.println("当前执行时间："+sf.format(calendar.getTime()));
		// 打印当前name的内容
		System.out.println("当前name为："+name);
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
}
{% endhighlight java %}
#### MyTimer.java
{% highlight java %}
public class MyTimer {
	public static void main(String[] args) {
		//1.创建一个timer实例
		Timer timer=new Timer();
		//2.创建一个MyTimerTask实例
		MyTimerTask myTimerTask=new MyTimerTask("No.1");
		/**
			* 获取当前时间,并设置成距离当前时间3s之后的时间
			* 如当前时间是2017-12-12 12:12:12
			* 则设置后的时间则为2017-12-12 12:12:15
			*/
		Calendar calendar=Calendar.getInstance();
		SimpleDateFormat sf=new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
		System.out.println(sf.format(calendar.getTime()));
		calendar.add(Calendar.SECOND, 3);
		/**
			* 1.在时间等于或超过time的时候执行且仅执行一次task
			* 如在2016-11-11 11:11:11执行一次task:打印任务的名字
			*/
		myTimerTask.setName("schedule1");
		timer.schedule(myTimerTask, calendar.getTime());
	}
}
{% endhighlight java %}

### 2.2 schedule(task,time,period)
#### 时间等于或超过time时首次执行的task,之后每隔period毫秒重复执行一次task
#### task：所要安排的任务
#### time：首次执行任务的时间
#### period:执行一次task的时间间隔，单位是毫秒
#### MyTimer.java
{% highlight java %}
/**
	* 2.时间等于或超过time时首次执行task
	* 之后每隔period毫秒重复执行一次task
	* 如在2018-12-12 12:12:12第一次执行task:打印任务的名字
	* 之后每隔两秒执行一次task
	*/
myTimerTask.setName("schedule2");
timer.schedule(myTimerTask, calendar.getTime(),2000);
{% endhighlight java %}

### 2.3 schedule(task,delay)
#### 等待delay毫秒后执行且仅执行一次task
#### task:所要安排的任务
#### delay:执行任务前的延迟时间，单位是毫秒
{% highlight java %}
/**
	* 3.等待delay毫秒后执行且仅执行一次task
	* 如现在是2018-12-12 12:12:12
	* 则在2018-12-12 12:12:13执行一次task:打印任务名称
	*/
myTimerTask.setName("schedule3");
timer.schedule(myTimerTask, 1000);
{% endhighlight java %}
### 2.4 schedule(task,delay,period)
#### 等待delay毫秒后首次执行task,之后每隔period毫秒重复执行一次task
#### task:所要安排的任务
#### delay:执行任务前的延迟时间，单位是毫秒
#### period:执行一次task的时间间隔，单位是毫秒
{% highlight java %}
/**
	* 4.等待delay毫秒后首次执行task
	* 之后每隔period毫秒重复执行一次task
	* 如现在是2018-12-12 12:12:12
	* 则在2018-12-12 12:12:13第一次执行task:打印任务的名字
	* 之后每隔两秒执行一次task
	*/
myTimerTask.setName("schedule4");
timer.schedule(myTimerTask, 3000,2000);
{% endhighlight java %}
### 2.5 scheduleAtFixedRate的两种用法
#### scheduleAtFixedRate(task,time,period)：时间等于或超过time时首次执行task,之后每隔period毫秒重复执行一次task
#### task:所要安排的任务
#### time:首次执行任务的时间
#### period:执行一次task的时间间隔，单位是毫秒
{% highlight java %}
/**
	* 1.时间等于或超过time时首次执行task
	* 之后每隔period毫秒重复执行一次task
	* 如在2018-12-12 12:12:12第一次执行task：打印任务的名字
	* 之后每隔两秒执行一次task
	*/
myTimerTask.setName("scheduleAtFixedRate1");
timer.scheduleAtFixedRate(myTimerTask, calendar.getTime(), 2000);
{% endhighlight java %}
#### scheduleAtFixedRate(task,delay,period):等待delay毫秒后首次执行task,之后每隔period毫秒重复执行一次task
#### task:所要安排的任务
#### delay:执行任务前的延迟时间，单位是毫秒
#### period:执行一次task的时间间隔，单位是毫秒
{% highlight java %}
/**
	* 2.等待delay毫秒后首次执行task
	* 之后每隔period毫秒重复执行一次task
	* 如现在是2018-12-12 12:12:12
	* 则在2018-12-12 12:12:13第一次执行task:打印任务的名字
	* 之后每隔两秒执行一次task
	*/
myTimerTask.setName("scheduldAtFixedRate2");
timer.scheduleAtFixedRate(myTimerTask, 3000, 2000);
{% endhighlight java %}

### 2.7 timerTask下的函数
#### cancel():取消当前TimerTask里的任务
#### 在MyTimerTask.java类里添加计时器，当执行完3次以后取消任务
{% highlight java %}
public class MyTimerTask extends TimerTask{
	private String name;
	private Integer conut=0;
	public MyTimerTask(String inputName) {
		name=inputName;
	}
	@Override
	public void run() {
		if(conut<3) {
			//以yyyy-MM-dd HH:mm:ss的格式打印当前执行时间
			//如2016-11-11 00:00:00
			Calendar calendar=Calendar.getInstance();
			SimpleDateFormat sf=new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
			System.out.println("当前执行时间："+sf.format(calendar.getTime()));
			// 打印当前name的内容
			System.out.println("当前name为："+name);
			conut++;
		}else {
			cancel();
			System.out.println("任务取消！");
		}
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
}
{% endhighlight java %}
#### scheduledExecutionTime():返回此任务最近实际执行的已安排执行的时间，long型
{% highlight java %}
myTimerTask.setName("schedule");
timer.schedule(myTimerTask, 3000);
System.out.println("预执行时间："+sf.format(myTimerTask.scheduledExecutionTime()));
{% endhighlight java %}
### 2.8 timer下的函数
#### cancel():终止此计时器，丢弃所有当前已安排的任务。
{% highlight java %}
public class CancelTest {
	public static void main(String[] args) throws InterruptedException {
		//创建Timer实例
		Timer timer=new Timer();
		//创建TimerTask实例
		MyTimerTask task1=new MyTimerTask("task1");
		MyTimerTask task2=new MyTimerTask("task2");
		//获取当前的执行时间并打印
		Date startTime=new Date();
		SimpleDateFormat sf=new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
		System.out.println("开始执行时间:"+sf.format(startTime));
		//task1首次执行是距离现在时间3s后执行,之后每隔2s执行一次
		//task2首次执行时距离现在时间1后执行，之后每隔2s执行一次
		timer.schedule(task1, 3000,2000);
		timer.schedule(task2, 1000, 2000);
		//休眠5s
		Thread.sleep(5000);
		//获取当前的执行时间并打印
		Date cancelTime=new Date();
		System.out.println("取消任务时间："+sf.format(cancelTime));
		//取消所有任务
		timer.cancel();
		System.out.println("任务取消！");
	}
}
{% endhighlight java %}

#### purge()：从此计时器的任务队列中移除所有已取消的任务。返回从队列中移除的任务数
{% highlight java %}
public class CancelTest {
	public static void main(String[] args) throws InterruptedException {
		//创建Timer实例
		Timer timer=new Timer();
		//创建TimerTask实例
		MyTimerTask task1=new MyTimerTask("task1");
		MyTimerTask task2=new MyTimerTask("task2");
		//获取当前的执行时间并打印
		Date startTime=new Date();
		SimpleDateFormat sf=new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
		System.out.println("开始执行时间:"+sf.format(startTime));
		//task1首次执行是距离现在时间3s后执行,之后每隔2s执行一次
		//task2首次执行时距离现在时间1后执行，之后每隔2s执行一次
		timer.schedule(task1, 3000,2000);
		timer.schedule(task2, 1000, 2000);
		System.out.println("当前取消任务数："+timer.purge());
		//休眠5s
		Thread.sleep(5000);
		//获取当前的执行时间并打印
		Date cancelTime=new Date();
		System.out.println("取消任务时间："+sf.format(cancelTime));
		//取消所有任务
		task2.cancel();
		System.out.println("当前取消任务数："+timer.purge());
	}
}
{% endhighlight java %}
### 2.9 schedule与scheduleAtFixedRate的区别
#### ·首次计划执行时间早于当前时间
#### schedule方法
#### “fixed-delay”,如果第一次执行时间被dalayl了，随后的执行时间按照上一次实际执行完成的时间点进行计算
{% highlight java %}
public static void main(String[] args) {
	//规定时间格式
	final SimpleDateFormat sf=new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
	//获取当前的具体时间
	Calendar calendar=Calendar.getInstance();
	System.out.println("当前时间："+sf.format(calendar.getTime()));
	//设置成6s前的时间，若当前时间为2016-12-28 00:00:06
	//那么设置之后时间变成2016-12-12 00:00:00
	calendar.add(Calendar.SECOND, -6);
	Timer timer=new Timer();
	//第一次执行时间为6s前，之后每隔2s秒执行一次
	timer.schedule(new TimerTask() {
	@Override
	public void run() {
		//打印当前的计划执行时间
		System.out.println("任务执行时间："+sf.format(scheduledExecutionTime()));
		System.out.println("任务正在执行！");
	}
	}, calendar.getTime(),2000);
}

运行结果：
当前时间：2018-11-01 19:02:32
任务执行时间：2018-11-01 19:02:32
任务正在执行！
任务执行时间：2018-11-01 19:02:34
任务正在执行！
{% endhighlight java %}

#### scheduleAtFixedRate方法
#### “fixed-rate”，如果第一次执行时间被delay了，随后的执行时间按照上一次开始的时间点进行计算，并且为了赶上进度会多次执行任务，因此TimerTask中的执行体需要考虑同步
{% highlight java %}
public static void main(String[] args) {
	//规定时间格式
	final SimpleDateFormat sf=new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
	//获取当前的具体时间
	Calendar calendar=Calendar.getInstance();
	System.out.println("当前时间："+sf.format(calendar.getTime()));
	//设置成6s前的时间，若当前时间为2016-12-28 00:00:06
	//那么设置之后时间变成2016-12-12 00:00:00
	calendar.add(Calendar.SECOND, -6);
	Timer timer=new Timer();
	//第一次执行时间为6s前，之后每隔2s秒执行一次
	timer.scheduleAtFixedRate(new TimerTask() {
		@Override
		public void run() {
			//打印当前的计划执行时间
			System.out.println("任务执行时间："+sf.format(scheduledExecutionTime()));
			System.out.println("任务正在执行！");
		}
	}, calendar.getTime(),2000);
}

运行结果：
当前时间：2018-11-01 19:08:15
任务执行时间：2018-11-01 19:08:09
任务正在执行！
任务执行时间：2018-11-01 19:08:11
任务正在执行！
任务执行时间：2018-11-01 19:08:13
任务正在执行！
任务执行时间：2018-11-01 19:08:15
任务正在执行！
任务执行时间：2018-11-01 19:08:17
{% endhighlight java %}

#### ·任务执行时间超出执行周期间隔
#### schedule方法
#### 下一次执行时间相对于上一次实际执行完成的时间点，因此执行时间会不断延后
{% highlight java %}
public static void main(String[] args) {
	//规定时间格式
	final SimpleDateFormat sf=new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
	//获取当前的具体时间
	Calendar calendar=Calendar.getInstance();
	System.out.println("当前时间："+sf.format(calendar.getTime()));
	Timer timer=new Timer();
	timer.schedule(new TimerTask() {
	@Override
	public void run() {
		try {
		Thread.sleep(3000);
		} catch (InterruptedException e) {
		// TODO Auto-generated catch block
		e.printStackTrace();
		}
		//打印当前的计划执行时间
		System.out.println("任务执行时间："+sf.format(scheduledExecutionTime()));
		System.out.println("任务正在执行！");
	}
	}, calendar.getTime(),2000);
}

运行结果：
当前时间：2018-11-01 19:14:35
任务执行时间：2018-11-01 19:14:35
任务正在执行！
任务执行时间：2018-11-01 19:14:38
任务正在执行！
{% endhighlight java %}

#### scheduleAtFixedRate方法
#### 下一次执行时间相对于上一次开始的时间点，因此执行时间一般不会延后，因此存在并发性
{% highlight java %}
public static void main(String[] args) {
//规定时间格式
final SimpleDateFormat sf=new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
//获取当前的具体时间
Calendar calendar=Calendar.getInstance();
System.out.println("当前时间："+sf.format(calendar.getTime()));
Timer timer=new Timer();
timer.scheduleAtFixedRate(new TimerTask() {
	@Override
	public void run() {
	try {
		Thread.sleep(3000);
	} catch (InterruptedException e) {
		// TODO Auto-generated catch block
		e.printStackTrace();
	}
	//打印当前的计划执行时间
	System.out.println("任务执行时间："+sf.format(scheduledExecutionTime()));
	System.out.println("任务正在执行！");
	}
}, calendar.getTime(),2000);
}

运行结果：
当前时间：2018-11-01 19:17:20
任务执行时间：2018-11-01 19:17:20
任务正在执行！
任务执行时间：2018-11-01 19:17:22
任务正在执行！
{% endhighlight java %}

### 2.10模拟两个机器人的定时行为
#### 第一个机器人会隔两秒打印最近一次计划的时间、执行内容
#### 第二个机器人会模拟往桶里倒水，直到桶里的水满为止
#### 第一个机器人
{% highlight java %}
public class DancingRobot extends TimerTask {
	@Override
	public void run() {
		// 获取最近的一次任务执行的时间，并将其格式化
		SimpleDateFormat sf=new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
		System.out.println("任务执行时间："+sf.format(scheduledExecutionTime()));
		System.out.println("跳舞中ing");
	}
}
{% endhighlight java %}

#### 第二个机器人
{% highlight java %}
public class WaterRobot extends TimerTask{
	private Timer timer;
	//最大容量为5L
	private Integer bucketCapacity=0;
	public WaterRobot(Timer timer) {
		this.timer = timer;
	}
	@Override
	public void run() {
		//灌水直至桶满为止
		if(bucketCapacity<5) {
			System.out.println("增加1L水到水桶中！");
			bucketCapacity++;
		}else {
			//水满之后停止执行
			System.out.println("取消的任务数："+timer.purge());
			cancel();
			System.out.println("灌水机器人停止运作！");
			System.out.println("取消的任务数："+timer.purge());
			System.out.println("当前水容量："+bucketCapacity+"L");
			//等待2s终止timer里的所有内容
			try {
				Thread.sleep(2000);
			} catch (InterruptedException e) {
				e.printStackTrace();
			}
			timer.cancel();
		}
	}
}
{% endhighlight java %}


#### 执行类
{% highlight java %}
public class Executor {
	public static void main(String[] args) {
		Timer timer=new Timer();
		//获取当前时间
		Calendar calendar=Calendar.getInstance();
		SimpleDateFormat sf=new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
		System.out.println("当前时间："+sf.format(calendar.getTime()));
		DancingRobot dr=new DancingRobot();
		WaterRobot wr=new WaterRobot(timer);
		timer.schedule(dr, calendar.getTime(),2000);
		timer.scheduleAtFixedRate(wr, calendar.getTime(), 1000);
	}
}
{% endhighlight java %}

## 3.Timer的缺陷
#### 管理并发任务的缺陷：Timer有且仅有一个线程去执行定时任务，如果存在多个任务，且任务时间过长，会导致执行效果与预期不符
#### 当任务抛出异常时的缺陷：如果timerTask抛出RuntimeException,Timer会停止所有任务的运行
#### ·管理并发任务的缺陷
#### MyTimerTask2.java
{% highlight java %}
public class MyTimerTask2 extends TimerTask{
	private String name;
	private long costTime;
	public MyTimerTask2(String name, long costTime) {
		this.name = name;
		this.costTime = costTime;
	}
	@Override
	public void run() {
		//以yyyy-MM-dd HH:mm:ss的格式打印当前执行时间
		//如2018-12-12 12：12：12
		Calendar calendar=Calendar.getInstance();
		SimpleDateFormat sf=new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
		System.out.println(name+"当前执行时间："+sf.format(calendar.getTime()));
		try {
			Thread.sleep(costTime);
		} catch (InterruptedException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		//获取costTime之后的时间
		calendar=Calendar.getInstance();
		System.out.println(name+"任务完成时间："+sf.format(calendar.getTime()));

	}
	public String getName() {
		return name;
	}
	public long getCostTime() {
		return costTime;
	}
}
{% endhighlight java %}
#### MyTimer2.java创建两个任务并发执行
{% highlight java %}
public class MyTimer2 {
	public static void main(String[] args) {
		// 创建一个timer实例
		Timer timer=new Timer();
		//创建一个MyTimerTask实例
		MyTimerTask2 myTimerTask1=new MyTimerTask2("No.1",2000);
		MyTimerTask2 myTimerTask2=new MyTimerTask2("No.2",2000);
		/**
			* 获取当前时间，并设置成距离当前时间3s之后的时间
			*/
		Calendar calendar=Calendar.getInstance();
		SimpleDateFormat sf=new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
		System.out.println("当前时间:"+sf.format(calendar.getTime()));
		timer.schedule(myTimerTask1, calendar.getTime());
		timer.schedule(myTimerTask2, calendar.getTime());
	}
}
{% endhighlight java %}

#### 运行程序，查看控制台输出结果得知，两个任务并不是并发执行的。
{% highlight java %}
	当前时间:2018-11-02 19:07:29
	No.1当前执行时间：2018-11-02 19:07:29
	No.1任务完成时间：2018-11-02 19:07:31
	No.2当前执行时间：2018-11-02 19:07:31
	No.2任务完成时间：2018-11-02 19:07:33
{% endhighlight java %}
## 4.Timer的使用禁区
#### ·对时效性要求较高的多任务并发作业
#### ·对复杂的任务的调度
