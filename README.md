# android-ListView
##  簡易 ListView定義
### 一個ListView通常有兩個職責   
* 將資料填充到佈局。
* 處理使用者的選擇點選等操作。
### 一個ListView的建立需要3個元素。
* ListView中的每一列的View。
* 填入View的資料或者圖片等。
* 連線資料與ListView的介面卡。    
簡易來說，要使用ListView，首先要了解什麼是介面卡。介面卡是一個連線資料和AdapterView（ListView就是一個典型的AdapterView）的橋樑，通過它能有效地實現資料與AdapterView的分離設定，使AdapterView與資料的繫結更加簡便，修改更加方便

##  建立一個字定義的 ListView
### 在佈局檔案中加入一個ListView控制元件
簡易新建一個 xml，並新增 listView元件。   
```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">
    
    <ListView
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:id="@+id/listView" />
        
</LinearLayout>
```
### 建立新的Adapter xml依靠
視圖設計，試圖設計後，在ListView上即可相同試圖設計在付值。   
```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="100dp"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:gravity="center"
    android:orientation="horizontal">
    
    <View
        android:layout_width="8dp"
        android:layout_height="0dp"/>
        
    <ImageView
        app:srcCompat="@drawable/camera"
        android:id="@+id/item_camera"
        android:layout_width="80dp"
        android:layout_height="80dp"/>
        
    <LinearLayout
        android:layout_width="290dp"
        android:layout_height="100dp"
        android:orientation="vertical"
        android:gravity="center">
        
        <LinearLayout
            android:layout_width="290dp"
            android:layout_height="48dp"
            android:gravity="left"
            android:orientation="horizontal">
            
            <View
                android:layout_width="15dp"
                android:layout_height="0dp"/>
                
            <TextView
                android:layout_width="200dp"
                android:layout_height="40dp"
                android:id="@+id/item_name"
                android:text="TEST"
                android:textSize="23sp"
                android:maxLength="20"/>

        </LinearLayout>

        <LinearLayout
            android:layout_width="290dp"
            android:layout_height="48dp"
            android:gravity="center">

            <View
                android:layout_width="15dp"
                android:layout_height="0dp"/>

            <ImageView
                android:id="@+id/item_basketball"
                android:layout_width="40dp"
                android:layout_height="40dp"
                app:srcCompat="@drawable/basketball" />

            <View
                android:layout_width="15dp"
                android:layout_height="0dp"/>


            <ImageView
                android:id="@+id/item_circket"
                android:layout_width="40dp"
                android:layout_height="40dp"
                android:background="@drawable/circket" />

            <View
                android:layout_width="15dp"
                android:layout_height="0dp"/>

            <ImageView
                android:layout_width="40dp"
                android:layout_height="40dp"
                android:id="@+id/item_esport"
                android:background="@drawable/esport"/>

            <View
                android:layout_width="15dp"
                android:layout_height="0dp"/>

            <ImageView
                android:layout_width="40dp"
                android:layout_height="40dp"
                android:id="@+id/item_football"
                android:background="@drawable/football"/>

            <View
                android:layout_width="15dp"
                android:layout_height="0dp"/>
        </LinearLayout>
    </LinearLayout>
</LinearLayout>
```


### 在Activity中初始化並新建點擊功能
在新創建的MainActivity中，定義並初始畫其功能    
```
//ListView
private ListView listView ;
private List<MainList> mainLists = null ;
private MainAdapter mainAdapter ;
```
並在onCreate中初始化    
```
/**
*  listView 創建列表
*  mainList 數據源放入List中
*  mainAdapter
*/
listView  = (ListView)findViewById(R.id.listView);
mainLists = new ArrayList<MainList>();
getListViewData();
// 適配器 建立一個自定義
mainAdapter = new MainAdapter(this);
listView.setAdapter(mainAdapter);
listView.setOnItemClickListener(itemClickListener);
```

##  字定義適配器Adapter
為了提供我們自己喜歡的適配器並付值
```
    public class MainAdapter extends BaseAdapter {

        private Context context ;
        private LayoutInflater inflater ;

        public MainAdapter(Context context){
            this.context = context;
            inflater = LayoutInflater.from(context);
        }

        public void refresh(List<MainList> lists){
            mainLists = lists;
            notifyDataSetChanged();
        }

        @Override
        public int getCount() {
            return mainLists == null ? 0 : mainLists.size();
        }

        @Override
        public Object getItem(int i) {
            return null;
        }

        @Override
        public long getItemId(int i) {
            return 0;
        }

        @Override
        public View getView(int position, View view, ViewGroup viewGroup) {
            MainList mainList = mainLists.get(position);
            ViewHolder viewHolder = null ;
            if(view == null){
                viewHolder = new ViewHolder();
                view = inflater.inflate(R.layout.main_items,null);
                viewHolder.textName         = (TextView) view.findViewById(R.id.item_name);
                viewHolder.item_camera      = (ImageView)view.findViewById(R.id.item_camera);
                viewHolder.item_basketball  = (ImageView)view.findViewById(R.id.item_basketball);
                viewHolder.item_circket     = (ImageView)view.findViewById(R.id.item_circket);
                viewHolder.item_esport      = (ImageView)view.findViewById(R.id.item_esport);
                viewHolder.item_football    = (ImageView)view.findViewById(R.id.item_football);
                view.setTag(viewHolder);
            } else {
                viewHolder = (ViewHolder)view.getTag();
            }

            /**
             * 設值
             */
            viewHolder.textName.setText(mainList.getTextName());
            viewHolder.item_camera.setImageBitmap(mainList.getImagePhoto());

            return view;
        }
    }
```

### 在Adapter內確定每列相片點擊列與事件

在value內建立ids.xml，與內文 name = "position" type="id"        
```
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <item name="position" type="id" />
</resources>
```

創好後，在原本的Adapter適配器中加入以下code
```
/**
* @幫每一個按鈕加上標示確定是第幾列
*/
viewHolder.textName         .setTag(R.id.position,position);
viewHolder.item_camera      .setTag(R.id.position,position);
viewHolder.item_basketball  .setTag(R.id.position,position);
viewHolder.item_circket     .setTag(R.id.position,position);
viewHolder.item_esport      .setTag(R.id.position,position);
viewHolder.item_football    .setTag(R.id.position,position);
```

賦予每列點擊事件

新增每個items點擊功能       
```
/**
* @幫每一個按鈕新增監聽
*/
viewHolder.textName         .setOnClickListener(listener);
viewHolder.item_camera      .setOnClickListener(listener);
viewHolder.item_basketball  .setOnClickListener(listener);
viewHolder.item_circket     .setOnClickListener(listener);
viewHolder.item_esport      .setOnClickListener(listener);
viewHolder.item_football    .setOnClickListener(listener);
```

點擊功能，取得R.id.items...點擊事件        
```
View.OnClickListener listener = view -> {

        int position = (int)view.getTag(R.id.position);
        application.mainList = (MainList)mainLists.get(position);
        String item_name = application.mainList.getTextName();

        switch (view.getId()){
            case R.id.item_name:
                showAlertUpdaing(item_name,"click_name",position);
                break;
            case R.id.item_basketball:
                showAlertUpdaing(item_name,"click_basketball",position);
                break;

            case R.id.item_camera:
                showAlertUpdaing(item_name,"click_camera",position);
                break;

            case R.id.item_circket:
                showAlertUpdaing(item_name,"click_circket",position);
                break;

            case R.id.item_esport:
                showAlertUpdaing(item_name,"click_esport",position);
                break;

            case R.id.item_football:
                showAlertUpdaing(item_name,"click_football",position);
                break;
        }
    };
```


