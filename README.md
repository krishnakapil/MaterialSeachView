# MaterialSeachView
Android Material Design Style Custom Search View

![Screenshot 1]
(https://github.com/krishnakapil/MaterialSeachView/blob/master/material-search.png)    ![Screenshot 2]
(https://github.com/krishnakapil/MaterialSeachView/blob/master/material-search2.png)

#Usage
Copy the material-search.aar file into your project libs folder.
Add the following dependency into gradle.
```gradle
repositories {
        flatDir {
            dirs 'libs'
        }
}


dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile(name:'material-search', ext:'aar')
}
```

Add MaterialSearchView to your layout file

```xml
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <android.support.v7.widget.Toolbar
        android:id="@+id/toolbar"
        android:layout_width="match_parent"
        android:layout_height="?attr/actionBarSize"
        android:background="@android:color/darker_gray"
        android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"
        app:popupTheme="@style/ThemeOverlay.AppCompat.Light" />

    <com.search.material.library.MaterialSearchView
        android:id="@+id/search_view"
        style="@style/MaterialSearchViewStyle"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />

</RelativeLayout>
```
Get reference to it in the Activity and set adapter. Adapter should implement Filterable.

(Check MaterialSeachView/app/src/main/java/com/search/material/materialsearch)

```java
@Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        ....
        MaterialSearchView searchView = (MaterialSearchView) findViewById(R.id.search_view);
        
        SearchAdapter adapter = new SearchAdapter();
        searchView.setAdapter(adapter);
        
        searchView.setOnQueryTextListener(new MaterialSearchView.OnQueryTextListener() {
            @Override
            public boolean onQueryTextSubmit(String query) {
                return false;
            }

            @Override
            public boolean onQueryTextChange(String newText) {
                return false;
            }
        });

        searchView.setOnSearchViewListener(new MaterialSearchView.SearchViewListener() {
            @Override
            public void onSearchViewShown() {

            }

            @Override
            public void onSearchViewClosed() {

            }
        });

        searchView.setOnItemClickListener(new AdapterView.OnItemClickListener() {
            @Override
            public void onItemClick(AdapterView<?> parent, View view, int position, long id) {

            }
        });
        
    }
    
    @Override
    public boolean onCreateOptionsMenu(Menu menu) {
        // Inflate the menu; this adds items to the action bar if it is present.
        getMenuInflater().inflate(R.menu.menu_main, menu);

        MenuItem item = menu.findItem(R.id.action_search);
        searchView.setMenuItem(item);

        return true;
    }
```
Handle Voice search

```java
@Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        if (requestCode == MaterialSearchView.REQUEST_VOICE && resultCode == RESULT_OK) {
            ArrayList<String> matches = data.getStringArrayListExtra(RecognizerIntent.EXTRA_RESULTS);
            if (matches != null && matches.size() > 0) {
                String searchWrd = matches.get(0);
                if (!TextUtils.isEmpty(searchWrd)) {
                    searchView.setQuery(searchWrd, false);
                }
            }

            return;
        }
        super.onActivityResult(requestCode, resultCode, data);
    }
```

Closing Search when back pressed
```java
@Override
    public void onBackPressed() {
        if (searchView.isSearchOpen()) {
            searchView.closeSearch();
        } else {
            super.onBackPressed();
        }
    }
```

#Styling Material Search View
```xml
<style name="MaterialSearchViewStyle">
        <!-- Background for the search bar-->
        <item name="searchBackground">@android:color/white</item>
        
        <!-- Change voice icon-->
        <item name="searchVoiceIcon">@drawable/ic_action_voice_search</item>
        
        <!-- Change clear text icon-->
        <item name="searchCloseIcon">@drawable/ic_action_navigation_close</item>
        
        <!-- Change up icon-->
        <item name="searchBackIcon">@drawable/ic_action_navigation_arrow_back</item>
        
        <!-- Change background for the suggestions list view-->
        <item name="searchSuggestionBackground">...</item>
        
        <!-- Change text color for edit text. This will also be the color of the cursor-->
        <item name="android:textColor">@android:color/black</item>
        
        <!-- Change hint text color for edit text-->
        <item name="android:textColorHint">@android:color/black</item>
        
        <!-- Hint for edit text-->
        <item name="android:hint">@string/search_hint</item>
 </style>
```
