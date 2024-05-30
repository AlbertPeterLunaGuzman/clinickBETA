# clinickBETA


package com.example.sqlconnectionsandbox;

import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.TextView;
import android.widget.Toast;

import androidx.appcompat.app.AppCompatActivity;

import com.android.volley.Request;
import com.android.volley.RequestQueue;
import com.android.volley.Response;
import com.android.volley.VolleyError;
import com.android.volley.toolbox.StringRequest;
import com.android.volley.toolbox.Volley;

import org.json.JSONArray;
import org.json.JSONException;
import org.json.JSONObject;

public class MainActivity extends AppCompatActivity {

    private String fetchUrl = "http://192.168.1.3/test/test.php";

    private TextView resultTextView;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        Button connectButton = findViewById(R.id.connectToPhpButton);
        resultTextView = findViewById(R.id.resultTextView);

        connectButton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                connectToDatabase();
            }
        });
    }

    private void connectToDatabase() {
        RequestQueue queue = Volley.newRequestQueue(this);

        StringRequest stringRequest = new StringRequest(Request.Method.GET, fetchUrl,
                new Response.Listener<String>() {
                    @Override
                    public void onResponse(String response) {
                        try {
                            if (response.trim().isEmpty()) {
                                Toast.makeText(MainActivity.this, "Empty response", Toast.LENGTH_SHORT).show();
                                return;
                            }

                            JSONArray jsonArray = new JSONArray(response);

                            displayData(jsonArray);
                        } catch (JSONException e) {
                            e.printStackTrace();
                            Toast.makeText(MainActivity.this, "Error parsing JSON", Toast.LENGTH_SHORT).show();
                        }
                    }
                }, new Response.ErrorListener() {
            @Override
            public void onErrorResponse(VolleyError error) {
                Toast.makeText(MainActivity.this, "Network error", Toast.LENGTH_SHORT).show();
            }
        });

        queue.add(stringRequest);
    }

    private void displayData(JSONArray jsonArray) {
        StringBuilder stringBuilder = new StringBuilder();
        try {
            for (int i = 0; i < jsonArray.length(); i++) {
                JSONObject jsonObject = jsonArray.getJSONObject(i);
                stringBuilder.append("User ID: ").append(jsonObject.getString("userID")).append("\n");
                stringBuilder.append("Username: ").append(jsonObject.getString("userName")).append("\n");
                stringBuilder.append("First Name: ").append(jsonObject.getString("userFirstName")).append("\n");
                stringBuilder.append("Last Name: ").append(jsonObject.getString("userLastName")).append("\n");
                stringBuilder.append("Birthdate: ").append(jsonObject.getString("userBirthdate")).append("\n");
                stringBuilder.append("Address: ").append(jsonObject.getString("userAddress")).append("\n");
                stringBuilder.append("Email: ").append(jsonObject.getString("userEmail")).append("\n");
                stringBuilder.append("Signup Date: ").append(jsonObject.getString("userSignupDate")).append("\n");
                stringBuilder.append("Last Login Date: ").append(jsonObject.getString("userLastLoginDate")).append("\n");
                stringBuilder.append("\n\n");
            }
            resultTextView.setText(stringBuilder.toString());
        } catch (JSONException e) {
            e.printStackTrace();
            Toast.makeText(MainActivity.this, "Error parsing JSON", Toast.LENGTH_SHORT).show();
        }
    }
}



<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <Button
        android:id="@+id/connectToPhpButton"
        android:layout_width="150dp"
        android:layout_height="50dp"
        app:layout_constraintBottom_toTopOf="@+id/resultTextView"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        android:text="Connect"
        />

    <TextView
        android:id="@+id/resultTextView"
        android:layout_width="0dp"
        android:layout_height="0dp"
        android:layout_margin="16dp"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@id/connectToPhpButton"
        android:textSize="18sp"
        />

</androidx.constraintlayout.widget.ConstraintLayout>
