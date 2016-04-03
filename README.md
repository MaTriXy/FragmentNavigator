# FragmentNavigator
An useful fragment navigator helps you control fragments. Support fragments in activity and nested fragments.

### Demo
[Download](https://github.com/Aspsine/FragmentNavigator/blob/master/art/demo.apk?raw=true)

### How to use
The lib provide `FragmentNavigator` class and `FragmentNavigatorAdapter` interface to help you control fragments. It's very simple to use.

- MainActivity.java
```java
public class MainActivity extends AppCompatActivity implements BottomNavigatorView.OnBottomNavigatorViewItemClickListener {

    private static final int DEFAULT_POSITION = 0;

    private FragmentNavigator mNavigator;

    // a simple custom bottom navigation view
    private BottomNavigatorView bottomNavigatorView;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        mNavigator = new FragmentNavigator(getSupportFragmentManager(), new FragmentAdapter(), R.id.container);
        // set default tab position
        mNavigator.setDefaultPosition(DEFAULT_POSITION);
        mNavigator.onCreate(savedInstanceState);

        bottomNavigatorView = (BottomNavigatorView) findViewById(R.id.bottomNavigatorView);
        bottomNavigatorView.setOnBottomNavigatorViewItemClickListener(this);

        setCurrentTab(mNavigator.getCurrentPosition());
    }

    @Override
    protected void onSaveInstanceState(Bundle outState) {
        super.onSaveInstanceState(outState);
        mNavigator.onSaveInstanceState(outState);
    }

    @Override
    public void onBottomNavigatorViewItemClick(int position, View view) {
        setCurrentTab(position);
    }

    private void setCurrentTab(int position) {
        mNavigator.showFragment(position);
        bottomNavigatorView.select(position);
    }

    @Override
    protected void onRestoreInstanceState(Bundle savedInstanceState) {
        super.onRestoreInstanceState(savedInstanceState);
    }
}
```

- FragmentAdapter.java
```java
public class FragmentAdapter implements FragmentNavigatorAdapter {

    private static final String TABS[] = {"Chats", "Contacts", "Circle", "Me"};

    @Override
    public Fragment onCreateFragment(int position) {
        return MainFragment.newInstance(TABS[position]);
    }

    @Override
    public String getTag(int position) {
        // an simple unique tag
        return MainFragment.TAG + TABS[position];
    }

    @Override
    public int getCount() {
        return TABS.length;
    }
}
```

### Usage of Demo App:
- Step 1: Install demo app and open it, default tab is 'Chats'.
![](https://github.com/Aspsine/FragmentNavigator/blob/master/art/screenshot0.png?raw=true)

- Step 2: Switch bottom tab to 'Contacts', Contacts page contains two child fragments.
![](https://github.com/Aspsine/FragmentNavigator/blob/master/art/screenshot1.png?raw=true)

- Step 3: Switch top tab to 'Groups'
![](https://github.com/Aspsine/FragmentNavigator/blob/master/art/screenshot2.png?raw=true)

- Step 4: Switch bottom tab to 'Circle'
![](https://github.com/Aspsine/FragmentNavigator/blob/master/art/screenshot3.png?raw=true)

- Step 5: Click 'EXCEPTION' menu button on the right of actionbar
![](https://github.com/Aspsine/FragmentNavigator/blob/master/art/screenshot4.png?raw=true)

- Step 6: Click 'CLICK ME TO MAKE AN EXCEPTION' button crash the app to demostate a RuntimeException
![](https://github.com/Aspsine/FragmentNavigator/blob/master/art/screenshot5.png?raw=true)

- Step 7: We can see a crash dialog showed after app crashed. Then we click 'OK' button.(Some device may not have the crash alert dialog)  
Wow! App restarted, Fragment and bottom tab is properly restored it's state.
![](https://github.com/Aspsine/FragmentNavigator/blob/master/art/screenshot6.png?raw=true)

- Step 8: Let's switch to 'Contacts' to see whether child fragment is properly restored. Good, it's restored and the state is right.
![](https://github.com/Aspsine/FragmentNavigator/blob/master/art/screenshot7.png?raw=true)