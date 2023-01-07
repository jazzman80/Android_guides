# View Animation
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
