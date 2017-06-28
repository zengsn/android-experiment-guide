# <center>LitePal的使用</center>
#### 一、LitePal是什么
>	LitePal是一款开源的Android数据库框架，采用ORM模式，即对象关系映射，以面向对象的方式对数据库进行操作，不再使用繁琐的SQLite语句。与之同比ORM型的数据库还有GreeDao等，这里我使用了LitePal，GitHub项目地址为：[https://github.com/LitePalFramework/LitePal](https://github.com/LitePalFramework/LitePal)   目前还在维护。

#### 二、LitePal的配置
- **2.1在Gradle中添加依赖：**

	```
			dependencies {
			compile 'org.litepal.android:core:1.5.1'
			}
	``` 

- **2.1在main目录下新建目录，名称为assets，在此目录下新建一个名为litepal.xml的文件，在文件下加入如下代码，代码示例如下：**
 				
		 <?xml version="1.0" encoding="utf-8" ?>
		  <litepal>
		    <dbname value="NewItem"></dbname>
		    <version value="1"></version>
		    <list>
		        <mapping class="edu.hzuapps.androidlabs.homeworks.net1414080903220.knowdev.bean.News"></mapping>
		        <mapping class="edu.hzuapps.androidlabs.homeworks.net1414080903220.knowdev.bean.DevArticle"></mapping>
		    </list>
		</litepal>


`<dbname>`指定了数据库的名称，`<version>`指的是数据库的版本号，而`<list>`则是数据映射模型，<mapping>指向的就是模型model，也可以认为是javabean。


#### 三、LitePal的使用
- **3.1创建数据库**
创建数据库，只需要一条语句：`Litepal.getDatabase()`即可，注意LitePal使用前需要初始化，一种比较推荐的做法是新建一个application类，在onCreate方法中初始化，例如：

```Java
					public class KnowApplication extends LitePalApplication {
					
					    public static KnowApplication knowApplication;
					
					    @Override
					    public void onCreate() {
					        super.onCreate();
					        if (knowApplication == null) {
					            knowApplication = this;
					        }
					        LitePalHelper litePalHelper=new LitePalHelper();
					
					    }
			
``` 
- **3.2下面来讲一下如何使用LitePal进行增删改查，注意：要执行CRUD，模型类要继承DataSupport才可！下面以News类为例，展示如何操作。**

##### （1）首先定义一个News类，继承DataSupport
```java	
			/**
			 * @Variable: ctime 新闻发布时间
			 * @Variable: title 新闻标题
			 * @Variable: description 新闻来源
			 * @Variable: picUrl 新闻图片地址
			 * @Variable: url 新闻源地址
			 */
			public class News extends DataSupport implements Serializable {
			
			    String ctime;
			    String title;
			    String description;
			    String picUrl;
			    String url;
			
			    public String getCtime() {
			        return ctime;
			    }
			
			    public void setCtime(String ctime) {
			        this.ctime = ctime;
			    }
			
			    public String getTitle() {
			        return title;
			    }
			
			    public void setTitle(String title) {
			        this.title = title;
			    }
			
			    public String getDescription() {
			        return description;
			    }
			
			    public void setDescription(String description) {
			        this.description = description;
			    }
			
			    public String getPicUrl() {
			        return picUrl;
			    }
			
			    public void setPicUrl(String picUrl) {
			        this.picUrl = picUrl;
			    }
			
			    public String getUrl() {
			        return url;
			    }
			
			    public void setUrl(String url) {
			        this.url = url;
			    }
			
			}
```
##### （2）添加数据：
例如向News类添加数据：我们可以先对News实例化，或者用News的set方法给属性复制，例如：
```java
            News news=newsList.get(getLayoutPosition());
            news.setCtime("2017-06-23 16:34");
            news.setDescription("自动驾驶技术的领军者不是汽车公司，而是它");
            news.setDescription("腾讯科技");
            news.setPicUrl("http://inews.gtimg.com/newsapp_ls/0/1699675331_300240/0");
            news.save();
```         
使用save方法即可将数据保存到数据库中，此方法继承与DataSupport。
#####（3）更新数据：
1.更新数据同样可以使用`save`方法，只不过这种更新方法只能对已存储的对象进行操作，只之前执行过save方法或者改对象是从数据库读取出来的。

2.第二种方法则是使用`updateAll`方法:
```java			
			news.setCtime("2017-06-23 16:34");
            news.setTitle("自动驾驶技术的领军者不是汽车公司，而是它");
            news.updateAll("ctime=? and title=?","腾讯科技","      news.updateAll("description=? and     picUrl=?","腾讯科技","http://inews.gtimg.com/newsapp_ls/0/1699675331_300240/0");
 ```          
示例中将将ctime等于*2017-06-23 16:34*且title是"*自动驾驶技术的领军者不是汽车公司，而是它*"的数据将description更新为*腾讯科技*，picUrl更新为*“http://inews.gtimg.com/newsapp_ls/0/1699675331_300240/0*”

LitePal的更新数据方法较多，这里这简单地介绍。
##### （4）删除数据：
1.第一种是使用`delete`方法删除对象，改方法同样是针对已存储对象的。

2.第二种是`deleteAll`方法:

     DataSupport.deleteAll(News.class,"description<?","腾讯科技");
deleteAll方法中第一个为模型类名称，这里指定为News.class，第二个参数为description<?，它即代表要删除数据的条件，后面的是条件值，需要注意的是实际上deleteAll除第一个参数为模型类名称外，后面并没有限制参数的个数，即后面是 `String... conditions`
##### （5）查询数据：
1.查询所有数据可以用：

   	 List<News>newsList=DataSupport.findAll(News.class);
findAll的参数为你要查询的模型类，返回的是一个集合。

2.查询第一条数据

    News news =DataSupport.findFirst(News.class);
3.查询最后一条数据

 	News news =DataSupport.findLast(News.class);
4.查询指定的列,即只查该列的数据，下面只查询title的数据，返回的是一个集合

	List<News>newsList=DataSupport.select("title").find(News.class);
LitePal的查询方式有很多，具体可以查看官方文档，这里不再说明

#### 四、小结
本次的内容只是简单地介绍了LitePal的入门知识，具体还需要后面再实际项目中灵活使用，如果有学习过hibernate之类的话，理解起来LitePal就简单地多了。

pull request地址：https://github.com/hzuapps/android-labs-2017/pull/416

**By wubucheng**

**--2017.6.23**

