# Practica-Lista-Heroes
Main Activity

import android.annotation.SuppressLint;
import android.support.v7.app.AppCompatActivity;

import android.os.Bundle;

import android.widget.ArrayAdapter;

import android.widget.ListView;

import android.widget.Toast;

import java.util.List;

import retrofit2.Call;

import retrofit2.Callback;

import retrofit2.Response;

import retrofit2.Retrofit;

import retrofit2.converter.gson.GsonConverterFactory;


@SuppressLint("Registered")
class MainActivity extends AppCompatActivity {



    ListView listView;



    @Override

    protected void onCreate(Bundle savedInstanceState) {

        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);



        listView = (ListView) findViewById(R.id.listViewHeroes);



        //Se llama al método que deslpliega a los 'Heroes'

        getHeroes();

    }



    private void getHeroes() {

        Retrofit retrofit = new Retrofit.Builder()

                .baseUrl(Api.BASE_URL)

                .addConverterFactory(GsonConverterFactory.create()) //Aquí usamos el GsonConverterFactory para convertir directamente la data en formato JSON a Objetos

                .build();



        Api api = retrofit.create(Api.class);



        Call<List<Hero>> call = api.getHeroes();



        call.enqueue(new Callback<List<Hero>>() {

            @Override

            public void onResponse(Call<List<Hero>> call, Response<List<Hero>> response) {

                List<Hero> heroList = response.body();



                //Se crea un Array de tipo String para el ListView

                String[] heroes = new String[heroList.size()];



                //Proceso cíclico de los heroes que los inserta dentro del array

                for (int i = 0; i < heroList.size(); i++) {

                    heroes[i] = heroList.get(i).getName();

                }





                //Despliega el Arreglo de tipo String en la ListView

                listView.setAdapter(new ArrayAdapter<String>(getApplicationContext(), android.R.layout.simple_list_item_1, heroes));



            }



            @Override

            public void onFailure(Call<List<Hero>> call, Throwable t) {

                Toast.makeText(getApplicationContext(), t.getMessage(), Toast.LENGTH_SHORT).show();

            }

        });

    }



}


AndroidManifest

<?xml version="1.0" encoding="utf-8"?>

<manifest xmlns:android="http://schemas.android.com/apk/res/android"

    package="com.example.leonardo.myapplication">



    <!-- Permiso de Internet -->

    <uses-permission android:name="android.permission.INTERNET" />



    <application

        android:allowBackup="true"

        android:icon="@mipmap/ic_launcher"

        android:label="@string/app_name"

        android:roundIcon="@mipmap/ic_launcher_round"

        android:supportsRtl="true"

        android:theme="@style/AppTheme">

        <activity android:name=".MainActivity">

            <intent-filter>

                <action android:name="android.intent.action.MAIN" />



                <category android:name="android.intent.category.LAUNCHER" />

            </intent-filter>

        </activity>

    </application>


activity_main

<?xml version="1.0" encoding="utf-8"?>

<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"

    xmlns:app="http://schemas.android.com/apk/res-auto"

    xmlns:tools="http://schemas.android.com/tools"

    android:layout_width="match_parent"

    android:layout_height="match_parent"

    tools:context=".MainActivity">

    <!--ListView Aquí creamos la lista y le asignamos un ID-->

    <ListView

        android:id="@+id/listViewHeroes"

        android:layout_width="match_parent"

        android:layout_height="match_parent" />

</RelativeLayout>
