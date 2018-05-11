
<p align="left">
    <b><a href="https://github.com/katzer/cordova-plugin-printer/blob/example/README.md">SAMPLE APP</a> :point_right:</b>
</p>

Cordova Print Plugin [![npm version](https://badge.fury.io/js/cordova-plugin-printer.svg)](http://badge.fury.io/js/cordova-plugin-printer) [![Build Status](https://travis-ci.org/katzer/cordova-plugin-printer.svg?branch=master)](https://travis-ci.org/katzer/cordova-plugin-printer)
====================

[Cordova]框架的插件，从iOS、Android和Windows通用应用程序中打印HTML。

<p align="center">
    <img width="23.8%" src="https://github.com/katzer/cordova-plugin-printer/blob/example/images/print-ios.png"></img>
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
    <img width="26.8%" src="https://github.com/katzer/cordova-plugin-printer/blob/example/images/print-windows.png"></img>
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
    <img width="23.8%" src="https://github.com/katzer/cordova-plugin-printer/blob/example/images/print-android.png"></img>
</p>

### About Apple AirPrint
AirPrint苹果™技术可以帮助你创建高质量的打印输出,而不需要下载或安装驱动程序。AirPrint是建立在许多打印机型号从最流行的打印机制造商。在你的本地网络上选择一个AirPrint打印机来打印你最喜欢的iOS或OS X应用程序。

See [Drawing and Printing Guide for iOS][ios_guide] for detailed informations. 

### About Android Printing Framework
从_KitKat_开始，大多数Android设备都安装了打印服务插件，可以使用谷歌云打印和谷歌驱动服务进行打印。虽然谷歌云打印服务插件也可以用来打印任何打印机类型和型号，但从其他打印机制造商的打印服务插件可以通过应用程序商店获得。
除了支持物理打印机外，还可以将打印输出保存到您的谷歌驱动器帐户或本地作为Android设备上的PDF文件。

See [Building Apps with Multimedia for Android][android_guide] for detailed informations. 


## Supported Platforms
- iOS 8 or newer
- Android KitKat or newer
- Universal Windows Platform


## Installation
Install the latest version:

    cordova plugin add cordova-plugin-printer

Or a specific version:

    cordova plugin add cordova-plugin-printer@VERSION

Or the latest dev version:

    cordova plugin add https://github.com/katzer/cordova-plugin-printer.git

Or a custom version:

    cordova plugin add cordova-plugin-printer --searchpath path/to/plugin

And then execute:

    cordova build


## ChangeLog
#### Version 0.7.4 (not yet released)
- Fix broken _check_ and _pick_ on Android N and above

#### Version 0.7.3 (19.12.2016)
- Fixed incompatibility with Android KitKat (4.4)

Known limitations
- Plugin crashes on Windows OS 10.0.14
- _check_ on Android might return empty result as some versions of cordova seems to have a bug with multipart results.

See [CHANGELOG.md][changelog] to get the full changelog for the plugin.


## Usage
在*deviceready*事件被触发之前，插件及其方法是不可用的。

```javascript
document.addEventListener('deviceready', function () {
    // cordova.plugins.printer is now available
}, false);
```

### Check printer
他的打印能力可以通过“打印机”来检查。检查”界面。使用此功能可以将打印功能隐藏在无法使用的用户身上。

```javascript
/**
 * Checks if the printer service is available (iOS)
 * or if printer services are installed and enabled (Android).
 *
 * @param {Function} callback
 *      A callback function
 * @param {Object} scope
 *      Optional scope of the callback
 *      Defaults to: window
 */
cordova.plugins.printer.check(function (available, count) {
    alert(available ? 'Found ' + count + ' services' : 'No');
});
```

### Pick a printer
显示系统界面，允许用户选择可用的打印机。
要与打印机直接对话，您需要通过“printer.pick”来选择它们，以了解网络地址。

注意，windows平台不支持选择打印机。

```javascript
/**
 * Displays system interface for selecting a printer.
 *
 * @param {Function} callback
 *      A callback function
 */
cordova.plugins.printer.pick(function (uri) {
    alert(uri ? uri : 'Canceled');
});
```

### Print content

内容可以通过“打印机”发送到打印机。打印”界面。该方法使用带有HTML内容的字符串，一个指向另一个web页面或任何DOM节点的URI。

```javascript
/**
 * Sends the content to print service.
 *
 * @param {String} content
 *      HTML string or DOM node
 *      if latter, innerHTML is used to get the content
 * @param {Object} options
 *       Options for the print job
 * @param {Function} callback
 *      An optional callback function
 * @param {Object} scope
 *      An optional scope of the callback
 *      Defaults to: window
 */
cordova.plugins.printer.print('<html>..</html>', { duplex: 'long' }, function (res) {
    alert(res ? 'Done' : 'Canceled');
});
```

该方法接受一个属性列表。不是所有的平台和每个打印机都支持!

| Name | Description | Type | Platform |
|:---- |:----------- |:----:| --------:|
| name | 打印作业和文档的名称 | String | all |
| duplex | Specifies the duplex mode to use for the print job.<br>Either double-sided on short site (duplex:'short'), double-sided on long site (duplex:'long') or single-sided (duplex:'none').<br>Defaults to: 'none' | String | all |
| landscape| The orientation of the printed content, portrait or landscape.<br>Defaults to: false | Boolean | all |
| graystyle | If your application only prints black text, setting this property to _true_ can result in better performance in many cases.<br>Defaults to: false | Boolean | all |
| printerId | The network URL to the printer. | String | iOS |
| border | Set to _false_ to skip any border. Useful for fullscreen images.<br>Defaults to: true | Boolean | iOS |
| hidePageRange | Set to _true_ to hide the control for the page range.<br>Defaults to: false | Boolean | iOS |
| hideNumberOfCopies | Set to _true_ to hide the control for the number of copies.<br>Defaults to: false | Boolean | iOS |
| hidePaperFormat | Set to _true_ to hide the control for the paper format.<br>Defaults to: false | Boolean | iOS |
| bounds | The Size and position of the print view<br>Defaults to: [40, 30, 0, 0] | Array | iPad |

#### Further informations
- All CSS rules needs to be embedded or accessible via absolute URLs in order to print out HTML encoded content.
- The string can contain HTML content or an URI pointing to another web page.


## Examples
__NOTE:__ All CSS rules needs to be embedded or accessible via absolute URLs in order to print out HTML encoded content.

Print the whole HTML page:

```javascript
var page = location.href;

cordova.plugins.printer.print(page, 'Document.html');
```

Print the content from one part of the page:

```javascript
var page = document.getElementById('legal-notice');

cordova.plugins.printer.print(page, 'Document.html');
```

Print some custom content:

```javascript
var page = '<h1>Hello Document</h1>';

cordova.plugins.printer.print(page, 'Document.html');
```

Print a remote web page:

```javascript
cordova.plugins.printer.print('http://blackberry.de', 'BB10');
```

Send to printer directly:

```javascript
cordova.plugins.printer.pick(function (uri) {
    cordova.plugins.printer.print(page, { printerId: uri });
});
```


## Contributing

1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request

## License

This software is released under the [Apache 2.0 License][apache2_license].

Made with :yum: from Leipzig

© 2016 [appPlant GmbH][appplant]


[cordova]: https://cordova.apache.org
[ios_guide]: http://developer.apple.com/library/ios/documentation/2ddrawing/conceptual/drawingprintingios/Printing/Printing.html
[android_guide]: https://developer.android.com/training/building-multimedia.html
[changelog]: CHANGELOG.md
[check]: #check-printer
[pick]: #pick-a-printer
[print]: #print-content
[apache2_license]: http://opensource.org/licenses/Apache-2.0
[appplant]: www.appplant.de
