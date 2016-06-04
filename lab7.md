# 实验7：Android网络编程

## 7.1 知识点

###1.     

###2.     

## 7.2 实例步骤

###1. 根据位置信息从中国天气网获取天气信息
简要说明……  
详细步骤……  

###2. Socket编程
简要说明  
　　这个例子只用了TCP协议下的Socket编程，因此这里只讨论TCP的情况。  
　　在java中，服务器端socket、bind、listen等操作被封装在ServerSocket类库中，客户端socket、connect等操作被封装在Socket类库中。如需了解这些操作细节，可学习C语言下的socket编程。  

详细步骤      
服务器端步骤   
1). servs = new ServerSocket(port);　　　//创建套接字，port 为指定的端口号  
2). socket = servs.accept();　　　　　　　//等待连接，主线程会阻塞在这里  
3). 为每个连接创建线程来服务;  
4). 线程中进行读写socket【通信】。  

客户端步骤   
1). socket = Socket("ip_str", port);　　//创建套接字，ip_str 为服务器端IP，port为端口号，和服务器端指定的要一样    
2). 读写socket。　　　　　　　　　　　　//1).中已经执行连接服务器的操作，所以接下来可以直接读写。  
注意：    
1). 客户端连接服务器的操作要放在新线程里面。  
2). 需要添加权限：
```
    <uses-permission android:name="android.permission.INTERNET"/>  
```
3). 真机调试的时候，真机和主机要在同一个局域网。  
核心代码  
服务器端：[(所有源码)](https://github.com/isshe/Android-20160303/tree/master/MultiChatServ)
``` java
public class Myserver {
    //定义保存所有的Socket的ArrayList
    public static ArrayList<Socket> socketList = new ArrayList<Socket>();
    
    public static void main(String[] args) 
        throws IOException
    {
        ServerSocket ss = new ServerSocket(9402);               //端口号为9402
        while (true)
        {
            Socket s = ss.accept();
            socketList.add(s);　　　　　　　　　　　//把新连接加入ArrayList中。
            //每连接一个客户端就开一个线程为之服务
            new Thread(new ServerThread(s)).start();
        }   
    }   
}  
```
客户端代码：[(所有源码)](https://github.com/isshe/Android-20160303/tree/master/MultiChat/app/src/main/java/com/example/dell/multichat)
``` java
    public void run()
    {
        try {
            s = new Socket("192.168.240.22", 9402);            //IP是服务器IP， 端口号和服务器一致
            br = new BufferedReader((new InputStreamReader(s.getInputStream())));
            os = s.getOutputStream();       
            
            //do something

        }
        catch (...)
        {
            //...
        }
    }
```
另一个例子：https://github.com/hzuapps/android-labs/tree/master/app/src/main/java/edu/hzuapps/androidworks/homeworks/net1314080903204/tcp_tester 

###3. 从aqicn.org获取PM2.5信息@ZhengQZ123
简要说明……  
    利用aqicn.org提供的接口，显示pm2.5的数值
详细步骤……  
1. 获取网络权限
```
    <uses-permission android:name="android.permission.INTERNET"/>  
```
2.读取http://aqicn.org/publishingdata/json提供的json数据
``` 
                     BufferedReader reader = new BufferedReader(new InputStreamReader(new
                     URL("http://aqicn.org/publishingdata/json").openStream(), "utf-8"));
                     String line=null;
                     StringBuffer content =new StringBuffer();
                     while((line=reader.readLine())!=null)
                     {
                        content.append(line);
                     }
 ```
 3.解析获取到的json数据
 ``` 
                    try {
                        JSONArray jsonarr=new JSONArray(s);
                        JSONObject firstJO=jsonarr.getJSONObject(0);
                        JSONArray pollutants=firstJO.getJSONArray("pollutants");
                        JSONObject firstPollutants=pollutants.getJSONObject(0);
                        System.out.println("cityName=" firstJO.getString("cityName") ",local=" firstJO.getString("localName"));
                        String cityName= firstJO.getString("cityName");
                        String localName= firstJO.getString("localName");
                        Double pollutant=firstPollutants.getDouble("value");
                        String a=cityName+" "+localName+":"+pollutant;
                        firstJO.getString("localName"),firstPollutants.getDouble("value"));
                        tvPmData.setText(a);
                    } catch (JSONException e) {
                        e.printStackTrace();
                    }
``` 
4.将获取到的数据按照需要显示出来 tvPmData.setText(a)

activity代码：(https://github.com/ZhengQZ123/android-labs/blob/master/app/src/main/java/edu/hzuapps/androidworks/homeworks/net1314080903247/Net1314080903247Activity.java)

AndroidManifest代码：(https://github.com/ZhengQZ123/android-labs/blob/master/app/src/main/java/edu/hzuapps/androidworks/homeworks/net1314080903247/AndroidManifest.xml)

activity xml代码：(https://github.com/ZhengQZ123/android-labs/blob/master/app/src/main/res/layout/activity_net1314080903247.xml)

###4. RSS阅读器
简要说明……  
1.什么是 RSS？
- RSS 指 Really Simple Syndication（真正简易联合）
- RSS 使您有能力聚合（syndicate）网站的内容
- RSS 定义了非常简单的方法来共享和查看标题和内容
- RSS 文件可被自动更新</li>
- RSS 允许为不同的网站进行视图的个性化
- RSS 使用 XML 编写

2.一个RSS实例文档
- RSS 文档使用一种简单的自我描述的语法：
   ```
   <?xml version="1.0" encoding="ISO-8859-1" ?>
<rss version="2.0">

<channel>
  <title>W3School Home Page</title>
  <link>http://www.w3school.com.cn</link>
  <description>Free web building tutorials</description>
  <item>
    <title>RSS Tutorial</title>
    <link>http://www.w3school.com.cn/rss</link>
    <description>New RSS tutorial on W3School</description>
  </item>
  <item>
    <title>XML Tutorial</title>
    <link>http://www.w3school.com.cn/xml</link>
    <description>New XML tutorial on W3School</description>
  </item>
</channel>

</rss>
   ```
文档中的第一行：XML 声明 - 定义了文档中使用的 XML 版本和字符编码。此例子遵守 1.0 规范，并使用 ISO-8859-1 (Latin-1/West European) 字符集。
下一行是标识此文档是一个 RSS 文档的 RSS 声明（此例是 RSS version 2.0）。
下一行含有 <channel> 元素。此元素用于描述 RSS feed。
<channel> 元素有三个必需的子元素：
<title> - 定义频道的标题。
<link> - 定义到达频道的超链接。
<description> - 描述此频道（比如免费的网站建设教程）
每个 <channel> 元素可拥有一个或多个 <item> 元素。
每个 <item> 元素可定义 RSS feed 中的一篇文章或 "story"。
<item> 元素拥有三个必需的子元素：
<title> - 定义项目的标题。
<link> - 定义到达项目的超链接。
<description> - 描述此项目
最后，后面的两行关闭 <channel> 和 <rss> 元素。

 3.解析xml文件
 <br>这里采用的是SAX解析方法，SAX是一种占用内存少且解析速度快的解析器，它采用的是事件启动，它不需要解析完整个文档，而是按照内容顺序 看文档某个部分是否符合xml语法，如果符合就触发相应的事件，所谓的事件就是些回调方法（callback），这些方法定义在ContentHandler中，下面是其主要方法：
 - startDocument：当遇到文档的时候就触发这个事件，调用这个方法可以在其中做些预处理工作。

 - startElement(String namespaceURI,String localName,String qName,Attributes atts)：当遇开始标签的时候就会触发这个方法。

 - endElement(String uri,String localName,String name)：当遇到结束标签时触发这个事件，调用此法可以做些善后工作。

 - charachers(char [] ch,int start,int length)：当遇到xml内容时触发这个方法，用new String(ch,start,length)可以接受内容。 


详细步骤……  
1. 从网络中获取RSS地址，xml格式
```
   // 从网络获取RSS地址
	private static String strUrl = "http://rss.it.sohu.com/rss/ityaowen.xml";
```
2.建立描述RSS信息的RssFeed和RssItem类
```
public class RSSFeed {
	private List<RSSItem> listview;
	private int itemCount = 0;
	private String title, pubDate;

	public RSSFeed() {
		listview = new Vector(0);
	}

	//// 添加RssItem条目,返回列表长度
	public int addItem(RSSItem item) {
		listview.add(item);
		itemCount++;
		return itemCount;
	}

	public List getList() {
		return listview;
	}

	int getItemsCount() {
		return itemCount;
	}

	// 根据下标获取RssItem
	public RSSItem getItem(int location) {
		return listview.get(location);
	}

	public List getItemsForList() {
		List<Map<String, Object>> data = new ArrayList<Map<String, Object>>();
		int SIZE = listview.size();
		for (int i = 0; i < SIZE; i++) {
			Map<String, Object> mdMap = new HashMap<String, Object>();
			mdMap.put(RSSItem.TITLE, listview.get(i).getTitle());
			mdMap.put(RSSItem.PUBDATE, listview.get(i).getPubDate());
			data.add(mdMap);
		}
		return data;
	}

	public String getTitle() {
		return title;
	}

	public void setTitle(String title) {
		this.title = title;
	}

	public String getPubDate() {
		return pubDate;
	}

	public void setPubDate(String pubDate) {
		this.pubDate = pubDate;
	}
}
```
```
public class RSSItem {

	public static String TITLE = "title";
	public static String PUBDATE = "pubDate";
	

	private String title = null;
	private String pubDate = null;
	private String link = null;
	private String description = null;
	
	public String getTitle() {
		return title;
	}

	public void setTitle(String title) {
		this.title = title;
	}

	public String getPubDate() {
		return pubDate;
	}

	public void setPubDate(String pubDate) {
		this.pubDate = pubDate;
	}

	public String getLink() {
		return link;
	}

	public void setLink(String link) {
		this.link = link;
	}

	public String getDescription() {
		return description;
	}

	public void setDescription(String description) {
		this.description = description;
	}

	@Override
	public String toString() {
		if(title.length()>18){
			return title.substring(0,18)+"...";
		}
		return super.toString();
	}
}

```
3.利用SAX进行解析
```
public class ReadHelper extends DefaultHandler {
	private RSSFeed r_Feed;//用于保存解析过程中的channel  
	private RSSItem r_item;//用于保存解析过程中的item   

	private static final int RSS_TITLE = 1;
	private static final int RSS_LINK = 2;
	private static final int RSS_PUBDATE = 3;
	private static final int RSS_DESCRIPTION = 4;
	int currentStatus = 0;

	public ReadHelper() {
	}

	public RSSFeed getFeed(){
		return r_Feed;//通过这个方法把解析结果封装在 RSSFeed 对象中并返回   
	}

	@Override
	public void characters(char[] ch, int start, int length)
			throws SAXException {
			if(length<5){
				return;
			}
			else{
				String conString = new String(ch,start,length);//获取字符串
				switch (currentStatus) {
				case RSS_TITLE:
					r_item.setTitle(conString);// 设置完后，重置为开始状态
					currentStatus = 0;
					break;
				case RSS_LINK:
					r_item.setLink(conString);
					currentStatus = 0;
					break;
				case RSS_PUBDATE:
					r_item.setPubDate(conString);
					currentStatus = 0;
					break;
				case RSS_DESCRIPTION:
					r_item.setDescription(conString);
					currentStatus = 0;
					break;
				default:
					break;
				}
			}
	}

	@Override
	public void endDocument() throws SAXException {

	}

	@Override
	public void endElement(String uri, String localName, String name)
			throws SAXException {
		if (localName.equals("item")) {
			// 如果解析一个item节点结束，就将rssItem添加到rssFeed中。
			r_Feed.addItem(r_item	);
			return;
		}
	}

	@Override
	public void startDocument() throws SAXException {
		// TODO Auto-generated method stub
		r_Feed = new RSSFeed();
		r_item = new RSSItem();
	}

	@Override
	public void startElement(String uri, String localName, String name,
			Attributes attributes) throws SAXException {
		if (localName.equals("channel")) {
			currentStatus = 0;
			return;
		}
		if (localName.equals("item")) {
			r_item = new RSSItem();
			return;
		}
		if (localName.equals("title")) {
			currentStatus = RSS_TITLE;
			return;
		}
		if (localName.equals("link")) {
			currentStatus = RSS_LINK;
			return;
		}
		if (localName.equals("pubDate")) {
			currentStatus = RSS_PUBDATE;
			return;
		}
		if (localName.equals("description")) {
			currentStatus = RSS_DESCRIPTION;;
			return;
		}
		currentStatus = 0;
	}

}

```
```
public RSSFeed loadData(String rssUrl) {
		try {
			try {
				url = new URL(rssUrl);
			} catch (MalformedURLException e) {
				e.printStackTrace();
			}
			SAXParserFactory xFactory = SAXParserFactory.newInstance();//构建sax解析工厂
			SAXParser parser = xFactory.newSAXParser();//解析工厂生产解析器
			XMLReader xReader = parser.getXMLReader();//通过saxParser构建xmlReader阅读器
           //构建自定义的xml解析器 作为 xmlReader的处理器（代理）
			handler = new ReadHelper();
			xReader.setContentHandler(handler);
			//使用url打开流，并将流作为 xmlReader解析的输入源并解析
			InputSource iSource = new InputSource(url.openStream());
			xReader.parse(iSource);
			return handler.getFeed();

		} catch (Exception e) {
			// TODO: handle exception
		}
		return null;
	}
```
4.RSS列表显示在UI中
```
public void updata() {
		if (rsFeed != null) {
			SimpleAdapter sAdapter = new SimpleAdapter(this, rsFeed
					.getItemsForList(), R.layout.list_row, new String[] {
					RSSItem.TITLE, RSSItem.PUBDATE }, new int[] {
					R.id.txt_title, R.id.txt_pubDate });
			setListAdapter(sAdapter);
			this.getListView().setSelection(0);
		} else {
			return;
		}
	}
```

```
public class RSSShowItem extends Activity{
	private TextView txtContent;
	private Button button;
	
	private String title,pubDate,description,link;
	
	@Override
	protected void onCreate(Bundle savedInstanceState) {
		// TODO Auto-generated method stub
		super.onCreate(savedInstanceState);
		setContentView(R.layout.list_showitem);
		
		txtContent = (TextView)findViewById(R.id.txt_content);
		button = (Button)findViewById(R.id.btn_back);
		button.setOnClickListener(new OnClickListener(){

			public void onClick(View v) {
				// TODO Auto-generated method stub
				finish();
			}
			
		});

		Intent intent = getIntent();
		if(intent != null){
			Bundle bundle = intent.getBundleExtra("com.lq.showitem");
			if(bundle != null){
				title = bundle.getString("title");
				pubDate = bundle.getString("pubDate");
				description = bundle.getString("description");
				link = bundle.getString("link");
			}
		}
		txtContent.setText(title+"\n\n"+pubDate+"\n\n"+description+"\n\n"+link);
	}
	
}

```
5.由于要访问网络，故在Manifest.xml文件中添加网络访问权限
```
    <uses-permission android:name="android.permission.INTERNET" />
```
