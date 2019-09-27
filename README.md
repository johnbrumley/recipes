# recipes

help me remember commands

## fluid synth

convert midi to wav (to mp3) using a sf2.

```console
user@commputer:~$ fluidsynth -F output.wav my-font.sf2 my-midi.mid  | sox -t wav /dev/stdout output.mp3
```

should work but seems like it isn't. Thought that pipe to sox using stdout (and specifying type with -t) would get the conversion going.

maybe run em all separate

```console
user@commputer:~$ fluidsynth -F output.wav my-font.sf2 my-midi.mid
user@commputer:~$ sox output.wav output.mp3
```

## strip audio with ffmpeg

```console
user@commputer:~$ ffmpeg -i my-vid.mp4 -f mp3 -ab 192000 -vn audio-output.mp3
```

the -ab flag probably not necessary unless you really wanna set the rate

## grab audio online

```console
user@commputer:~$ youtube-dl -x --audio-format "mp3" http://my-url.com
```

## convert midi type0 to type1

need to install ruby midi stuff http://www.goodeveca.net/midifile_rb/

I probably didn't install it the right way, but here's how I'm running it. I also may have had to modify the SMFformat code to get it working.

```console
user@commputer:~$ ~/MIDIFILE_RB/SMFformat1 type0-midi.mid type1-midi.mid
```

## chop up long sound into many smalls

analyze file by "silences", split into separate files ,, use for generating datasets of audio

```console
user@commputer:~$ sox -V3 ../long-sound-file.mp3 split-file-.mp3 silence 1 0.3 3% 1 0.3 2% : newfile : restart
```

usually need to adjust the start and end silence values to get good results. It depends on how much noise floor is going on.

## fix problem with unity video texture (old way) without installing quicktime

basically convert video files to ogg

```console
user@commputer:~$ ffmpeg -i vid.mov -acodec libvorbis -vcodec libtheora -f ogv vid.ogv
```

## update web stuff via rsync

problem was needing to ssh over a weird port

```console
user@commputer:~$ rsync -avh -e "ssh -p 21098" ./files/to/sync  username@website.com:/home/username/path/to/dir
```

## set up dns redirect using openwrt

To `/etc/dnsmasq.conf` add:

```conf
address=/#/192.168.1.1
```

This will cause everything to point back to the router's ip (use different ip if router has a different one)

Then add a redirect page to the uhttpd server in `/www/index.html`, probably this is a nice thing to do:

```console
mv /www/index.html /www/index.html.original
```

In the new `index.html` use something like this:

```html
<!DOCTYPE html>
<html>
<head>
    <meta http-equiv="refresh" content="0; url=http://host-name:port" />
</head>
<body></body>
</html>
```
