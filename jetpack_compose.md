# Заметки по  Jetpack compose

`!` - подлежит детальному уточнению.

`Ctrl+Shift+F5` - обновить `Preview`.

![image](https://user-images.githubusercontent.com/49877495/145712736-9f8c2686-21de-40aa-88f0-dcda0a8f4ff1.png)


`Row` - выравнивает блоки по рядам.  
```kotlin
    Row() {
        Text(text = msg.author)
        Text(text = msg.body)
    }
```

`Column` - выравнивает блоки по столбцам.  
```kotlin
    Column() {
        Text(text = msg.author)
        Text(text = msg.body)
    }
```

`Box` - для укладки элементов (`!`).
```kotlin
    Box() {
        Text(text = msg.author)
        Text(text = msg.body)
    }
```

`Resource Manager` - компонент для загрузки графики в приложение. находится на левой панели IDE.  

Изображение можно отобразить в следующем блоке:  
```kotlin
Image(
    painter = painterResource(R.drawable.profile_picture),
    contentDescription = "Contact profile picture"
)
```

Для создания различных отступов можно использовать элемент `Spacer`:
```kotlin
// Add a vertical space between the author and message texts
Spacer(modifier = Modifier.height(4.dp))

// Add a horizontal space between the image and the column
Spacer(modifier = Modifier.width(8.dp))
```

Сделать круглое изображение можно следующим способом:  
```kotlin
Image(
    painter = painterResource(R.drawable.profile_picture),
    contentDescription = "Contact profile picture",
    modifier = Modifier
        // Set image size to 40 dp
        .size(40.dp)
        // Clip image to be shaped as a circle
        .clip(CircleShape)
)
```

Добавить цвет компоненту можно таким вариантом:
```kotlin
Text(
    text = msg.body,
    style = MaterialTheme.typography.body2
)
```

Просмотр отображения приложения в `Dark Mode`:  
```kotlin
@Preview(name = "Light Mode")
@Preview(
    uiMode = Configuration.UI_MODE_NIGHT_YES,
    showBackground = true,
    name = "Dark Mode"
)
```

