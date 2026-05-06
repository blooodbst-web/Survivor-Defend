# AdMob Android bridge for Survivor Defend

The game is an HTML5/Three.js app, so the browser page cannot load Google Mobile Ads directly. When the game is wrapped in an Android `WebView`, expose a JavaScript bridge named `SurvivorDefendAds` or `AndroidAdBridge` with this method:

```kotlin
@JavascriptInterface
fun showRewardedAd(payloadJson: String)
```

The page sends this JSON payload when the player taps **WATCH AD TO REVIVE**:

```json
{
  "placement": "reward_revive",
  "adUnitId": "ca-app-pub-4122791238302384/2453824433",
  "appId": "ca-app-pub-4122791238302384~1881989116"
}
```

After the native AdMob rewarded ad finishes, call one of these JavaScript callbacks from Android:

```kotlin
webView.evaluateJavascript("window.SurvivorDefendAdCallbacks.rewarded({status:'rewarded'})", null)
webView.evaluateJavascript("window.SurvivorDefendAdCallbacks.closed()", null)
webView.evaluateJavascript("window.SurvivorDefendAdCallbacks.failed('No ad available')", null)
```

Use Google's rewarded test ad unit while developing:

```text
ca-app-pub-3940256099942544/5224354917
```

Switch `ADMOB_CONFIG.useTestAds` in `index.html` to `true` for SDK testing, then back to `false` before production release.
