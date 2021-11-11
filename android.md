## Создание файла с текущим временем
```kotlin
    /**
     * Create a [File] named a using formatted timestamp with the current date and time.
     *
     * @return [File] created.
     */
    private fun createFile(context: Context, extension: String): File {
        val sdf = SimpleDateFormat("yyyy_MM_dd_HH_mm_ss_SSS", Locale.US)
        return File(context.filesDir, "IMG_${sdf.format(Date())}.$extension")
    }
```

## Получение координат фотографии
```kotlin
val exif:ExifInterface = ExifInterface(imagesResult[0].value!!.path!!) // URI
val lat = exif.getAttribute(ExifInterface.TAG_GPS_LATITUDE)
val lng = exif.getAttribute(ExifInterface.TAG_GPS_LONGITUDE)
val latlng = exif.latLong
```
