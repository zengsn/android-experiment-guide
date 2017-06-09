# ʵ��9��Android�ۺ�ʵ��

###13. ��ҵ�ύϵͳ
��Ҫ˵��������APPǰ��Ҫ��¼����Ҫ�У�������ҵ����ͼ�¼��ҵ�ɼ������ܣ����⻹��һ��XML������Fragment������ͨ����ȡ�ļ�URL����������ʾXML�ļ�������������

1.���࣬ʵ���˵�¼���ܣ������ʼ���Լ��ĸ�Fragment�������л�����
public class Net1414080903212Activity extends AppCompatActivity implements View.OnClickListener{
	.
	.
	.
	//��APP��������ʾ��¼����
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.login1414080903212);
        initView();	//ȡ����¼ʱ����Ҫ�����    
        Map<String, String > userInfo = Utils.getUserInfo(this);	 //ȡ���˺�
        if (userInfo != null)   {
            //��ʾ�ڽ�����
            etNumber.setText(userInfo.get("number"));
            etPassword.setText(userInfo.get("password"));
        }
    }
	.
	.
	.
	//���ݵ����ť�������¼������õ�¼�����Լ��ĸ�Fragment�������л�����
    @Override
    public void onClick(View v) {
        switch (v.getId())  {
            case R.id.btn_login:
                //��������¼����ťʱ����ȡ�ʺź�����
                String number = etNumber.getText().toString().trim();
                String password = etPassword.getText().toString().trim();
                //����ʺź������Ƿ���ȷ
                if (TextUtils.isEmpty(number))  {
                    Toast.makeText(this, "�������˺�", Toast.LENGTH_SHORT).show();
                    return;
                }
                if (TextUtils.isEmpty(password))  {
                    Toast.makeText(this, "����������", Toast.LENGTH_SHORT).show();
                    return;
                }
                //��¼�ɹ�
                Toast.makeText(this, "��¼�ɹ�", Toast.LENGTH_SHORT).show();
                setContentView(R.layout.activity_net1414080903212);		//��¼�ɹ���ʹҳ��ӵ�¼������ת��APP��ҳ
                bindView();		//UI�����ʼ�����¼���
                break;
        }

        android.app.FragmentTransaction transaction = getFragmentManager().beginTransaction();
        hideAllFragment(transaction);	//��������fragment
        switch (v.getId()) {
            case R.id.navigation_home:
                selected();		//��ʼ�Ƚ����е�����Ϊδѡ��
                tab_homepage.setSelected(true);
                if (fra_home == null)   {
                    fra_home = new HomeFragment("������ҳ");	//����ҳ��һ������ҳ�����κι���
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

2.������¼������
public class Utils {
    //�����¼�˺ź����뵽data1414080903212.xml�ļ���
    public static boolean saveUserInfo(Context context, String number,
                                       String password) {
        SharedPreferences sp = context.getSharedPreferences("data1414080903212", Context.MODE_PRIVATE);
        SharedPreferences.Editor edit = sp.edit();
        edit.putString("userName", number);
        edit.putString("pwd", password);
        edit.commit();
        return true;
    }
    //��data1414080903212.xml�ļ��л�ȡ�洢���ʺź�����
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

3.������ҵ����Fragment����
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

4.������ҵ����Activity���Ĵ���
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
                    Toast.makeText(this, "�����ɹ�", Toast.LENGTH_SHORT).show();
                }else{
                    Toast.makeText(this, "����ʧ��", Toast.LENGTH_SHORT).show();
                }
        }
    }
}

5.������ҵ�ɼ�Fragment���루ͬ3���ƣ�
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

6.������ҵ�ɼ�Activity���Ĵ��루ͬ4���ƣ�
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

7.XML�������Ĵ���
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
            //��github�ϲɼ�����
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
            //���½���
            mTextView.setText(hw);
        }

    };
}

8.��ҳFragment����
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