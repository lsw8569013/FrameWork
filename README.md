# FrameWork


android MVC框架 核心思想
	
	BaseActivity 的封装
	
	在onCreate 封装三个抽象的方法 findViewById();	setListener();	processLogic(); 
	分别用于查找view,设置监听，和处理一些界面初始就加载数据；
	
	在onCreate方法中，封住一个initView()，公共方法，用于处理界面公共一下代码；比如，界面顶部界面的加载，和返回按钮的处理
	
	定义一个回调函数  abstract interface DataCallback<T> 由回调方法 来处理服务器返回的数据。
	
	定义一个子线程BaseTask ，发送请求，返回的数据通过handler,发送给主线程，来处理，并把这个线程添加到线程池中。
	
	BaseHandler 继承 handler，获取到baseTask ,发送过来的数据，调用 DataCallback的 回调方法processData，来处理数据。
	
	protected void getDataFromServer（RequestVo reqVo,  DataCallback callBack){
		BaseHandler handler = new BaseHandler(this, callBack, reqVo);// handle类数据得到后使用
			BaseTask taskThread = new BaseTask(this, reqVo, handler);// 线程类--获取数据
			this.threadPoolManager.addTask(taskThread);
	}
	
	RequestVo 包含请求链接 ，context,msg，请求Map,解析器
	
	
	在子类activity 中，只要继承 BaseActivity，在这几个 findViewById();	setListener();	processLogic();进行一些开始UI界面的初始化。
	
	访问服务器的时候，只要调用 getDataFromServer ，把请求数据，解析器，填进行就可以了.数据处理在回调方法中处理。
