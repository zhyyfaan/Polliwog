# Polliwog v-1.0
A tool of android demo rapid generation for college student.   
   
**Designer: Xiangyu Gao  
Developer: Shengzhong Tang, Yifan Zhu  
Assistant：Liangyan Mo, Mengjiao He**  
(Email:cwing.gao@gmail.com / 302787376@qq.com)  
*We welcome you to join us，and perfect and Perfect and extend this tool software together.*

  
## Overview   
Polliwog is a set of framework and auxiliary software of rapid developing Android App Demo for college students or other non-professional groups with a certain computer foundation. This auxiliary software need be combined with Android Studio (From Google) software. It can realize the automatic generation from visual interface development to Java code. The automatic generation part is mainly a fixed form of code part, which is most suitable for user-oriented application-oriented App Demo development. Users can adjust automatically generated content , format according to their needs and modify our framework to suit their own development needs.

There is only one set of development framework design at present, and we will provide more design framework and more personalized assistant software to meet more needs of App demo development in the subsequent version update iteration process.

  
  
## Detailed introduction  

### Framework introduction
The framework is mainly designed around the Activity, we design the Page class under the level of which. We use the Page class to implement each layout file, and then use the interface to create a Page object to complete the actual function of the interface control. The structure of the frame is as follows:
![image](https://github.com/CWing-Gao/Polliwog/blob/master/Images-folder/figure_1.png)

It requires developers to write a holistic layout during the development process. A completed page is completed by a ***.xml file or eventually a variety of scattered controls are referenced in a ***.xml file.
In addition to the Page class, there are Adapter classes, Msg classes, and so on. The Adapter class is designed and implemented for the Listview and other controls to facilitate the addition of the Item layout to the Listview control through the adapter. The Msg class is a message declaration class that declares information encoding and command encoding between Page and Page, Page and Activity. Of course, you can create a new class generator by modifying our code to suit your own design framework.
The communication between Page itself as well as Page and Activity is implemented by Handler.


### Introduction of auxiliary software
#### XML file writing
Regarding the layout file, the development process is not different from the original development process. It mainly uses the development tools inside Android Studio and complete by the combination of visual drag-and-drop development and code writing.
Among them, in order to facilitate the use of the auxiliary code generation tool, each Page layout should have a complete layout file to be combined, and your file name should be unified in the prefix or suffix, as well as the Adapter Item interface.

#### Java code generation
After generating the XML code, you can use the auxiliary software to generate Java code in a fixed format. The developer only needs to fill in the response logic part of the control to complete a simple demo development.

#### Auxiliary software instructions
The auxiliary software can convert the folders of the constituent files of multiple layout files into Java code, or convert a single file once. In the overall project generation, you need to set the recognition characters in the settings to let the software distinguish your layout file types. For example, if all Page layout files have a prefix (or suffix) Page, you can set them in the settings. The settings will be saved locally as a file for continued use, but not for individual file generation.
The implementation function of each control to be generated is stored in the widget_collection.csv file. When converting, the software firstly recognizes the XML file and then reads the settings in the widget_collection.csv to generate the Java code, so the user can extend the control type and its own function without modifying the source code of the program.


## Appendix

### The generation paradigm for Java code  

#### PageClass

      package cwing;  //Project package

      //Basic import
      import android.os.Handler;
      import android.os.Message;
      import android.support.v7.app.ActionBar;
      import android.support.v7.app.AppCompatActivity;

      //Widget class contains
      import android.widget.Button;


      public class PageName implements View.OnClickListener, {
          AppCompatActivity parent=null;	//Parent Activity
          MainActivity.MainHandler mainHandler=null;  //Core handle processor
          
         //All widget declarations detected
         //privat Widgetclass Widget+name=null;
          private Button buttonName=null;

      public PageGet(AppCompatActivity parent, Handler mainHandler){
              this.parent= parent;
             this.mainHandler=( MainActivity.MainHandler)mainHandler;
          }

          public void setUI() {
              parent.setContentView(R.layout.page_name);
              ActionBar actionBar = parent.getSupportActionBar();
              actionBar.hide();
          }

          public void loadControl() {
              buttonName=(Button) parent.findViewById(R.id.butt_back);
          }

         //Widget proprietary function
          @Override
          public void onClick(View view) {		
              Message msg=new Message();
              switch (view.getId()){
                  case R.id.butt_back:
                      break;
              }

          }

          void memoryClear()
          {
          }

          public void onClose() {
              return false;
          }
      }

#### Adapter 
      public class AdapterHomeList extends ArrayAdapter<ContentCourseItem> {

          private int resourceId;
          private AppCompatActivity parent=null;
          private List<ContentCourseItem> list;
          Main.MainHandler mainHandler=null;


          public AdapterHomeList(@NonNull Context context, @LayoutRes int resource, List<ContentCourseItem> objects,Main.MainHandler handler) {
              super(context, resource, objects);
              this.parent=(AppCompatActivity) context;
              Log.d("resourceId", resource + "");
              this.resourceId = resource;
              this.list=objects;
              this.mainHandler=handler;
          }


          @Override
          public View getView(int position, @Nullable final View convertView, @NonNull ViewGroup parent) {
              Log.d("resourceId", resourceId + "");
              View view = LayoutInflater.from(getContext()).inflate(resourceId, parent, false);

              ContentCourseItem content=(ContentCourseItem) getItem(position);

              ImageView image1=(ImageView) view.findViewById(R.id.image_1);
              TextView textName1=(TextView) view.findViewById(R.id.text_name_1);
              TextView textTeacher1=(TextView) view.findViewById(R.id.text_teacher_1);
              TextView textType1=(TextView) view.findViewById(R.id.text_type_1);



              image1.setImageBitmap(content.image_1);
              textName1.setText(content.name_1);
              textTeacher1.setText(content.teacher_1);
              textType1.setText(content.type_1);

              ImageView image2=(ImageView) view.findViewById(R.id.image_2);
              TextView textName2=(TextView) view.findViewById(R.id.text_name_2);
              TextView textTeacher2=(TextView) view.findViewById(R.id.text_teacher_2);
              TextView textType2=(TextView) view.findViewById(R.id.text_type_2);

              image2.setImageBitmap(content.image_2);
              textName2.setText(content.name_2);
              textTeacher2.setText(content.teacher_2);
              textType2.setText(content.type_2);


              LinearLayout linearLayout1=(LinearLayout) view.findViewById(R.id.linear_1);
              LinearLayout linearLayout2=(LinearLayout) view.findViewById(R.id.linear_2);

              linearLayout1.setOnClickListener(new View.OnClickListener() {
                  @Override
                  public void onClick(View view) {
                      Message msg=new Message();
                      msg.what= Msg.ACTION_INTO_COURSE;
                      mainHandler.sendMessage(msg);

                  }
              });

              linearLayout2.setOnClickListener(new View.OnClickListener() {
                  @Override
                  public void onClick(View view) {
                      Message msg=new Message();
                      msg.what= Msg.ACTION_INTO_COURSE;
                      mainHandler.sendMessage(msg);
                  }
              });

              return view;
          }
      }

### Identify schedules of widget (You can expand it)

| Name | Package | function |
|--|--|--|
|Button | android.widget.Button | onClick(View v) |
|ToggleButton | android.widget.ToggleButton | |
|Textview | android.widget.TextView | |
|checkbox | android.widget.CheckBox | |
|RadioButton | android.widget.RadioButton | |
|checkedTextView | android.widget.CheckedTextView | |
|spinner	|	android.widget.Spinner ||
|progressbar	| android.widget.ProgressBar ||
|seekbar | android.widget.SeekBar ||
|QuickcontactBadge	|	android.widget.QuickContactBadge ||
|ratingbar |	android.widget.RatingBar ||
|switch | android.widget.switch ||
|Space	| android.widget.Space ||
|gridlayout |||
|framelayout	| android.widget.FrameLayout||
|linearlayout | android.widget.LinearLayout ||
|relativelayout	| android.widget.RelativeLayout||
|tablelayout | android.widget.TableLayout ||
|tablerow	| android.widget.TableRow ||
|radiogroup | android.widget.RadioGroup ||
|listview	| android.widget.ListView ||
|expandablelistview | android.widget.ExpandableListView ||
|scrollview | android.widget.ScrollView ||
|horizontalscrollview | android.widget.HorizontalScrollView||
|tabhost	|android.widget.TabHost||
|webview|android.webkit.WebView||
|searchview	|android.widget.SearchView||
|imagebutton	| android.widget.ImageButton||
|imageview|android.widget.ImageView||
|videoview | android.widget.VideoView||
|datepicker | android.widget.DatePicker||
|timepicker | android.widget.TimePicker||
|calenderview| ||
|textclock | android.widget.TextClock||

