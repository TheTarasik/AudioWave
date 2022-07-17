# AudioWave
[View demo](https://thetarasik.github.io/audio-wave/)

Using a few library:
  - [audiowaveform](https://thetarasik.github.io/AudioWave/)
  - [waveform-data](https://www.npmjs.com/package/waveform-data)

To generate own amplitude you must download audiowaveform under you OS.

Open terminal and paste this code:
```
audiowaveform.exe -i <music-file-path> -o track.json -b 8 -z 256
```

music-file-path - path to audio file

After the track.json was successfully created open this and paste array to index.html -> arr (variable)

### Server part
```
const transformAudio = () => {
    return new Promise((resolve, reject) => {
        exec('audiowaveform.exe -i <music-file-path> -o track.json -b 8 -z 256', (err, stdout, stderr) => {
            if (err) {
                return reject({
                  status: false
                });
            }
            resolve({
                status: true
            })
        });
    })
};

transformAudio()
    .then((data) => {
        if (data.status) {
            fs.readFile(path.join(<music-file-path>), {encoding: 'utf-8'}, function(err,data){
                const waveform = WaveformData.create(JSON.parse(data));
                const resampledWaveform = waveform.resample({ width: 500 });
                const channel = resampledWaveform.channel(0);
                console.log(JSON.stringify(channel.max_array()));
            });
        }
    })
    .catch((e) => {
        console.log(e);
    })
```
