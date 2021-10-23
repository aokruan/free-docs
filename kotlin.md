### Добавление маски на намер мобильного телефона  
```kotlin
fun doPhone(): String {
   val phoneNumber = "9131112222"
   val PHONE_MASK = "+{7} ([000]) [000]-[00]-[00]"

  return String.format(
    "+7 (%s) %s-%s-%s", 
    phoneNumber.substring(0, 3), 
    phoneNumber.substring(3, 6),
    phoneNumber.substring(6, 8), 
    phoneNumber.substring(8, 10))
}
```
### Перевод из `String` даты полученой из `JSON` в `DateTimeFormat`  
```gradle
implementation 'joda-time:joda-time:x.xx'
```
```kotlin
val str = "2011-03-10T11:54:30.207Z"
val parser = ISODateTimeFormat.dateTime()
val dt = parser.parseDateTime(str)
val formatter = DateTimeFormat.shortDateTime()
Log.e("Result", formatter.print(dt)) 
//10 мар. 2011 г. 18:54:30

val date = "2018-05-08 10:41"
val dateTime = DateTime.parse(date, DateTimeFormat.forPattern("yyyy-MM-dd HH:mm"))
// 2018-05-08T10:41:00.000+07:00
```
### Функция для создания строки с `prefix` и `postfix`
```kotlin
fun main(args: Array<String>) {
    println(mySuperFun("prefix | ", " | postfix"))
    println(mySuperFun("prefix | ", null))
    println(mySuperFun(null, " | postfix"))
    println(mySuperFun(null, null))
}

fun mySuperFun(prefix: String?, postfix:String? ): String? {
    val chars = "0123456789abcdefghijklmnopqrstuvwxyz"
    var output = ""
    for (i in 0..3) {
        output += chars[Math.floor(Math.random() * chars.length).toInt()]
    }
    prefix?.let { output = "$it$output" }
    postfix?.let { output = "$output$it" }

    return output
}
```
