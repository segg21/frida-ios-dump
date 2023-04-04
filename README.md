# frida-ios-dump
A tool to extract a decrypted IPA from a jailbroken, **rootless** device.

## Usage

1. Install [frida](http://www.frida.re/) on your device.
   You have two options:
   - Add [my repo](https://miticollo.github.io/repos/#my)
   - Compile it yourself. 
     Refer to the dedicated [gist](https://gist.github.com/miticollo/6e65b59d83b17bacc00523a0f9d41c11) for more information.
2. Run `sudo pip install -r requirements.txt --upgrade`
3. Use `iproxy` to enable SSH forwarding over USB (e.g. `iproxy -ddd 2222:22`).
4. On the device, install `curl` and `ldid` from Procursus. 
   Then, run the following commands as **root** either over SSH or in a terminal window:
   ```shell
   curl -LO --output-dir /var/tmp/ 'https://raw.githubusercontent.com/miticollo/frida-ios-dump/master/scp.entitlements'
   ldid -S/var/tmp/scp.entitlements "$(which scp)"
   rm -v /var/tmp/scp.entitlements
   ```
   <span><!-- https://discord.com/channels/349243932447604736/1082886572011180053/1092577566008807494 --></span>
5. **Open the target app on the device.**
6. Run `./dump.py <target>`

For SSH/SCP, make sure to add your public key to the `~/.ssh/authorized_keys` file on the target device.

```
./dump.py Spotify 
Start the target app Spotify
Dumping Spotify to /var/folders/q2/x23bcyr53w3dnmlh2fqjp2mr0000gp/T
[frida-ios-dump]: SpotifyShared.framework has been loaded. 
start dump /private/var/containers/Bundle/Application/64798BA1-EBD8-4090-9AF7-4CAC093B450B/Spotify.app/Spotify
Spotify.fid: 100%|██████████| 112M/112M [00:03<00:00, 34.3MB/s]
0.00B [00:00, ?B/s]start dump /private/var/containers/Bundle/Application/64798BA1-EBD8-4090-9AF7-4CAC093B450B/Spotify.app/Frameworks/SpotifyShared.framework/SpotifyShared
SpotifyShared.fid: 100%|██████████| 4.26M/4.26M [00:00<00:00, 19.7MB/s]
AppIntentVocabulary.plist: 125MB [00:09, 13.9MB/s]
Generating "Spotify.ipa"
0.00B [00:00, ?B/s]
```

Congratulations!!! You've got a decrypted IPA file.

## Support

Python 2.x and 3.x

## Tested environment

- iPhone XR with iOS 15.1b1 jailbroken using [Fugu15 Max](https://github.com/opa334/Fugu15/releases/latest)
- [Python3](https://github.com/pyenv/pyenv)
- Spotify
