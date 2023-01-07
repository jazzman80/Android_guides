# View Animation
## XML Animation
Анимация выносится в xml файл в папке res/animator
```
<set xmlns:android="http://schemas.android.com/apk/res/android"
    android:ordering="sequentially">
    <set android:ordering="together">
        <objectAnimator
            android:duration="200"
            android:propertyName="scaleX"
            android:valueTo="2.0F"
            android:valueType="floatType" />
        <objectAnimator
            android:duration="200"
            android:propertyName="scaleY"
            android:valueTo="2.0F"
            android:valueType="floatType" />
    </set>
    <set android:ordering="together">
        <objectAnimator
            android:duration="500"
            android:propertyName="scaleX"
            android:valueTo="1.0F"
            android:valueType="floatType" />
        <objectAnimator
            android:duration="500"
            android:propertyName="scaleY"
            android:valueTo="1.0F"
            android:valueType="floatType" />
    </set>
```
Во фрагменте прописываем запуск анимации
```
        val animator = AnimatorInflater.loadAnimator(
            requireContext(),
            R.animator.animation_scale
        ) as AnimatorSet

        animator.setTarget(binding.button)
        animator.interpolator = BounceInterpolator()
        animator.start()
```
## animate()
Любая view имеет билдер animate()
```
binding.textView.animate()
                .scaleX(2.0f)
                .scaleY(2.0f)
                .setDuration(1000L)
                .withEndAction {
                    binding.textView.animate()
                        .scaleX(1.0f)
                        .scaleY(1.0f)
                        .setDuration(500L)
                        .start()
                }.start()
```
## ObjectAnimator
Позволяет анимировать любые атрибуты
```
        val scaleX = PropertyValuesHolder.ofFloat("scaleX", 2.0f)
        val scaleY = PropertyValuesHolder.ofFloat("scaleY", 2.0f)

        val objectAnimator = ObjectAnimator.ofPropertyValuesHolder(binding.button, scaleX, scaleY)
        objectAnimator.addListener(
            object : AnimatorListener {

                override fun onAnimationEnd(animation: Animator) {
                    val scaleX2 = PropertyValuesHolder.ofFloat("scaleX", 1.0f)
                    val scaleY2 = PropertyValuesHolder.ofFloat("scaleY", 1.0f)
                    ObjectAnimator.ofPropertyValuesHolder(binding.button, scaleX2, scaleY2)
                        .setDuration(500L)
                        .start()
                }

                override fun onAnimationCancel(animation: Animator) = Unit
                override fun onAnimationStart(animation: Animator) = Unit
                override fun onAnimationRepeat(animation: Animator) = Unit
            }
        )
        objectAnimator.setDuration(200L).start()
```
## Animator set
Позволяет связывать ObjectAnimator в последовательности и добавлять интерполяции
```
        val scaleX = ObjectAnimator.ofFloat(binding.button, "scaleX", 2.0f).setDuration(200L)
        val scaleY = ObjectAnimator.ofFloat(binding.button,"scaleY", 2.0f).setDuration(200L)
        val scaleX2 = ObjectAnimator.ofFloat(binding.button,"scaleX", 1.0f).setDuration(500L)
        val scaleY2 = ObjectAnimator.ofFloat(binding.button,"scaleY", 1.0f).setDuration(500L)

        AnimatorSet().apply{
            play(scaleX).with(scaleY)
            play(scaleX2).with(scaleY2)

            play(scaleX2).after(scaleX)
            interpolator = BounceInterpolator()
            start()
        }
```
Список интерполяторов https://thoughtbot.com/blog/android-interpolators-a-visual-guide
## Value Animator
Позволяет анимировать любые значения    
На примере появления текста
```
        val newText = "Animated"

        val animator = ValueAnimator.ofFloat(0f,1f)
        animator.addUpdateListener { valueAnimator ->
            binding.textView.text = newText.subSequence(
                startIndex = 0,
                endIndex = (valueAnimator.animatedFraction * newText.length).toInt(),
            )
        }
        animator.setDuration(1000L).start()
```

