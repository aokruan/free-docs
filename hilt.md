# Внедрение `Dagger Hilt`  

Добавить зависимости `build.gradle(:app)`:  
```kotlin
plugins {
    id 'com.android.application'
    id 'kotlin-android'
    id 'kotlin-android-extensions'
    id 'kotlin-kapt'
    id 'dagger.hilt.android.plugin'
}

...

//Dagger - Hilt
implementation "com.google.dagger:hilt-android:2.38.1"
kapt "com.google.dagger:hilt-android-compiler:2.37"
implementation "androidx.hilt:hilt-lifecycle-viewmodel:1.0.0-alpha03"
kapt "androidx.hilt:hilt-compiler:1.0.0"
```

Добавить класс `Application` в корень.  
```kotlin
package ru.aokruan.adbs

import android.app.Application
import dagger.hilt.android.HiltAndroidApp

@HiltAndroidApp
class AdbsApplication : Application() {}
```

Добавить запись в `AndroidManifest.xml`:  
```xml
<uses-permission android:name="android.permission.INTERNET"/>
...
<application
    android:name=".AdbsApplication"
...
```

Добавить каталог `di`.
```kotlin
package ru.aokruan.adbs.di

import android.content.Context
import com.bumptech.glide.Glide
import com.bumptech.glide.load.engine.DiskCacheStrategy
import com.bumptech.glide.request.RequestOptions
import dagger.Module
import dagger.Provides
import dagger.hilt.InstallIn
import dagger.hilt.android.qualifiers.ApplicationContext
import dagger.hilt.components.SingletonComponent
import retrofit2.Retrofit
import retrofit2.converter.gson.GsonConverterFactory
import ru.aokruan.adbs.MainRemoteData
import ru.aokruan.adbs.R
import ru.aokruan.adbs.Service
import javax.inject.Singleton

@Module
@InstallIn(SingletonComponent::class)
object AppModule {

    @Singleton
    @Provides
    fun provideGlideInstance(@ApplicationContext context: Context) = Glide.with(context).setDefaultRequestOptions(
        RequestOptions()
            .placeholder(R.drawable.profile_picture)
            .error(R.drawable.ic_launcher_background)
            .diskCacheStrategy(DiskCacheStrategy.DATA)
    )

    @Provides
    fun providesBaseUrl(): String = "https://api.myip.com"

    @Provides
    @Singleton
    fun provideRetrofit(BASE_URL: String): Retrofit = Retrofit.Builder()
        .addConverterFactory(GsonConverterFactory.create())
        .baseUrl(BASE_URL)
        .build()

    @Provides
    @Singleton
    fun provideMainService(retrofit: Retrofit): Service = retrofit.create(Service::class.java)

    @Provides
    @Singleton
    fun provideMainRemoteData(mainService: Service): MainRemoteData = MainRemoteData(mainService)
}
```

Добавить аннотации и внедрение в `MainActivity.kt`:  
```kotlin
@AndroidEntryPoint
class MainActivity : ComponentActivity() {

    @Inject
    lateinit var glide:RequestManager
    
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
    }
}
```

Добавить модель ответа от сервера:  
```kotlin
package ru.aokruan.adbs

import com.google.gson.annotations.SerializedName

data class MyInfo(
    @SerializedName("cc") val cc: String,
    @SerializedName("country") val country: String,
    @SerializedName("ip") val ip: String
)
```

Добавить сервис, в котором будут храниться параметры запросов:  
```kotlin
package ru.aokruan.adbs

import retrofit2.Response
import retrofit2.http.GET

interface Service {
    @GET("/")
    suspend fun getUser(): Response<MyInfo>
}
```

Добавить промежуточную абстракцию связывающую репозиторий и сервис:  
```kotlin
package ru.aokruan.adbs

import javax.inject.Inject

class MainRemoteData @Inject constructor(private val mainService: Service) {
    suspend fun getUser(userId: Int) = mainService.getUser()
}
```

Добавить репозиторий для связи c `ViewModel` c промежуточной абстракцией:  
```kotlin
package ru.aokruan.adbs

import javax.inject.Inject
import javax.inject.Singleton

@Singleton
class MainRepository @Inject constructor(private val remoteData: MainRemoteData) {
    suspend fun getUser(userId: Int) = remoteData.getUser(userId)
}
```

Добавить в `MainActivity.kt` запрос на получение информации из `ViewModel`:  
```kotlin
package ru.aokruan.adbs

import android.content.res.Configuration
import android.os.Bundle
import android.util.Log
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.activity.viewModels
import androidx.compose.material.MaterialTheme
import androidx.compose.runtime.*
import com.bumptech.glide.RequestManager
import dagger.hilt.android.AndroidEntryPoint
import ru.aokruan.adbs.ui.theme.AdbsTheme
import javax.inject.Inject

@AndroidEntryPoint
class MainActivity : ComponentActivity() {
    private val viewModel by viewModels<MainViewModel>()

    @Inject
    lateinit var glide: RequestManager

    override fun onCreate(savedInstanceState: Bundle?) {

        super.onCreate(savedInstanceState)
        viewModel.getUser(0).observe(this, { data ->
            Log.e("TAG", "Ip address: " + data.ip)
        })
    }
}
```
