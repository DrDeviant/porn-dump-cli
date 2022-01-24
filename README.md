# porn-dump-cli

Stream capture / data dump CLI for biggest adult streaming platforms to prove their lack of security and privacy concerns.

> This project is still mainly a `WIP` so many things might change in the future.

## Origins

Originaly made because a simple `PoC` would never cause real security issues for big companies behind the biggest adult streaming platforms... I wanted to make something a little more powerfull and better structured.

So, here comes __porn-dump-cli__. It takes its origines from [stream-capture-poc](https://github.com/DgSe95/stream-capture-poc).

## Install

Simply run the following commands:

```
git clone https://github.com/DgSe95/porn-dump-cli.git
cd porn-dump-cli
npm install
```

You might also need to install `ffmpeg`:

```bash
# CentOS / RedHat / Rocky Linux
sudo yum install ffmpeg

# Ubuntu based distribs
sudo apt install ffmpeg
```

## Supported platforms

Here is the list of supported platforms and future ones:

* [X] Stripchat and their _white label_ websites (e.g. xHamsterLive, MyCamTV, and so on...)
* [ ] Chaturbate
* [ ] Cam4

## Features

What it can do:

* Make screenshots of the user/model page ([source](cli.js#L1139)):

  ```js
  await makeScreenshot(page, parsedModelName);
  ```

* Dump several details from the discovered API ([source](cli.js#L1147)):

  ```js
  // Dump stream settings from JSON
  // This stream is the only one found that has some protection like user-agent checks
  // TODO: bypass checks to make results similar to those gathered from real browser
  await dumpJSON(dumpStreamSettingsURL(), `${outputDir}/${parsedModelName}.cam.json`);

  // Dump model intro from JSON
  await dumpJSON(dumpModelIntroURL(), `${outputDir}/${parsedModelName}.intro.json`);

  // Dump model info from JSON
  await dumpJSON(dumpModelInfoURL(), `${outputDir}/${parsedModelName}.json`);

  // Dump site config from JSON
  await dumpJSON(dumpSiteConfigURL(), `${outputDir}/site-config.json`);

  // Dump site settings from JSON
  await dumpJSON(dumpSiteSettingsURL(), `${outputDir}/site-settings.json`);

  // Dump all displayed models on site from JSON
  await dumpJSON(dumpHomepageModelsURL(), `${outputDir}/site-models.json`);

  // Dump all connected models to JSON
  await dumpJSON(dumpOnlineModelsURL(), `${outputDir}/online-models.json`);
  ```

* Replay the user/model stream only ([source](cli.js#L1184)):

  ```js
  await playStream(streamURL);
  ```

* Record the user/model stream only ([source](cli.js#L1189)):

  ```js
  await recordStreamMP4(streamURL, parsedModelName);
  await recordStreamMKV(streamURL, parsedModelName);
  ```

* Record and replay the user/model stream ([source](cli.js#L1203)):

  ```js
  await recordAndPlayStreamMP4(streamURL, parsedModelName);
  await recordAndPlayStreamMKV(streamURL, parsedModelName);
  ```

## Usage

* To get the help: `./cli.js -h` | `./cli.js --help`

  ```
  Adult streaming websites data dumper / recorder [WiP]

  Usage: cli.js <options>

  Supported: stripchat, xhamsterlive, mycamtv
  Planned: chaturbate, cam4 and many more!

  Analyze:
    -u, --url             The URL to look fer                                            [string] [requi-yar-ed]
    -s, --screenshot      Make screenshot of the given URL                                             [boolean]
        --remote          Dump data from remote service (Using wss://chrome.browserless.io)            [boolean]
        --remote-api-key  API key for remote service (browserless.io)                                   [string]

  Dump:
        --dump-all, --da               Dump all possible data from website                             [boolean]
        --dump-config, --dc            Dump website config                                             [boolean]
        --dump-settings, --ds          Dump website settings                                           [boolean]
        --dump-model-info, --dmi       Dump model infos                                                [boolean]
        --dump-homepage-models, --dhm  Dump homepage models                                            [boolean]
        --dump-online-models, --dom    Dump online models                                              [boolean]

  Record an' play:
    -p, --play                     Play given stream URL                                               [boolean]
    -r, --record                   Record given stream URL                                             [boolean]
        --preview                  Preview recorded stream (no need to set `--record-time`)            [boolean]
        --record-time              Recordin' time (minutes)                                             [number]
        --keep-recording           Keep recordin' when preview window be closed (default)              [boolean]
        --stop-recording-on-close  Avast recordin' when preview window be closed                       [boolean]
        --hwaccel                  Enable 'ardware acceleration                                        [boolean]
        --vaapi                    Use VAAPI acceleration                                              [boolean]
        --nvidia                   Use NVIDIA acceleration                                             [boolean]

  Output:
    -f, --format  Recording format (mkv,mp4)                                           [string] [default: "mp4"]
    -o, --output  Define where to store dumped data                              [string] [default: "./samples"]

  Options for me hearties!
    -h, --help     Parlay this here code of conduct                                                    [boolean]
    -v, --version  'Tis the version ye be askin' fer                                                   [boolean]

  Ex. marks the spot:

    cli.js --url https://stripchat.com/model-name

  Mabe with some THC by DgSe95 \m/

  PS: Yes, I speak Pirate! :P
  ```

* To play the dumped stream:

  ```
  ./cli.js --url https://stripchat.com/model-name --play
  ```

* To record the dumped stream:

  ```
  ./cli.js --url https://stripchat.com/model-name --record --record-time 10
  ```

* To record and preview the dumped stream:

  ```bash
  # Keep recording until preview is closed
  ./cli.js --url https://stripchat.com/model-name --record --preview --stop-recording-on-close

  # Keep recording until defined time is reached even if preview window is closed
  ./cli.js --url https://stripchat.com/model-name --record --record-time 10 --preview

  # Keep recording until defined time is reached but stop if preview window is closed
  ./cli.js --url https://stripchat.com/model-name --record --record-time 10 --preview --stop-recording-on-close
  ```

* To dump known `JSON` data only:

  ```
  ./cli.js --url https://stripchat.com/model-name --dump-all
  ```

* To dump and record everything possible:

  ```
  ./cli.js --url https://stripchat.com/model-name --screenshot --record --preview --stop-recording-on-close --dump-all
  ```

### Output folder

By default, it will create a folder named `samples` at the project root but you specify any other path that way:

```
./cli.js [other arguments] --output /path/to/whatever/you/want
```

### Recording formats

By default, the `mp4` format will be used but you can also use the `mkv` format that way:

```
./cli.js [other arguments] --format mkv
```

### Hardware acceleration

Some user/models are using HD cams in 720p or 1080p, this might be very demanding for your CPU and in this case you would need help from your GPU.

Two hardware acceleration types are supported for now:

1. VAAPI
2. NVIDIA

To use them, simply add the following to your commands:

```bash
# For using VAAPI
./cli.js [other arguments] --hwaccel --vaapi

# For using NVIDIA
./cli.js [other arguments] --hwaccel --nvidia
```

## Disclaimer

This project has been made in the hope that biggest adult streaming platforms will make sure to really protect the identity and then privacy of their users and models.

I consider to not be responsible about your usage of this project at the moment the code don't break anything in the target websites and only use their lack of security in their API and streaming platform implementation.

Any script kiddies with some browsers and web console knowledge and at least a software like `vlc` or `ffmpeg` is litterally able to dump everything themselves without using this project.

Thus, this project is just a way to make things easier and less repetitive but there is no _real magic_ behind.

## Credits

Author: [@DgSe95](https://twitter.com/DgSe95)
