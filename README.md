# tubuler 🤙

<p align="center">
  <img src="resources/images/tubuler.png" width="600">
</p>

## What Is tubuler?

tubuler is a free and open source application for downloading YouTube videos, based on the [youtube-dl](http://ytdl-org.github.io/youtube-dl/) command line tool. The application is built on [Electron](https://www.electronjs.org/) with front-end components provided by [Bulma](https://bulma.io/). Under the hood is a [Flask](https://flask.palletsprojects.com/en/1.1.x/) back-end with youtube-dl.

tubuler was built with *accessibility*, *simplicity* and *openness* in mind. youtube-dl is one of the many great free and open source tools out there. As a command line tool however, youtube-dl poses a barier for users who might not be familiar with the CLI. tubuler was made to change that.

Pronouncing *tubuler* is similar to *tubular*, a term which was used by surfers and skaters in the 80's.

## How to Use

Using tubuler is easy: simply paste the YouTube video link into the app and click download. tubuler automatically chooses the best video and audio quality. The download time varies depending on the length of the video and its quality at which it was uploaded. tubuler can also work on other [video streaming sites](https://ytdl-org.github.io/youtube-dl/supportedsites.html).

## Getting tubuler

[Download tubuler for macOS](https://github.com/dtcrout/tubuler/releases/download/v1.0.0/tubuler.zip)

tubuler is currently available for macOS. Builds for Windows and Linux may be available in the near future. You can also download this repo and build tubuler for your platform of choice (see [Building tubuler](#Building-tubuler)).

## Development

### Requirements

* [Node.js](https://nodejs.org/en/)
* [Python 3](https://www.python.org/)

### Getting Started

To get a development environment up and running, clone this repo and run the following steps:

1. `npm install`
    - Installs Node.js dependencies
2. `npm run deps`
    - Creates Python virtual environment and installs dependencies
3. `npm run babel`
    - Transcribes source files
4. `cp -r node_modules/bulma/css .`
    - Copies over Bulma CSS files for front-end
5. `npm start`
    - Starts tubuler

### How It Works

tubuler is split into two different parts: the Electron front-end and the Flask back-end. Running the app starts the Flask app in the background. When the user submits a URL, a POST request is made to the back-end which triggers the video download to the user's selected location.

#### Front-end

The front-end code is split into two directories:

* `src/`: Development source files (all development happens here)
* `lib/`: Source code used by Electron

JS development adheres to [ES6](https://en.wikipedia.org/wiki/ECMAScript) to standardize development. When running or building tubuler, [Babel](https://babeljs.io/) is ran first to transcribe the files in `src/` to the files in `lib/`. Looking into `package.json` can give more clarity into the development workflow.

#### Back-end

All Python source files are in the `src/download_video/` directory. The Flask app can be ran independently by running the following command in the root of the tubuler:

```
$ ./venv/bin/python src/download_video/app.py
```

Once the app is running, we can use [cURL](https://en.wikipedia.org/wiki/CURL) to call the Flask app:

```
$ curl -X POST -H "Content-type: application/json" \
               -H "Accept: application/json" \
               -d '{"url": "[youtube_video_url]", "ydl_opts": {"outtmpl": "[path]/video.mp4"}}' "localhost:5000/download_video"
```

where `[youtube_video_url]` is the link to the video you wish to download and `[path]` is a directory on your machine where you want to save the video.

Before running the Flask app by itself, we need to create a Python virtual environment first by running `npm run deps`.  This creates a virtual environment `venv/` where all Python dependencies are contained. The back-end is developed in Python using [PEP-8](https://www.python.org/dev/peps/pep-0008/) formatting.

### Building tubuler

To build tubuler, the Flask app needs to be made into an executable. To start, we need to comment/uncomment lines so we use the executable instead of the Python code. In `src/pythonProcess.js`, the beginning of `createPythonProcess()` should look like the following:

```
// Run app using Python
// const script = path.join(__dirname, 'download_video', 'app.py');
// pyProcess = spawn('./venv/bin/python', [script]);

// Run app executable
const script = path.join(__dirname, 'download_video_dist', 'app', 'app');
pyProcess = execFile(script);
```

Once that's done, we can build the app by running `npm run build`. This will create a directory containing the standalone tubuler app.

## Feedback

If you have any ideas on how to make tubuler better or if you're having issues, feel free to contribute [here](https://github.com/dtcrout/tubuler/issues).

## Stack

* [Babel](https://babeljs.io/)
* [Bulma](https://bulma.io/)
* [Electron](https://www.electronjs.org/)
* [Flask](https://flask.palletsprojects.com/en/1.1.x/)
* [PyInstaller](https://www.pyinstaller.org/)
* [youtube-dl](http://ytdl-org.github.io/youtube-dl/)

## License

[MIT](LICENSE)

tubuler is free and open source.
