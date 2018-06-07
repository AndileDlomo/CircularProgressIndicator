# CircularProgressIndicator

Simple but customizable view for displaying progress

<img src="art/1_1.png" width="30%" /> <img src="art/1_2.png" width="30%" /> <img src="art/1_3.png" width="30%" />


##### With custom colors
<img src="art/colors_1.png" width="30%" /> <img src="art/colors_2.png" width="30%" />


##### With or without dot
<img src="art/dot_yes.png" width="30%" /> <img src="art/dot_no.png" width="30%" />


##### With custom progress text (more examples [here](#formatting-progress-text))
<img src="art/prefix_yes.png" width="30%" /> <img src="art/suffix_yes.png" width="30%" /> <img src="art/prefix_suffix.png" width="30%" />


## How to use

Add view to your layout:
```xml
<antonkozyriatskyi.circularprogressindicator.CircularProgressIndicator
    android:id="@+id/circular_progress"
    android:layout_width="200dp"
    android:layout_height="200dp"
    android:layout_gravity="center"
    android:layout_marginBottom="8dp"
    android:layout_marginTop="8dp"
    app:progressColor="@color/colorPrimary"
    app:progressBackgroundColor="#efefefef"
    app:progressStrokeWidth="8dp"
    app:drawDot="true"
    app:dotColor="@color/colorAccent"
    app:dotWidth="16dp"
    app:textSize="24sp"
    app:textColor="@color/colorPrimaryDark" />
```

Since all attributes have default values, you can specify none of them. Thus following code also works:
```xml
<antonkozyriatskyi.circularprogressindicator.CircularProgressIndicator
    android:id="@+id/circular_progress"
    android:layout_width="200dp"
    android:layout_height="200dp"
    android:layout_gravity="center"
    android:layout_marginBottom="8dp"
    android:layout_marginTop="8dp" />
```


Than find it in code and set progress:
```java
CircularProgressIndicator circularProgress = findViewById(R.id.circular_progress);

// you can set max and current progress values individually
circularProgress.setMaxProgress(10000);
circularProgress.setCurrentProgress(5000);
// or all at once
circularProgress.setProgress(5000, 10000);

// you can get progress values using following getters
circularProgress.getProgress() // returns 5000
circularProgress.getMaxProgress() // returns 10000
```

#### Attributes
|  Description                                                                                                                                        | XML                               | Java                                                                                                                                                    | Default value                 |
|  -------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------- |
|  Progress color                                                                                                                                     | `app:progressColor`               | setter: `setProgressColor(color)`<br>getter: `getProgressColor()`                                                                                       | `#3f51b5`                     |
|  Progress background  color                                                                                                                         | `app:progressBackgroundColor`     | setter: `setProgressBackgroundColor(color)`<br>getter: `getProgressBackgroundColor()`                                                                   | `#e0e0e0`                     |
|  Width of progress stroke                                                                                                                           | `app:progressStrokeWidth`         | setters: `setProgressStrokeWidthDp(widthInDp)` or `setProgressStrokeWidthPx(widthInPx)`<br>getter: `getProgressStrokeWidth()` (returns width in pixels) | `8dp`                         |
|  Whether to draw dot. `true` or `false`                                                                                                             | `app:drawDot`                     | setter: `setShouldDrawDot(shoulDrawDot)`<br>getter: `isDotEnabled()`                                                                                    | `true`                        |
|  Dot color                                                                                                                                          | `app:dotColor`                    | setter: `setDotColor(dotColor)`<br>getter: `getDotColor()`                                                                                              | same as progress color        |
|  Dot width                                                                                                                                          | `app:dotWidth`                    | setters: `setDotWidthDp(widthInDp)` or `setDotWidthPx(widthInPx)`<br>getter: `getDotWidth()` (returns width in pixels)                                  | same as progress stroke width |
|  Progress text size                                                                                                                                 | `app:textSize`                    | setters: `setTextSizeSp(sizeInSp)` or `setTextSizePx(sizeInPx)`<br>getter: `getTextSize()` (returns size in pixels)                                     | `24sp`                        |
|  Progress text color                                                                                                                                | `app:textColor`                   | setter: `setTextColor(textColor)`<br>getter: `getTextColor()`                                                                                           | same as progress color        |
|  ~~Whether to use delimiter or not. `true` or `false`~~.<br/> Deprecated and will be removed in next release.<br/> Use ProgressTextAdapter instead. | `app:useProgressTextDelimiter`    | setter: `setShouldUseDelimiter(shouldUseDelimiter)`<br>getter: `isTextDelimiterEnabled()`                                                               | `true`                        |
|  ~~The delimiter to use in progress text~~.<br/> Deprecated and will be removed in next release.<br/> Use ProgressTextAdapter instead.              | `app:progressTextDelimiter`       | setter: `setProgressTextDelimiter(delimiter)`<br>getter: `getProgressTextDelimiter()`                                                                   | `,`                           |
|  ~~Prefix for progress text~~.<br/> Deprecated and will be removed in next release.<br/> Use ProgressTextAdapter instead.                           | `app:progressTextPrefix`          | setter: `setProgressTextPrefix(prefix)`<br>getter: `getProgressTextPrefix()`                                                                            | `null` (disabled)             |
|  ~~Suffix for progress text~~.<br/> Deprecated and will be removed in next release.<br/> Use ProgressTextAdapter instead.                           | `app:progressTextSuffix`          | setter: `setProgressTextSuffix(suffix)`<br>getter: `getProgressTextSuffix()`                                                                            | `null` (disabled)             |
|  Formatting pattern to be used in `PatternProgressTextAdapter`. Checkout [Formatting progress text](#formatting-progress-text) section.             | `app:formattingPattern`           |                                                                                                                                                         | not specified                 |

---

#### Formatting progress text
Before version 1.0.5 formatting text was limited to specifying prefix, suffix and delimiter. Since version 1.1 you have full control over the text to be displayed (thanks to the [repitch](https://github.com/repitch) for the idea and implementation).
So `app:useProgressTextDelimiter`, `app:progressTextDelimiter`, `app:progressTextPrefix`, `app:progressTextSuffix` attributes are now deprecated and not taken into account.

The only thing you have to do is to provide an implementation of `CircularProgressIndicator.ProgressTextAdapter`.
You must override it's `String formatText(double currentProgress);`.
Library already provides you with two implementations:
* [DefaultProgressTextAdapter](circularprogressindicator/src/main/java/antonkozyriatskyi/circularprogressindicator/DefaultProgressTextAdapter.java) that just returns string representation of the progress value cast to `int`.
* [PatternProgressTextAdapter](circularprogressindicator/src/main/java/antonkozyriatskyi/circularprogressindicator/PatternProgressTextAdapter.java) that uses pattern specified in `formattingPattern` attribute to format progress (if it is set, otherwise it fallbacks to the `DefaultProgressTextAdapter`).

##### Custom formatters' examples

`DefaultProgressTextAdapter` is a default text adapter, which is used when no `formatterPattern` attribute set or `progressIndicator.setProgressTextAdapter(null)` method was called.
It's implementation looks like this:
```java
    @Override
    public String formatText(double currentProgress) {
        return String.valueOf((int) currentProgress);
    }
```


`PatternProgressTextAdapter` supports formatting progress using Java's [formatting patterns](https://docs.oracle.com/javase/7/docs/api/java/util/Formatter.html).
For example, if you want to display progress with two digits for the fractional part, you can use `%.2f` pattern and set it via

- attribute:   
   `app:formattingPattern="@string/pattern"` that refers to a string resource<br/>`<string name="pattern">%.2f</string>`<br/>
   If you don't want to use string resource - you have to use unicode for percent sign (`&#37;`) instead of sign itself:<br/>
   `app:formattingPattern="&#37;.2f"`
    
- or `PatternProgressTextAdapter`:
   `circularProgress.setProgressTextAdapter(new PatternProgressTextAdapter("%.2f"));`
   
Here is the example of `TimeTextAdapter` by [repitch](https://github.com/repitch) that displays text in format of 'HH.MM.SS'
```java
private static final CircularProgressIndicator.ProgressTextAdapter TIME_TEXT_ADAPTER = new CircularProgressIndicator.ProgressTextAdapter() {
        @Override
        public String formatText(double time) {
            int hours = (int) (time / 3600);
            time %= 3600;
            int minutes = (int) (time / 60);
            int seconds = (int) (time % 60);
            StringBuilder sb = new StringBuilder();
            if (hours < 10) {
                sb.append(0);
            }
            sb.append(hours).append(":");
            if (minutes < 10) {
                sb.append(0);
            }
            sb.append(minutes).append(":");
            if (seconds < 10) {
                sb.append(0);
            }
            sb.append(seconds);
            return sb.toString();
        }
    };
```

If you don't want any text to be displayed - just create your own `ProgressTextAdapter` that returns an empty string.

---

### Download using Gradle

Add this in your root `build.gradle` at the end of `repositories` in `allprojects` section:
```groovy
allprojects {
    repositories {
        maven { url 'https://jitpack.io' }
    }
}
```

Then add this dependency to your **module-level** `build.gradle` in `dependencies` section:
```groovy
implementation 'com.github.antonKozyriatskyi:CircularProgressIndicator:1.1.0'
```

If you have any troubles downloading the library - checkout [issue#5](https://github.com/antonKozyriatskyi/CircularProgressIndicator/issues/5)

---

### License

```
 Copyright 2018 Anton Kozyriatskyi

   Licensed under the Apache License, Version 2.0 (the "License");
   you may not use this file except in compliance with the License.
   You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

   Unless required by applicable law or agreed to in writing, software
   distributed under the License is distributed on an "AS IS" BASIS,
   WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
   See the License for the specific language governing permissions and
   limitations under the License.
```
