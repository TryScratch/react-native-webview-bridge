# here are the list of item needed to be added to each js for android and ios

### `index.ios.js` & `index.android.js`

#### variables rename

`RCTWebView` -> `RCTWebViewBridge`

`WebViewState` -> `WebViewBridgeState`

`var RCT_WEBVIEW_REF = 'webview';` -> `var RCT_WEBVIEWBRIDGE_REF = 'webviewbridge';`

`RCTWebViewManager` -> `WebViewBridgeManager`

`var WebView = React.createClass({` -> `var WebViewBridge = React.createClass({`



#### remove all import and replace it with this

> android might have some other things. for example merge and does not have ActivityIndicatorIOS

```js
var React = require('react-native');
var invariant = require('invariant');
var keyMirror = require('keymirror');

var {
  ActivityIndicatorIOS,
  EdgeInsetsPropType,
  StyleSheet,
  Text,
  View,
  requireNativeComponent,
  PropTypes,
  NativeModules: {
    WebViewBridgeManager
  }
} = React;
```


#### inside PropTypes

```js
/**
 * Will be called once the message is being sent from webview
 */
onBridgeMessage: PropTypes.func,
```

#### add the following right before render

```js
var onBridgeMessage = (event: Event) => {
  const onBridgeMessageCallback = this.props.onBridgeMessage;
  if (onBridgeMessageCallback) {
    const messages = event.nativeEvent.messages;
    messages.forEach((message) => {
      onBridgeMessageCallback(message);
    });
  }
};
```

#### add the onBridgeMessage prop to RCTWebViewBridge inside render method

```js
<RCTWebViewBridge
  ...
  onBridgeMessage={onBridgeMessage}/>
```


#### add a brand new method to the component right after `reload`

```js
sendToBridge: function (message: string) {
  WebViewBridgeManager.sendToBridge(this.getWebViewBridgeHandle(), message);
},
```

#### rename `getWebViewHandle` to `getWebViewBridgeHandle`
