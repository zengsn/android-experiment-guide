# 实验9：Android综合实验

###13. 作业提交系统
简要说明：进入APP前需要登录，主要有：布置作业任务和记录作业成绩两大功能，另外还有一个XML解析的Fragment，可以通过获取文件URL来解析并显示XML文件所包含的数据

1.主类，实现了登录功能，界面初始化以及四个Fragment的来回切换功能
public class Net1414080903212Activity extends AppCompatActivity implements View.OnClickListener{
	.
	.
	.
	//打开APP，首先显示登录界面
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.login1414080903212);
        initView();	//取出登录时所需要的组件    
        Map<String, String > userInfo = Utils.getUserInfo(this);	 //取出账号
        if (userInfo != null)   {
            //显示在界面上
            etNumber.setText(userInfo.get("number"));
            etPassword.setText(userInfo.get("password"));
        }
    }
	.
	.
	.
	//根据点击按钮的侦听事件，调用登录功能以及四个Fragment的来回切换功能
    @Override
    public void onClick(View v) {
        switch (v.getId())  {
            case R.id.btn_login:
                //单击“登录”按钮时，获取帐号和密码
                String number = etNumber.getText().toString().trim();
                String password = etPassword.getText().toString().trim();
                //检查帐号和密码是否正确
                if (TextUtils.isEmpty(number))  {
                    Toast.makeText(this, "请输入账号", Toast.LENGTH_SHORT).show();
                    return;
                }
                if (TextUtils.isEmpty(password))  {
                    Toast.makeText(this, "请输入密码", Toast.LENGTH_SHORT).show();
                    return;
                }
                //登录成功
                Toast.makeText(this, "登录成功", Toast.LENGTH_SHORT).show();
                setContentView(R.layout.activity_net1414080903212);		//登录成功后，使页面从登录界面跳转到APP首页
                bindView();		//UI组件初始化与事件绑定
                break;
        }

        android.app.FragmentTransaction transaction = getFragmentManager().beginTransaction();
        hideAllFragment(transaction);	//隐藏所有fragment
        switch (v.getId()) {
            case R.id.navigation_home:
                selected();		//初始先将所有导航设为未选中
                tab_homepage.setSelected(true);
                if (fra_home == null)   {
                    fra_home = new HomeFragment("这是主页");	//该主页是一个空主页，无任何功能
                    transaction.add(R.id.ly_content, fra_home);
                }
                else {
                    transaction.show(fra_home);
                }
                break;
				.
				.
				.
        }
        transaction.commit();
    }
}

2.创建登录工具类
public class Utils {
    //保存登录账号和密码到data1414080903212.xml文件中
    public static boolean saveUserInfo(Context context, String number,
                                       String password) {
        SharedPreferences sp = context.getSharedPreferences("data1414080903212", Context.MODE_PRIVATE);
        SharedPreferences.Editor edit = sp.edit();
        edit.putString("userName", number);
        edit.putString("pwd", password);
        edit.commit();
        return true;
    }
    //从data1414080903212.xml文件中获取存储的帐号和密码
    public static Map<String, String> getUserInfo(Context context)  {
        SharedPreferences sp = context.getSharedPreferences("data1414080903212", Context.MODE_PRIVATE);
        String number = sp.getString("userName", null);
        String password = sp.getString("pwd", null);
        Map<String, String> userMap = new HashMap<String, String>();
        userMap.put("number", number);
        userMap.put("password", password);
        return userMap;
    }
}

3.布置作业任务Fragment代码
public class AssignFragment extends Fragment {
    .
	.
	.
    public AssignFragment() {
    }

    public static AssignFragment newInstance(String context) {
        AssignFragment fragment = new AssignFragment();
        Bundle args = new Bundle();
        args.putString("context", context);
        fragment.setArguments(args);
        return fragment;
    }

    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
    }

    @Override
    public View onCreateView(LayoutInflater inflater, ViewGroup container,
                             Bundle savedInstanceState) {
        view = inflater.inflate(R.layout.frag1414080903212_assign,container,false);
        assignmentdao = new AssignmentDao(view.getContext());
        assignLV = (ListView) view.findViewById(R.id.assignList);
        assignmentlist = assignmentdao.findAll();
        assignmentadapter = new AssignmentAdapter();
        assignLV.setAdapter(assignmentadapter);
        return view;
    }

    public interface OnFragmentInteractionListener {
        void onFragmentInteraction(Uri uri);
    }

    @Override
    public void onResume()  {
        assignmentlist = assignmentdao.findAll();
        assignmentadapter.notifyDataSetChanged();
        super.onResume();
    }

    @Override
    public void onActivityCreated(Bundle savedInstanceState) {
        super.onActivityCreated(savedInstanceState);

        assin = (Button)view.findViewById(R.id.assignhw);
        assin.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(getActivity(), AssignActivity.class);
                startActivity(intent);
            }
        });
    }

    class AssignmentAdapter extends BaseAdapter {
        .
		.
		.
        public View getView(int position, View convertView, ViewGroup parent) {
            View item = convertView != null?convertView:View.inflate(getActivity(), R.layout.assignlist_1414080903212, null);
            TextView idTV = (TextView) item.findViewById(R.id.assign_id);
            TextView titleTV = (TextView) item.findViewById(R.id.assign_title);
            TextView contentTV = (TextView) item.findViewById(R.id.assign_content);
            TextView timeTV = (TextView) item.findViewById(R.id.sub_date);

            final Assignment ass = assignmentlist.get(position);
            idTV.setText(ass.getId()+"");
            titleTV.setText(ass.getHomework_title());
            contentTV.setText(ass.getHomework_content());
            timeTV.setText(ass.getSubmit_time());
            return item;
        }
    }
}

4.布置作业任务Activity核心代码
public class AssignActivity extends AppCompatActivity implements View.OnClickListener {
    .
	.
	.
    @Override
    protected void onCreate(Bundle saveInstanceState) {
        super.onCreate(saveInstanceState);
        setContentView(R.layout.assign_fragment1414080903212);
        btn = (Button)findViewById(R.id.buttonAssign);
        btn1 = (Button)findViewById(R.id.buttonCancelAssign);
        btn.setOnClickListener(this);
        btn1.setOnClickListener(this);
    }

    @Override
    public void onClick(View v) {
        switch (v.getId()){
            case R.id.buttonCancelAssign:
                super.finish();
                break;
            case R.id.buttonAssign:
                edittitle = (EditText)findViewById(R.id.texttitle);
                editcontent = (EditText)findViewById(R.id.textcontent);
                edittime = (EditText)findViewById(R.id.textdate);

                ass.setHomework_title(edittitle.getText().toString());
                ass.setHomework_content(editcontent.getText().toString());
                ass.setSubmit_time(edittime.getText().toString());

                AssignmentDao assdao = new AssignmentDao(this);
                int id=-1;
                if ( (id = assdao.assign(ass))!=-1 ) {
                    ass.setId(id);
                    AssignFragment.assignmentlist.add(ass);
                    Toast.makeText(this, "发布成功", Toast.LENGTH_SHORT).show();
                }else{
                    Toast.makeText(this, "发布失败", Toast.LENGTH_SHORT).show();
                }
        }
    }
}

5.批改作业成绩Fragment代码（同3类似）
public class MarkFragment extends Fragment {
	.
	.
	.
    public MarkFragment() {
    }

    public static MarkFragment newInstance(String context) {
        ...
    }

    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
    }

    @Override
    public View onCreateView(LayoutInflater inflater, ViewGroup container,
                             Bundle savedInstanceState) {
        ...
    }

    public interface OnFragmentInteractionListener {
        void onFragmentInteraction(Uri uri);
    }

    @Override
    public void onResume()  {
        ...
    }

    @Override
    public void onActivityCreated(Bundle savedInstanceState) {
        ...
    }

    class MarkAdapter extends BaseAdapter {
        ...
        }
    }
}

6.批改作业成绩Activity核心代码（同4类似）
public class MarkActivity extends AppCompatActivity implements View.OnClickListener {
    .
	.
	.
    @Override
    protected void onCreate(Bundle saveInstanceState) {
        ...
    }

    @Override
    public void onClick(View v) {
        ...
    }
}

7.XML解析核心代码
public class XmlFragment extends Fragment {
    .
	.
	.
    @Override
    public View onCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState) {
        View view = inflater.inflate(R.layout.xml_fragment1414080903212,container,false);
        handler = new Handler();
        Button button = (Button)view.findViewById(R.id.xml_btn);
        text = (TextView) view.findViewById(R.id.txt_xml);
        mTextView = (TextView) view.findViewById(R.id.txt_xml);
        button.setOnClickListener(new submitOnClickListener());

        return view;
    }
    class submitOnClickListener implements View.OnClickListener {
        @Override
        public void onClick(View v) {
            //从github上采集数据
            new Thread(){
                public void run(){
                    try {
                        Document docu = Jsoup.connect("https://github.com/HardyYao/android-labs-2017/blob/master/AndroidLabs/app/src/main/java/edu/hzuapps/androidlabs/homeworks/net1414080903212/data1414080903212.xml").get();
                        int i = 1;
                        Elements data = docu.select("#LC"+i);
                        while(!data.isEmpty()){
                            hw += data.text();
                            i++;
                            data = docu.select("#LC"+i);
                        }

                    }catch(IOException e){
                        System.out.println("catch IOException: " + e);
                    }
                    handler.post(runnableUi);
                }
            }.start();
        }
    }

    Runnable   runnableUi=new  Runnable(){
        @Override
        public void run() {
            //更新界面
            mTextView.setText(hw);
        }

    };
}

8.主页Fragment代码
public class HomeFragment extends Fragment {
    private String context;
    private TextView mTextView;

    public HomeFragment(String context){
        this.context = context;
    }

    @Nullable
    @Override
    public View onCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState) {
        View view = inflater.inflate(R.layout.home_fragment1414080903212,container,false);
        mTextView = (TextView)view.findViewById(R.id.txt_homepage);
        mTextView.setText(context);
        return view;
    }
}