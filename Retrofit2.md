---
title: Retrofit2
date: 2016-08-14 14:25:19
tags: [Retrofit2]
---
#Retrofit2 
使用Http请求与服务器通信，上传或下载数据等
Volley、AsyncHttp、OkHttp
基于OkHttp的RESTFUL Api请求工具
相对于Volley来说，Volley更符合正常的逻辑思维

1. 创建retrofit对象，并指定域名
public static final String API_URL = "";
Retrofit retrofit = new Retrofit.Builder().baseUrl(API_URL).addConverterFactory(GsonConverFactory.create()).build();

2.新建一个api接口，使用注解描述这个api
public interface ZhuanLanApi {
    @GET("/api/columns/{user} ")
    Call<ZhuanLanAuthor> getAuthor(@Path("user") String user)
}
<!--more-->
3.用这个retrofit对象创建一个Api对象，直接调用方法，就能表示出请求的api
ZhuanLanApi api = retrofit.create(ZhuanLanApi.class);
Call<ZhuanLanAuthor> call = api.getAuthor("qinchao");
样就表示你要请求的api是https://zhuanlan.zhihu.com/api/columns/qinchao

4.使用该对象获取数据，调用enqueue方法异步发送http请求，或者调用execute方法发送同步请求。
// 请求数据，并且处理response
call.enqueue(new Callback<ZhuanLanAuthor>() {
    @Override
    public void onResponse(Response<ZhuanLanAuthor> author) {
    }
    @Override
    public void onFailure(Throwable t) {
    }
});

====创建一个接口来描述Http请求=====


=====基于 Retrofit + RxJava 做的一些巧妙的封装======

===实体类基类
public class BaseModel<T> implements Serializable {
    public String code;
    public String msg;
    public T data;
    /**
     * 默认1 为请求成功的有效数据
     * @return 请求成功
     */
    public boolean success(){
        return code.equals("1");
    
    @Override
    public String toString() {
        return "BaseModel={"+"code=["+code+"] msg=["+msg+" data=["+data+"}";
    }
}


=====Retrofit请求接口的ApiService
public interface ApiService{
	@FormUrlEncodeed
	@POST("/v1/login?c=login&i=emlogin")
	Observable<BaseModel<LoginData>> login(
		@Field("phone") String phone,
		@Field("password") String password
	);
}

====正确就做对应更新UI或者其他后续操作(该操作待封装)
Api.getDefault().login(phone,password)
	.subscribeOn(Scheduler.io())
	.obserceOn(AndroidSchedulers.mainThread())
	.subscribe(new XXX<BaseModel<LoginData>>(){
		@Override
		public void call(BaseModel<LoginData> data){
			if(data.code.equals("1")){
				//TODO success
			}else{
				//TODO false
			}
		}
	});

==== 对结果预处理,使用了一个Transformer对象对Observable进行转换，
//使用flatMap操作符把Obserable<BaseModel<T>>,转换成为Observable<T>
// 如果成功了，就返回一个Observable<T>，把具体的数据发射给订阅者。
// 如果失败了我们就使用Observable.error()发送一个异常的可观察者。
// 做了线程的切换,方便外部的调用
public static <T> Observable.Transformer<BaseModel<T>,T> handleResult(){
	return new Observable.Transformer<BaseModel<T>,T>(){
		@Override
		public Observable<T> call(Observable<BaseModel<T>> tObservable){
			return tObservable.flatMap(new Func1<BaseModel<T>,Observable<T>>(){
				@Override
				public Observable<T> call (BaseModel<T> result){
					Track.i("result from network:"+result);
					if(result.success()){
						return createData(result.data); 
					}else{
						return Observable.error(new ServerException(result.msg));
					}
				}
			}).subscribeOn(Schedulers.io()).obserceOn(AndroidSchedulers.mainThread());
		}
	};//end return
}

//成功数据
private static <T> Observable<T> createData(T data){
	return Observable.create(new Observable.OnSubscribe<T>(){
		@Override
		public void call(Subscriber<? super T> subscriber){
			try{
				subscriber.onNext(data);
				subscriber.onCompelted();
			}catch(Exception e){
				subscriber.onError(e);
			}
		}
	});
}

===第一次根据封装
Api.getDefault().login(phone,password)
	.compost(RxHelper.handleResult())
	.subscribe(new Action1<LoginData>(){
		@Override
		public void call(LoginData data){

		}
	});

	//封装了线程处理，该订阅只需要关心成功的数据，并和baseModel无关，只和泛型有关

//封装错误返回数据的Subscriber
public abstract class RxSubscribe<T> extends Subscriber<T>{
	@Override
	public void onNext(T t){
		_onNext(t);
	}
	@Override
	public void onError(Throwable e){
		e.printStackTrace();
		if(TDevice.getNetworkType()==0){
			_onError("网络不可用");
		}else if(e instanceof ServerException){
			_onError(e.getMessage());
		}else {
			_onError("请求失败，请稍后再试");
		}
	}

	protected abstract void _onNext(T t);
	protected abstract void _onError(String message);
}

== 最终封装效果

Api.getDefault().login(phone,password)
	.compost(RxHelper.handleResult())
	.subscribe(new RxSubscribe<LoginData>(){
		@Override
		protected void _onNext(LoginData data){
			//TODO success
		}
		@Override
		protected void _onError(String message){
			//TODO failed
		}
	});


==Subscriber扩展封装，需要显示一个Dialog
public abstract class RxSubscribe<T> extends Subscriber<T>{
	private Context mContext;
	private SweetAlertDialog dialog;
	private String msg;
	public RxSubscribe(Context context){
		this(context,"请稍后");
	}
	public RxSubscribe(Context context,String msg){
		this,mContext = context;
		this.msg=msg;
	}
	protected boolean showDialog(){
		return true;
	}
	@Override
	public void onCompelted(){
		if (showDialog()) {
			dialog.dismiss();
		}
	}
	@Override
	public void onStart(){
		super.onStart();
		if(showDialog()){
			dialog = new SweetAlertDialog(mContext,SweetAlertDialog.PROGERSS_TYPE)
			.setTitleText(msg);
			dialog.setCancelable(true);
			dialog.setCacelOnTouchOutside(true);
			dialog.setOnCancelListener(new DialogInterface.setOnCancelListener(){
				@Override
				public void onCancel(DialogInterface dialog){
					if(!isUnsubscribed()){
						unsubscribe();
					}
				}
			});
			dialog.show();
		}
	}
	@Override
	public void onNext(T t){
		_onNext(t);
	}
	@Override
	public void onError(Throwable e){
		e.printStackTrace();
		if(TDevice.getNetworkType()==0){
			_onError("网络不可用");
		}else if(e instanceof ServerException){
			_onError(e.getMessage());
		}else {
			_onError("请求失败，请稍后再试");
		}
		if(showDialog()){
			dialog.dismiss();
		}
	}

	protected abstract void _onNext(T t);
	protected abstract void _onError(String message);
}


//处理服务器数据的缓存，封装Observable
//从缓存中获取一个Observable<T>对象fromCache，如果获取不到的话就不发射数据
// 在fromNetwork那里用了一个map函数，里面只是做了把网络取回来的数据保存到缓存中。
public static <T> Observable<T> load(Context context,final String cacheKey,final long expireTime,Observable<T> fromNetwork, boolean froceRefresh){
	Observable<T> fromCache = Observable.create(new Observable.OnSubscribe<T>(){
		@Override
		public void call(Subscriber<? super T> subscriber){
			T cache = (T) CacheManager.readObject(context,cacheKey,expireTime);
			if(cache!=null){
				subscriber.onNext(cache);
			}else{
				subscriber.onCompelted();
			}
		}

	}).subscribeOn(Schedulers.io()).obserceOn(AndroidSchedulers.mainThread());
	fromNetwork = fromNetwork,map(new Func1<T,T>(){
		@Override
		public T call(T result){
			CacheManager.saveObject(context,(Serializable)result,cacheKey);
			return result;
		}
	});
	if(froceRefresh){
		return fromNetwork;
	}else{
		return Observable.concat(fromCache,fromNetwork).frist();
	}
}

===最终封装
Observable<LoginData> fromNetwork = Api.getDefault().login(phone,password)
	.compost(RxHelper.handleResult());
	RetrofitCache.load(context,cacheKey,100000L,fromNetwork,false)
	.subscribe(new RxSubscribe<LoginData>(context,"登录中"){
		@Override
		protected abstract void _onNext(T t){

		}
		@Override
		protected abstract void _onError(String message){

		}
	});