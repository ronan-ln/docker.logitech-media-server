# service.logitech-media-server
Logitech media server addon for Libreelec

The addon uses the [doliana/logitech-media-server](https://github.com/DOliana/docker-image-logitech-media-server) and adds the possibility to broadcast Kodi's audio output to Logitech media server devices (with this PR https://github.com/DOliana/docker-image-logitech-media-server/pull/5)

# Dockerfile changes
Installs pulseaudio-utils for parecord
  
# How it works
  * Load modules to enable Pulseaudio communication through UNIX socket and a new sink to output Kodi's audio
  * Run docker container for Logitech Media Server sharing the pusleaudio socket

# Extra steps to stream Kodi audio
  * Install the WaveInput plugin from Logitech Media Server settings
  * Update the custom-convert.conf file for WaveInput to work with Pulseaudio bins
    * SSH into the runnign container
    * Update `/srv/squeezebox/cache/InstalledPlugins/Plugins/WaveInput/custom-convert.conf` with this content
```
#
# wavin
#
wavin pcm * *
	# R
	[parec] --channels=2 --format=s16le --rate=44100 -d $FILE$
wavin mp3 * *
	# RB:{BITRATE=-B %B}
	[parec] -d $FILE$  | [lame] --silent -q $QUALITY$ -v $BITRATE$ - -
wavin flc * *
	# R
	[parec] -d $FILE$ | [flac] --endian=little --sign=signed --sample-rate=44100 --bps=16 --channels=2 --silent - -
```

  * Add a favourite with URL `wavin:squeezebox.monitor`
  * In Kodi's audio settings use the relevant Pulseaudio output




	

