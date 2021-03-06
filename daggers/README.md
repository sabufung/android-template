Daggers
=======

![Logo](../logo.png)

Dagger2 utility, help to setup Dagger2 easier. 

 * Eliminate boilerplate code to create sub-components
 * Eliminate boilerplate code to find component to involve inject

```java

public class SampleApp extends Application {

  @Override public void onCreate() {
    super.onCreate();
    Daggers.configure(this);
    Daggers.openAppScope(DaggerAppComponent.builder()
        .appModule(new AppModule(this))
        .build());
  }

}

class ExampleActivity extends Activity implements HasScope {
  
  @Inject UserModel userModel;
  
  @Override protected void onCreate(@Nullable Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    Daggers.inject(this);
    // TODO use userModel
  }


  @Override
  public Object module() {
    return new ActivityModule();
  }

}

public class ExampleFragment extends Fragment {
  @Inject UserModel userModel;

  @Override public void onCreate(@Nullable Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    Daggers.inject(this);
    // TODO use userModel
  }
}

public class ExampleViewGroup extends FrameLayout {
  @Inject ExamplePresenter presenter; // provided by AppModule
  
  public ExampleViewGroup(Context context, AttributeSet attrs) {
    super(context, attrs);
    Daggers.injectAppDependencies(this);
    // TODO use presenter
  }
}

```

**Remember**

 * `Daggers.injectAppDependencies(this)` // injected by `AppComponent`
 * `Daggers.inject(this)` // injected by `ActivityComponent`

For example and additional information see [sample](../sample).

Download
--------

```groovy
allprojects {
  repositories {
	 ...
    maven { url 'https://jitpack.io' }
  }
}

dependencies {
  compile 'com.github.tikivn.android-template:daggers:-SNAPSHOT'
}
```

Proguard
--------
```proguard
-keep,allowobfuscation @interface dagger.Component
-keep,allowobfuscation @interface dagger.Subcomponent
-keepclassmembers @dagger.Component public class * {
 *** plus(***);
 void inject(***);
}
-keepclassmembers @dagger.Subcomponent public class * {
 void inject(***);
}
```

License
-------

    Copyright 2017 Tiki Corp

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.
