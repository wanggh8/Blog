---
title: ButterKnife 
date: 2020-05-25 10:31:12
tags: Android

---

<!--more-->



## ButterKnife介绍

ButterKnife是一个专注于Android系统的View注入框架。ButterKnife用到的注解并不是在运行时反射的，而是在编译的时候生成新的class。

- ButterKnife项目地址：[https://github.com/JakeWharton/butterknife](https://github.com/JakeWharton/butterknife)

### ButterKnife的优势：

       1. 强大的View绑定和Click事件处理功能，简化代码，提升开发效率
       2. 方便的处理Adapter里的ViewHolder绑定问题
       3. 运行时不会影响APP效率，使用配置方便
       4. 代码清晰，可读性强

### 依赖

```java
implementation 'com.jakewharton:butterknife:8.8.1'
annotationProcessor 'com.jakewharton:butterknife-compiler:8.8.1'
```

### Activity使用：

```java
    public class MainActivity extends AppCompatActivity {
    
        //获取控件
        @BindView(R.id.name)
        EditText name;
    
        @BindView(R.id.btn)
        Button btn;
        @BindView(R.id.txt)
        TextView txt;
    
        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
    
            //绑定处理
            ButterKnife.bind(this);
        }
    
        //按钮点击事件处理
        @OnClick(R.id.btn)
        public void onViewClicked() {
    
            if (TextUtils.isEmpty(name.getText().toString().trim())){
                return;
            }
    
            if (name.getText().toString().trim().length() < 6){
                return;
            }
    
            txt.setText(name.getText());
        }
    }
```

### Fragment使用：

```java
    public class BlankFragment extends Fragment {
    
    
        @BindView(R.id.txt)
        TextView txt;
        @BindView(R.id.btn)
        Button btn;
    
    
        Unbinder unbinder;
    
        @Override
        public View onCreateView(LayoutInflater inflater, ViewGroup container,
                                 Bundle savedInstanceState) {
            // Inflate the layout for this fragment
            View inflate = inflater.inflate(R.layout.fragment_blank, container, false);
            unbinder = ButterKnife.bind(this, inflate);
            return inflate;
        }
    
        @Override
        public void onDestroyView() {
            super.onDestroyView();
            unbinder.unbind();
        }
    
        @OnClick(R.id.btn)
        public void onViewClicked() {
    
        }
    }
```