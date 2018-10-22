# Ti.TelephonyUtils

This is a Titanium module for handling some stuff with telephone.

## Usage

```javascript
var Tel = require("de.appwerft.telephonyutils");

var res = Tel.hasAllPermissions();
console.log(res);

Tel.callNumber("+494060812460"); // calls a number
Tel.isSpeakerphoneOn();
Tel.finishCall();
Tel.toggleLoudspeaker();
```

The module needs three permissions. These are runtime permissions. For convenience you can use a wrapper:

```javascript
require("lib/permissions").requestPermissions(["MODIFY_PHONE_STATE","CALL_PHONE","READ_PHONE_STATE"],function(){
	if (arguments[0]==true) {
		Tel.callNumber("+494060812460"); // calls a number
		Tel.isSpeakerphoneOn();
		Tel.finishCall();
		Tel.toggleLoudspeaker();
	}
});

```

### Content of `permissions.js` is:

```javascript
exports.requestPermissions = function(_permissions, _callback) {
	if (Ti.Platform.osname != 'android') {
		_callback(true);
		return;
	}
	var permissions = (Array.isArray(_permissions) ? _permissions : [_permissions]).map(function(perm) {
		return (perm.match(/^android\.permission\./)) ? perm : 'android.permission.' + perm;
	});
	var grantedpermissions = 0;
	permissions.forEach(function(perm, i) {
		if (Ti.Android.hasPermission(perm))
			grantedpermissions++;
		if (grantedpermissions == permissions.length)
			_callback(true);
	});
	if (grantedpermissions < permissions.length) {
		Ti.Android.requestPermissions(permissions, function(_e) {
			_callback(_e.success);
		});
	}
};
```

