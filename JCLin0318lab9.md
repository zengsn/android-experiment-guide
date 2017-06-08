# 实验9：Android综合实验

###1. 实现账号密码登录

 ```  
public class Net1414080903117Activity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_net1414080903117);

        final EditText etUsername = (EditText) findViewById(R.id.et_main_username);
        final EditText etPassword = (EditText) findViewById(R.id.et_main_password);

        Button btLogin = (Button) findViewById(R.id.bt_login);

        btLogin.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                String username = etUsername.getText().toString();
                String password = etPassword.getText().toString();
                if (check(username, password)) {
                    startActivity(new Intent(Net1414080903117Activity.this, Net1414080903117MenuActivity.class));
                } else {
                    Toast.makeText(Net1414080903117Activity.this, "账号/密码错误！！", Toast.LENGTH_SHORT).show();
                }

            }
        });
    }


    public boolean check(String username, String password) {
        if (username.equals("JCLin0318") && password.equals("123"))
            return true;
        return false;
    }
}

```  

2. 将用户输入的水电费数据插入数据库
```  
Net1414080903117MySQLiteOpenHelper helper=new 
Net1414080903117MySQLiteOpenHelper(Net1414080903117SubmitActivity.this);
SQLiteDatabase db=helper.getWritableDatabase();
ContentValues values=new ContentValues();
values.put("electricity",etElectricity.getText().toString());
values.put("water",etWater.getText().toString());
values.put("date", Net1414080903117TimeUtil.getDateYMD());
db.insert("record",null,values);
db.close();

```  
3. 取出数据库每一条数据
```  
List<Net1414080903117RecordBean> beans=new ArrayList<>();
Net1414080903117MySQLiteOpenHelper helper=new Net1414080903117MySQLiteOpenHelper(this);
SQLiteDatabase db=helper.getReadableDatabase();
Cursor cursor=db.query("record",null,null,null,null,null,null);
while(cursor.moveToNext()){
    Net1414080903117RecordBean bean=new Net1414080903117RecordBean();
    bean.setDate(cursor.getString(cursor.getColumnIndex("date")));
    bean.setElectricity(cursor.getDouble(cursor.getColumnIndex("electricity")));
    bean.setWater(cursor.getDouble(cursor.getColumnIndex("water")));
    beans.add(bean);
}

```  
4.获取当前日期
```  
  public static  String getDateYMD(){
    SimpleDateFormat format=new SimpleDateFormat("yyyy-MM-dd");
    return format.format(new Date());
}
5.	数据实体类
```
public class Net1414080903117RecordBean {

    double electricity;
    double water;
    String date;

    public double getElectricity() {
        return electricity;
    }

    public void setElectricity(double electricity) {
        this.electricity = electricity;
    }

    public double getWater() {
        return water;
    }

    public void setWater(double water) {
        this.water = water;
    }

    public String getDate() {
        return date;
    }

    public void setDate(String date) {
        this.date = date;
    }
}
```
6.		获取网络数据
```
new Thread(){
    @Override
    public void run() {
        try {
            OkHttpClient client = new OkHttpClient();
            Request request = new Request.Builder().url("https://raw.githubusercontent.com/JCLin0318/android-labs-2017/master/AndroidLabs/app/src/main/java/edu/hzuapps/androidlabs/homeworks/net1414080903117/Net1414080903117.json").build();//目标地址
            Response response = client.newCall(request).execute();
            String responseData = response.body().string();
            ParseJson(responseData);
        } catch (Exception e) {
            e.printStackTrace();
        }
        handler.post(runnableUi);
    }
}.start();
