---
title: fragment特性和传参用法
date: 2017-06-05 17:10:45
tags:
categories:
---


android 3.0中加入了fragment特性，兼容包中也提供了相应的支持，让开发者编写和管理用户界面更快捷方便。
官方推荐使用：fragment使用Fragment.setArguments(Bundle bundle)传递参数，而不适用构造方法传递参数。
public class FramentTestActivity extends ActionBarActivity {
	
	@Override
	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_main);

		if (savedInstanceState == null) {
			getSupportFragmentManager().beginTransaction()
					.add(R.id.container, new TestFragment("param")).commit();
		}
		
	}

	public static class TestFragment extends Fragment {

		private String mArg = "没有参数";
		
		public TestFragment() {
			Log.i("INFO", "TestFragment non-parameter constructor");
		}
		
		public TestFragment(String arg){
			mArg = arg;
			Log.i("INFO", "TestFragment construct with parameter");
		}

		@Override
		public View onCreateView(LayoutInflater inflater, ViewGroup container,
				Bundle savedInstanceState) {
			View rootView = inflater.inflate(R.layout.fragment_main, container,
					false);
			TextView tv = (TextView) rootView.findViewById(R.id.tv);
			tv.setText(mArg);
			return rootView;
		}
	}

}

可以看到我们传递过来的数据正确的显示了，如果设备配置参数发生变化，例如横竖屏切换就显示为“没有参数”
Fragment.setArguments(Bundle bundle)这种方式
public class FramentTest2Activity extends ActionBarActivity {
       
        @Override
        protected void onCreate(Bundle savedInstanceState) {
              super.onCreate(savedInstanceState);
             setContentView(R.layout. activity_main);

              if (savedInstanceState == null) {
                    getSupportFragmentManager().beginTransaction()
                                 .add(R.id. container, TestFragment.newInstance("param")).commit();
             }

       }

        public static class TestFragment extends Fragment {

              private static final String ARG = "arg";
             
              public TestFragment() {
                    Log. i("INFO", "TestFragment non-parameter constructor" );
             }

              public static Fragment newInstance(String arg){
                    TestFragment fragment = new TestFragment();
                    Bundle bundle = new Bundle();
                    bundle.putString( ARG, arg);
                    fragment.setArguments(bundle);
                     return fragment;
             }
             
              @Override
              public View onCreateView(LayoutInflater inflater, ViewGroup container,
                           Bundle savedInstanceState) {
                    View rootView = inflater.inflate(R.layout. fragment_main, container,
                                  false);
                    TextView tv = (TextView) rootView.findViewById(R.id. tv);
                    tv.setText(getArguments().getString( ARG));
                     return rootView;
             }
       }

}
上述这种传参方法始终能将参数正常传递并显示

横竖屏切换会重新走生命周期，activity会重新创建，依附于activity的fragment也会消失

activity的onCreate(Bundle savedInstanceState)源码
     protected void onCreate(Bundle savedInstanceState) {
        if (DEBUG_LIFECYCLE ) Slog.v( TAG, "onCreate " + this + ": " + savedInstanceState);
        if (mLastNonConfigurationInstances != null) {
            mAllLoaderManagers = mLastNonConfigurationInstances .loaders ;
        }
        if (mActivityInfo .parentActivityName != null) {
            if (mActionBar == null) {
                mEnableDefaultActionBarUp = true ;
            } else {
                mActionBar .setDefaultDisplayHomeAsUpEnabled( true);
            }
        }
        if (savedInstanceState != null) {
            Parcelable p = savedInstanceState.getParcelable( FRAGMENTS_TAG );
            mFragments .restoreAllState(p, mLastNonConfigurationInstances != null
                    ? mLastNonConfigurationInstances .fragments : null);
        }
        mFragments .dispatchCreate();
        getApplication().dispatchActivityCreated( this , savedInstanceState);
        mCalled = true ;
    }


fragment是由fragmentManager管理
所以跟进FragmentManager.restoreAllState()

  for (int i=0; i<fms.mActive.length; i++) {
           FragmentState fs = fms.mActive[i];
           if (fs != null) {
              Fragment f = fs.instantiate(mActivity, mParent);
               if (DEBUG) Log.v(TAG, "restoreAllState: active #" + i + ": " + f);
               mActive.add(f);
               // Now that the fragment is instantiated (or came from being
               // retained above), clear mInstance in case we end up re-restoring
                // from this FragmentState again.
                fs.mInstance = null;
           } else {
               mActive.add(null);
                if (mAvailIndices == null) {
                    mAvailIndices = new ArrayList<Integer>();
               }
               if (DEBUG) Log.v(TAG, "restoreAllState: avail #" + i);
               mAvailIndices.add(i);
           }
}

跟进FragmentState.instantitate()

public Fragment instantiate(Activity activity, Fragment parent) {
        if (mInstance != null) {
            return mInstance ;
        }
       
        if (mArguments != null) {
            mArguments .setClassLoader(activity.getClassLoader());
        }
       
        mInstance = Fragment.instantiate(activity, mClassName , mArguments );
       
        if (mSavedFragmentState != null) {
            mSavedFragmentState .setClassLoader(activity.getClassLoader());
            mInstance .mSavedFragmentState = mSavedFragmentState ;
        }
        mInstance .setIndex(mIndex , parent);
        mInstance .mFromLayout = mFromLayout ;
        mInstance .mRestored = true;
        mInstance .mFragmentId = mFragmentId ;
        mInstance .mContainerId = mContainerId ;
        mInstance .mTag = mTag ;
        mInstance .mRetainInstance = mRetainInstance ;
        mInstance .mDetached = mDetached ;
        mInstance .mFragmentManager = activity.mFragments;
        if (FragmentManagerImpl.DEBUG) Log.v(FragmentManagerImpl.TAG,
                "Instantiated fragment " + mInstance );

        return mInstance ;
    }


    最终转入到Fragment.instantitate()
     public static Fragment instantiate(Context context, String fname, Bundle args) {
        try {
            Class<?> clazz = sClassMap .get(fname);
            if (clazz == null) {
                // Class not found in the cache, see if it's real, and try to add it
                clazz = context.getClassLoader().loadClass(fname);
                sClassMap .put(fname, clazz);
            }
            Fragment f = (Fragment)clazz.newInstance();
            if (args != null) {
                args.setClassLoader(f.getClass().getClassLoader());
                f. mArguments = args;
            }
            return f;
        } catch (ClassNotFoundException e) {
            throw new InstantiationException( "Unable to instantiate fragment " + fname
                    + ": make sure class name exists, is public, and has an"
                    + " empty constructor that is public" , e);
        } catch (java.lang.InstantiationException e) {
            throw new InstantiationException( "Unable to instantiate fragment " + fname
                    + ": make sure class name exists, is public, and has an"
                    + " empty constructor that is public" , e);
        } catch (IllegalAccessException e) {
            throw new InstantiationException( "Unable to instantiate fragment " + fname
                    + ": make sure class name exists, is public, and has an"
                    + " empty constructor that is public" , e);
        }
    }
通过此方法可以看到，最终通过反射无参构造实例化一个新的Fragment，并且给mArgments初始化为原先的值，而原来的Fragment实例的数据都丢失了，并重新进行了初始化
所以构造函数传递的参数就会丢失
Activity重新创建时，会重新构建它所管理的Fragment，原先的Fragment的字段值将会全部丢失，但是通过Fragment.setArguments(Bundle bundle)方法设置的bundle会保留下来。
所以尽量使用Fragment.setArguments(Bundle bundle)方式来传递参数

官方也推荐fragment使用setArguments(Bundle bundle)传递参数，而不用构造方法传递参数。