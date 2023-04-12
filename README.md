# frida-ios-dump
A tool to extract a decrypted IPA from a jailbroken, **rootless** device.

## Usage

1. Install [frida](http://www.frida.re/) on your device.
   You have two options:
   - Add [my repo](https://miticollo.github.io/repos/#my)
   - Compile it yourself. 
     Refer to the dedicated [gist](https://gist.github.com/miticollo/6e65b59d83b17bacc00523a0f9d41c11) for more information.
2. Clone this project.
   ```shell
   git clone --depth=1 -j8 https://github.com/miticollo/frida-ios-dump.git
   cd frida-ios-dump/
   ```
3. Run `sudo pip install -r requirements.txt --upgrade`
4. Use `iproxy` to enable SSH forwarding over USB (e.g. `iproxy -ddd 2222:22`).
5. On the device, install `curl`, `ldid` and `openssh` from Procursus. 
   Then, run the following commands as **root** either over SSH or in a terminal window:
   ```shell
   curl -LO --output-dir /var/tmp/ 'https://raw.githubusercontent.com/miticollo/frida-ios-dump/master/scp.entitlements'
   ldid -S/var/tmp/scp.entitlements -M "$(which scp)"
   rm -v /var/tmp/scp.entitlements
   ```
   <span><!-- https://discord.com/channels/349243932447604736/1082886572011180053/1092577566008807494 --></span>
6. **Open the target app on the device.**
7. Run `./dump.py <target>`

For SSH/SCP, make sure to add your public key to the `~/.ssh/authorized_keys` file on the target device.

```
./dump.py Spotify 
Start the target app Spotify
Dumping Spotify to /var/folders/q2/x23bcyr53w3dnmlh2fqjp2mr0000gp/T
start dump /private/var/containers/Bundle/Application/56AE666E-0F06-4969-91C8-5B63F33ECF58/Spotify.app/Spotify
Spotify.fid: 100%|██████████| 112M/112M [00:03<00:00, 35.5MB/s]
start dump /private/var/containers/Bundle/Application/56AE666E-0F06-4969-91C8-5B63F33ECF58/Spotify.app/Frameworks/SpotifyShared.framework/SpotifyShared
SpotifyShared.fid: 100%|██████████| 4.26M/4.26M [00:00<00:00, 19.8MB/s]
AppIntentVocabulary.plist: 125MB [00:10, 13.1MB/s]
Generating "Spotify.ipa"
0.00B [00:00, ?B/s]
```

Congratulations!!! You've got a decrypted IPA file.

## Support

Python 2.x and 3.x

## Tested environment

- iPhone XR with iOS 15.1b1 jailbroken using [Fugu15 Max](https://github.com/opa334/Fugu15/releases/tag/1.0.0-beta.5)
- [Python3](https://github.com/pyenv/pyenv)
- Spotify
