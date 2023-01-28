# Compose
## Gradle
В Gradle уровня Module добавляем
```
android {
    buildFeatures {
        compose true
    }

    composeOptions {
        kotlinCompilerExtensionVersion = "1.3.2"
    }
}
```
И dependencies
```
// Compose
def composeBom = platform('androidx.compose:compose-bom:2022.10.00')
implementation composeBom
androidTestImplementation composeBom

implementation 'androidx.compose.material3:material3'

implementation 'androidx.compose.ui:ui-tooling-preview'
debugImplementation 'androidx.compose.ui:ui-tooling'

androidTestImplementation 'androidx.compose.ui:ui-test-junit4'
debugImplementation 'androidx.compose.ui:ui-test-manifest'

implementation 'androidx.activity:activity-compose:1.5.1'
implementation 'androidx.lifecycle:lifecycle-viewmodel-compose:2.5.1'
implementation 'androidx.compose.runtime:runtime-livedata'

```
## Фрагмент
```
import android.os.Bundle
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import androidx.compose.material3.Text
import androidx.compose.runtime.Composable
import androidx.compose.ui.platform.ComposeView
import androidx.fragment.app.Fragment

class MainFragment : Fragment() {

    override fun onCreateView(
        inflater: LayoutInflater,
        container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View {
        val view = ComposeView(requireContext())

        view.setContent {
            MainFragmentBody()
        }

        return view
    }
}

@Composable
fun MainFragmentBody() {
    Text(text = "Hello")
}
```
