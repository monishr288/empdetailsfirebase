
# Ex.No:1 To create a employee details fields and to display the employee details using Firebase Database in Android Studio.


## AIM:

To create and display the employee details using Firebase Database in Android Studio.

## EQUIPMENTS REQUIRED:

Android Studio(Min.required Artic Fox)

## ALGORITHM:

Step 1: Open Android Stdio and then click on File -> New -> New project.

Step 2: Then type the Application name as HelloWorld and click Next. 

Step 3: Then select the Minimum SDK as shown below and click Next.

Step 4: Then select the Empty Activity and click Next. Finally click Finish.

Step 5: Design layout in activity_main.xml.

Step 6: Display the employee details in MainActivity file.

Step 7: Save and run the application.

## PROGRAM:
```
/*
Program to print the DatabaseTable using the firebasedatabase”.
Developed by : MONISH R
Registeration Number : 212223220061
*/
```
## MainActivity.java
```
package com.example.sqliteexample;


import androidx.appcompat.app.AppCompatActivity;

import android.database.Cursor;
import android.os.Bundle;
import android.util.Log;
import android.view.View;
import android.widget.EditText;
import android.widget.Toast;

public class MainActivity extends AppCompatActivity {

    EditText editUserID, editUserName, editPassword;

    DatabaseManager dbManager;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        editUserID = findViewById(R.id.editTextID);
        editUserName = findViewById(R.id.editTextUserName);
        editPassword = findViewById(R.id.editTextPassword);

        dbManager = new DatabaseManager(this);
        dbManager.open();
    }

    public void btnInsertPressed(View view) {

        dbManager.insert(
                editUserName.getText().toString(),
                editPassword.getText().toString()
        );

        Toast.makeText(this,
                "Data Inserted Successfully",
                Toast.LENGTH_SHORT).show();
    }

    public void btnFetchPressed(View view) {

        Cursor cursor = dbManager.fetch();

        if (cursor.moveToFirst()) {

            do {

                String id = cursor.getString(0);
                String name = cursor.getString(1);
                String password = cursor.getString(2);

                Log.i("DATABASE",
                        "ID : " + id +
                                " Name : " + name +
                                " Password : " + password);

            } while (cursor.moveToNext());
        }
    }
}
```
## activity_main.xml :
```
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <EditText
        android:id="@+id/editTextID"
        android:layout_width="220dp"
        android:layout_height="wrap_content"
        android:layout_marginTop="212dp"
        android:hint="Enter User ID"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.502"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <EditText
        android:id="@+id/editTextUserName"
        android:layout_width="220dp"
        android:layout_height="wrap_content"
        android:layout_marginTop="12dp"
        android:hint="Enter User Name"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.497"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@id/editTextID" />

    <EditText
        android:id="@+id/editTextPassword"
        android:layout_width="220dp"
        android:layout_height="wrap_content"
        android:hint="Enter Password"
        android:inputType="textPassword"
        app:layout_constraintTop_toBottomOf="@id/editTextUserName"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent"/>

    <Button
        android:id="@+id/buttonInsert"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Insert"
        android:onClick="btnInsertPressed"
        app:layout_constraintTop_toBottomOf="@id/editTextPassword"
        app:layout_constraintStart_toStartOf="parent"/>

    <Button
        android:id="@+id/buttonFetch"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Fetch"
        android:onClick="btnFetchPressed"
        app:layout_constraintTop_toBottomOf="@id/editTextPassword"
        app:layout_constraintEnd_toEndOf="parent"/>

</androidx.constraintlayout.widget.ConstraintLayout>
```
## DatabaseHelper.java
```
package com.example.sqliteexample;




import android.content.Context;
import android.database.sqlite.SQLiteDatabase;
import android.database.sqlite.SQLiteOpenHelper;

public class DatabaseHelper extends SQLiteOpenHelper {

    static final String DATABASE_NAME = "USERDB";
    static final int DATABASE_VERSION = 1;

    static final String TABLE_NAME = "USERS";
    static final String USER_ID = "_id";
    static final String USER_NAME = "username";
    static final String USER_PASSWORD = "password";

    static final String CREATE_TABLE =
            "CREATE TABLE " + TABLE_NAME +
                    " (" +
                    USER_ID + " INTEGER PRIMARY KEY AUTOINCREMENT, " +
                    USER_NAME + " TEXT, " +
                    USER_PASSWORD + " TEXT);";

    public DatabaseHelper(Context context) {
        super(context, DATABASE_NAME, null, DATABASE_VERSION);
    }

    @Override
    public void onCreate(SQLiteDatabase db) {
        db.execSQL(CREATE_TABLE);
    }

    @Override
    public void onUpgrade(SQLiteDatabase db, int oldVersion, int newVersion) {
        db.execSQL("DROP TABLE IF EXISTS " + TABLE_NAME);
        onCreate(db);
    }
}
```
## DatabaseManager.java
```
package com.example.sqliteexample;


import android.content.ContentValues;
import android.content.Context;
import android.database.Cursor;
import android.database.sqlite.SQLiteDatabase;

public class DatabaseManager {

    private DatabaseHelper dbHelper;
    private Context context;
    private SQLiteDatabase database;

    public DatabaseManager(Context c) {
        context = c;
    }

    public DatabaseManager open() {
        dbHelper = new DatabaseHelper(context);
        database = dbHelper.getWritableDatabase();
        return this;
    }

    public void insert(String name, String password) {

        ContentValues contentValues = new ContentValues();

        contentValues.put(DatabaseHelper.USER_NAME, name);
        contentValues.put(DatabaseHelper.USER_PASSWORD, password);

        database.insert(DatabaseHelper.TABLE_NAME, null, contentValues);
    }

    public Cursor fetch() {

        String[] columns = {
                DatabaseHelper.USER_ID,
                DatabaseHelper.USER_NAME,
                DatabaseHelper.USER_PASSWORD
        };

        return database.query(
                DatabaseHelper.TABLE_NAME,
                columns,
                null,
                null,
                null,
                null,
                null
        );
    }
}
```

## OUTPUT

<img width="1834" height="948" alt="image" src="https://github.com/user-attachments/assets/c1d88bce-dd95-412b-8f5f-56e3e8e8d94e" />
<img width="1811" height="948" alt="image" src="https://github.com/user-attachments/assets/42824f4b-5b2c-45d2-a059-35325a097537" />



## RESULT
Thus a Simple Android Application create a firebase database and to display the employee details using Firbase Real Time Database in Android Studio is developed and executed successfully.
