期中实验报告——NotePad                                                  
一.实验要求                                                                             
基本要求：                                                                                                                                                             
1.NoteList中显示条目增加时间戳显示                                                                                                                                 
2.添加笔记查询功能（根据标题查询）                                                                                                    
扩展要求：                                                                                                                                                                                                                               
附加功能：根据自身实际情况进行扩充（至少两项）                                                                                                             
二.基本功能                                                                                                                                                                                                                                     
1.NoteList中显示条目增加时间戳显示                                                                                          
关键代码：                                                                                                                                                                                 
在res—layout—notelist_item.xml布局文件中                         
<TextView                                     
    android:id="@android:id/text2"                           
    android:layout_width="match_parent"                                
    android:layout_height="wrap_content"                                      
    android:textAppearance="?android:attr/textAppearanceLarge"                            
    android:gravity="center_vertical"                         
    android:paddingLeft="5dp"                          
    android:singleLine="true" />                                       
效果展示：                                                                                                                                                                                          
![image](https://user-images.githubusercontent.com/90746476/143774249-23c5b0f6-f50d-4a8a-a385-40d5022fb139.png)                                                                           
2.添加笔记查询功能（根据标题查询）                                                                                         
 关键代码：                                                                                           
在res—values—strings.xml中添加menu_search字段 在res—menu—list_options_menu.xml布局文件中添加搜索功能，新增menu_search：在res—layout中新建一个查找笔记内容的布局文件note_search.xml：                                                                       
@Override                                                      
protected void onCreate(Bundle savedInstanceState) {                                               
    super.onCreate(savedInstanceState);                                                        
    setContentView(R.layout.note_search);                                                                
    SearchView searchView = findViewById(R.id.search_view);                                                            
    Intent intent = getIntent();                                                                
    if (intent.getData() == null) {                                                                     
        intent.setData(NotePad.Notes.CONTENT_URI);                                                                  
    }                                                                                   
    listView = findViewById(R.id.list_view);                                                               
    sqLiteDatabase = new NotePadProvider.DatabaseHelper(this).getReadableDatabase();                                                    
    //设置该SearchView显示搜索按钮                                                                   
    searchView.setSubmitButtonEnabled(true);                                                                    

    //设置该SearchView内默认显示的提示文本                                                                      
    searchView.setQueryHint("查找");                                                               
    searchView.setOnQueryTextListener(this);                                                                 

}                                                                             
public boolean onQueryTextChange(String string) {                                                                     
    String selection1 = NotePad.Notes.COLUMN_NAME_TITLE+" like ? or "+NotePad.Notes.COLUMN_NAME_NOTE+" like ?";                                  
    String[] selection2 = {"%"+string+"%","%"+string+"%"};                                                    
    Cursor cursor = sqLiteDatabase.query(                                                                                                                       
            NotePad.Notes.TABLE_NAME,                                                                   
            PROJECTION, // The columns to return from the query                                                                                                                  
            selection1, // The columns for the where clause                                                                                                                     
            selection2, // The values for the where clause                                                                                  
            null,          // don't group the rows                                                                                                                     
            null,          // don't filter by row groups                                                                                                      
            NotePad.Notes.DEFAULT_SORT_ORDER // The sort order                                                                                                            
    );                                                                                                                   
    // The names of the cursor columns to display in the view, initialized to the title column                                                                        
    String[] dataColumns = {                                                                                                                   
            NotePad.Notes.COLUMN_NAME_TITLE,                                    
            NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE                                 
    } ;                               
    // The view IDs that will display the cursor columns, initialized to the TextView in                               
    // noteslist_item.xml                                   
    int[] viewIDs = {                                   
            android.R.id.text1,                                 
            android.R.id.text2                                        
    };                                         
    // Creates the backing adapter for the ListView.                                       
    SimpleCursorAdapter adapter                                        
            = new SimpleCursorAdapter(                                               
            this,                             // The Context for the ListView                                         
            R.layout.noteslist_item,         // Points to the XML for a list item                                  
            cursor,                           // The cursor to get items from                                         
            dataColumns,                                          
            viewIDs                                                
    );                                                                  
    // Sets the ListView's adapter to be the cursor adapter that was just created.                                  
    listView.setAdapter(adapter);                                             
    return true;                                            
}                                                  
}                                                        
效果展示：                                                          
![image](https://user-images.githubusercontent.com/90746476/143774543-aac50cb9-50b3-4297-b95c-81b0083e2f30.png)                                                           
三、附加功能实现：                                    
1.实现ui美化                                     
关键代码：                                   
在菜单的布局文件（xml文件）中创建菜单栏                                                  
<item                               
        android:title="改变颜色">                                  
        <menu>                                       
            <item                                          
                android:title="改变背景颜色"                                          
                android:id="@+id/background-color">                           
            </item>                                
            <item android:id="@+id/text-color"                                  
                android:title="改变字体颜色">                              
            </item>                         
        </menu>                       
    </item>                                
  private void showColor(){                             
        AlertDialog alertDialog=new AlertDialog.Builder(this).setTitle("请选择颜色").                      
               setView(R.layout.color_layout)                                           
                    @Override                                 
                    public void onClick(DialogInterface dialog, int which) {                        
                        dialog.dismiss();                                 
                    }                            
                }).create();                        
        alertDialog.show();                          
    }                                  
 public void onClick(View v) {                      
        switch (v.getId()){                       
            case R.id.aqua:                          
                if(isFlag){                            
                    mText.setBackgroundColor(Color.parseColor("#00FFFF"));                         
                    colorBack="#00FFFF";                                      
                }else{                             
                    mText.setTextColor(Color.parseColor("#00FFFF"));                        
                    colorText="#00FFFF";                              
                }                                 
                break;                           
                ········                              
        }                            
    }                                
 效果展示：                          
 ![image](https://user-images.githubusercontent.com/90746476/143774747-012b13eb-4c5e-4185-8339-f9f554dff011.png)                                              
点击后会变色                  
2.改变字体颜色大小：                       
 关键代码：                          
在strings.xml中添加代码： 字体大小 10号字 16号字 20号字 字体颜色 红色 黄色 在editor_options_menu.xml中添加代码：                              

            <item                 
                android:id="@+id/font_16"                
                android:title="@string/font16" />                 
            <item                
                android:id="@+id/font_20"                
                android:title="@string/font20" />                
        </group>                            
    </menu>                                  
</item>                 

<item                
    android:title="@string/font_color"              
    android:id="@+id/font_color"            
    >
    <menu>                   
        <!--定义一组普通菜单项-->               
        <group>               
            <!--定义两个菜单项-->             
            <item
                android:id="@+id/red_font"               
                android:title="@string/red_title" />               
            <item                           
                android:title="@string/yellow_title"                  
                android:id="@+id/yellow_font"/>                  
        </group>                      
    </menu>                        
</item>                           
效果展示：                         
![image](https://user-images.githubusercontent.com/90746476/143775090-a7bc15be-aeff-4bfc-9be7-f91800e5a339.png)                             


