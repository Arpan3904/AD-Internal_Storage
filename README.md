# AD-Internal_Storage
## activity_main.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:gravity="center"
    >

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="username"
        android:id="@+id/t1"
        android:padding="40px"

        />
    <EditText

        android:layout_width="300px"
        android:layout_height="wrap_content"
        android:id="@+id/e1"
        android:layout_toRightOf="@+id/t1"

        />
    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="password"
        android:layout_below="@+id/t1"
        android:id="@+id/t2"
        android:padding="40px"
        />
    <EditText
        android:layout_width="300px"
        android:layout_height="wrap_content"
        android:id="@+id/e2"
        android:layout_below="@+id/e1"
        android:layout_toRightOf="@+id/t2"
        />
    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/b"
        android:layout_below="@+id/t2"
        android:text="Login"
        />

</RelativeLayout>
```
## MainActivity.java
```java

package com.example.internal_storage;
import androidx.appcompat.app.AppCompatActivity;
import android.content.Context;
import android.content.Intent;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.Toast;
import java.io.FileOutputStream;


public class MainActivity extends AppCompatActivity {
    EditText e1, e2;
    FileOutputStream fstream;
    Button b;
    Intent intent;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        b = findViewById(R.id.b);
        e1 = findViewById(R.id.e1);
        e2 = findViewById(R.id.e2);

        b.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                String username = e1.getText().toString()+"\n";
                String password = e2.getText().toString();

                try {
                    fstream = openFileOutput("user_details", Context.MODE_PRIVATE);
                    fstream.write(username.getBytes());
                    fstream.write(password.getBytes());
                    fstream.close();
                    Toast.makeText(getApplicationContext(), "Details Saved Successfully",Toast.LENGTH_SHORT).show();
                   intent = new Intent(MainActivity.this,Details.class);
                   startActivity(intent);

                } catch (Exception e) {
                    e.printStackTrace();
                }
            }
        });
    }
}

```
## activity_details.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:gravity="center"
    android:orientation="vertical"
    >

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"

        android:id="@+id/t1"
        android:padding="100px"/>



    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/b"
        android:text="Back"
        />

</LinearLayout>
```
## Details.java
```java
package com.example.internal_storage;

import androidx.appcompat.app.AppCompatActivity;

import android.content.Intent;

import android.os.Bundle;
import android.view.View;
import android.widget.Button;

import android.widget.TextView;



import java.io.FileInputStream;
import java.io.FileNotFoundException;

public class Details extends AppCompatActivity {
    TextView t1;
    Button b;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_details);
        b = findViewById(R.id.b);
        t1 = findViewById(R.id.t1);

        try {
            FileInputStream fin = openFileInput("user_details");
            StringBuffer sb = new StringBuffer();
            int in;
            while ((in = fin.read()) != -1) {
                sb.append((char) in);
            }
            String details = sb.toString();
            String[] userDetails = details.split("\n");


                t1.setText("Name: " + userDetails[0] + "\nPassword: " + userDetails[1]);


        } catch (Exception e) {
            e.printStackTrace();
        }

        b.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                Intent i = new Intent(Details.this, MainActivity.class);
                startActivity(i);
            }
        });
    }
}

```


