# Lottie
## Gradle
В Gradle уровня Module добавляем
```
// Lottie
implementation 'com.airbnb.android:lottie:5.2.0'
```
## Разметка
В лэйаут пишем LottieView
## Файл анимации
Файл анимации в виде lottie_json размещаем в main/assets
## Запуск
В коде запускаем анимацию через binding
```
    binding.lottieView.setAnimation("lottie_lego.json")
    binding.lottieView.playAnimation()
    binding.lottieView.repeatCount = LottieDrawable.INFINITE
```
