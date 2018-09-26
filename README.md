# zalego1
registering new user
package com.example.lt.interview;

import android.app.ProgressDialog;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.text.TextUtils;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.Toast;

import com.android.volley.Request;
import com.android.volley.RequestQueue;
import com.android.volley.Response;
import com.android.volley.VolleyError;
import com.android.volley.toolbox.StringRequest;
import com.android.volley.toolbox.Volley;

import java.util.HashMap;
import java.util.Map;

import static com.example.lt.interview.R.id.button;

public class MainActivity extends AppCompatActivity {
    EditText firstname,lastname,username,password;
    Button register;
    EditText login;


    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        //declare variables
        firstname=(EditText)findViewById(R.id.firstname);
        lastname=(EditText)findViewById(R.id.lastname);
        username=(EditText)findViewById(R.id.username);
        password=(EditText)findViewById(R.id.password);



    }
        //on click register button
    public void Register(View view) {
        //coolect input from the user
        String fname=firstname.getText().toString().trim();
        String lname=lastname.getText().toString().trim();
        String uname=username.getText().toString().trim();
        String pword=password.getText().toString().trim();

        //check if the textboxes are empty
        if (TextUtils.isEmpty(fname)) {
            firstname.setError("Firstname");
        }
        else if(TextUtils.isEmpty(lname)) {
            lastname.setError("Lastname");
        }
        else if(TextUtils.isEmpty(uname)) {
            username.setError("username");
        }
        else if(TextUtils.isEmpty(pword)) {
            password.setError("password");
        }
        else {
            //register function
                InsertData(fname,lname,uname,pword);
        }




    }

    private void InsertData(final String fname, final String lname, final String uname, final String pword) {
        final ProgressDialog dialog = new ProgressDialog(this);
        dialog.setMessage("Please wait... Sending message");
        dialog.setCancelable(false);
        dialog.show();

        final String url = new Url().getUrl() + "register.php";

        StringRequest stringRequest = new StringRequest(Request.Method.POST, url,
                new Response.Listener<String>() {
                    @Override
                    public void onResponse(String response) {
                        dialog.dismiss();



                        if (response.equals("Successfully Inserted")) {


                            firstname.setText("");
                            lastname.setText("");
                            username.setText("");
                            password.setText("");


                        } else if (response.equals("Could not Insert")) {
                            Toast.makeText(MainActivity.this, response, Toast.LENGTH_LONG).show();
                        } else {
                            Toast.makeText(MainActivity.this, "There was an unknown error", Toast.LENGTH_LONG).show();
                        }
                    }
                },
                new Response.ErrorListener() {
                    @Override
                    public void onErrorResponse(VolleyError error) {
                        dialog.dismiss();
                        Toast.makeText(MainActivity.this, error.toString(), Toast.LENGTH_LONG).show();
                    }
                }) {
            @Override
            protected Map<String, String> getParams() {
                Map<String, String> params = new HashMap<String, String>();
                params.put("fname", fname);
                params.put("lname", lname);
                params.put("uname",uname);
                params.put("pword",pword);


                return params;
            }

        };

        RequestQueue requestQueue = Volley.newRequestQueue(this);
        requestQueue.add(stringRequest);
    }

    }
