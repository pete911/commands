# remap keyboard keys

Remapping keyboard keys is done by [`hidutil`](https://developer.apple.com/library/archive/technotes/tn2450/_index.html)
command. Mapping will not be persisted between reboots, to make it persistent:
- create `~/Library/LaunchAgents/com.local.KeyRemapping.plist` file with appropriate mapping (you can use https://hidutil-generator.netlify.app/ to generate the mapping)
- load it `launchctl load ~/Library/LaunchAgents/com.local.KeyRemapping.plist`
- verify it was loaded successfully `launchctl list | grep KeyRemapping`
- start mapping `launchctl start com.local.KeyRemapping`
- verify mapping is running `hidutil property --get "UserKeyMapping"`

Launchctl just runs the content `hidutil property --set '{"UserKeyMapping":[<mappings>]}'`
e.g. if we set ESC key to caps lock, and tilda to ESC key, launchctl just runs -
`hidutil property --set '{"UserKeyMapping":[{"HIDKeyboardModifierMappingSrc": 0x700000029, "HIDKeyboardModifierMappingDst": 0x700000035}, {"HIDKeyboardModifierMappingSrc": 0x700000039, "HIDKeyboardModifierMappingDst": 0x700000029}]}'`

## links
- key map generator - https://github.com/amarsyla/hidutil-key-remapping-generator
- key map generator source - https://hidutil-generator.netlify.app/
- hdutil technical note https://developer.apple.com/library/archive/technotes/tn2450/_index.html
- blog https://rakhesh.com/mac/using-hidutil-to-map-macos-keyboard-keys/
