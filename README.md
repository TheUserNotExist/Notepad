# 添加笔记的时间戳
通过修改源码来添加笔记的时间戳
## 修改过程
**1.添加时间戳的布局**  
在noteslist_item.xml中添加时间戳的TextView，添加代码如下：  
<TextView  
&emsp;&emsp;&emsp;&emsp; android:id="@+id/text2"  
&emsp;&emsp;&emsp;&emsp; android:layout_width="match_parent"  
&emsp;&emsp;&emsp;&emsp; android:layout_height="wrap_content"  
&emsp;&emsp;&emsp;&emsp; android:paddingLeft="5dip"  
&emsp;&emsp;&emsp;&emsp; android:singleLine="true"  
&emsp;&emsp;&emsp;&emsp; android:gravity="center_vertical"  
 */>  
**2.修改时间戳的格式**  
修改笔记在NoteEditor中的updateNote方法和新建笔记在NotePadProvider中的insert方法来将格式转换为常用的格式：

修改updateNote方法后的代码如下:  
long now = System.currentTimeMillis();  
Date date = new Date(now);  
SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yy-MM-dd HH:mm:ss");  
String dateFormat = simpleDateFormat.format(date);  
values.put(NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE, dateFormat);  

新建的insert方法的代码如下：  
Long now = Long.valueOf(System.currentTimeMillis());  
Date date = new Date(now);  
SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yy-MM-dd HH:mm:ss");  
String dateFormat = simpleDateFormat.format(date);   
if (values.containsKey(NotePad.Notes.COLUMN_NAME_CREATE_DATE) == false) {  
    values.put(NotePad.Notes.COLUMN_NAME_CREATE_DATE, dateFormat);  
}  
if (values.containsKey(NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE) == false) {  
    values.put(NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE, dateFormat);  
}  


**3.投影修改时间的列**  
在PROJECTION中添加修改时间的列的投影，添加代码如下：  
NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE,  

PROJECTION只定义了需要被取出来的数据列，而之后用Cursor进行数据库查询，再之后用Adapter进行装填。  
我们需要将显示列dataColumns和他们的viewIDs加入修改时间这一属性  
修改后的代码如下：  
String[] dataColumns = { NotePad.Notes.COLUMN_NAME_TITLE, NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE }  
int[] viewIDs = { android.R.id.text1, R.id.text2 }  
## 运行效果
![conclusion](https://github.com/TheUserNotExist/Notepad/blob/master/1.png)
