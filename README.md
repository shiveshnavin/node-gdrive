# node-gdrive

Upload file to google drive

## Installation

With [npm](https://www.npmjs.com/) do:

``` sh
npm install node-gdrive
```

Use command line global:

``` sh
npm install node-gdrive -g
```

## Config

``` sh
gdrive token:get # generator new token
```

## Usage

```
const fs = require("fs");
const Drive = require("node-gdrive");


console.log('Make sure to run node node_modules/node-gdrive/bin/gdrive token:get')


var folderid = process.argv[2] || "0BzGsb5NmvVQsOTRtZEpiaEROOUk";
if (!fs.existsSync(folderid))
    fs.mkdirSync(folderid)

Drive
    .list(folderid)
    .then(file => {
        console.log("file", file);

        let files = "echo Download Started for "+file.length+" files.\n"
        let i=0;
        file.forEach(element => {
            files = files + `gdrive download ${element.id} -o ${folderid}/${element.name} \n echo -- ${++i}/${file.length} done\n`
        });

        fs.writeFileSync(`${folderid}.sh`, files)
        // console.log(files)

    })
    .catch(err => {
        console.error("err", err);
    });
    
  ```

### Node project

``` js
// upload file

const Drive = require("node-gdrive");

let filePath = 'foo.txt';

var upload = Drive.upload(filePath, {share: true});
upload.on('*', (event, data) => {
  console.log(event, data);
});

// Download file
let fileId = "1eoAgH8xgBkkUDXkTdyPSHSbaJViv33oX";

Drive
  .download(fileId, {
    resumable: true, // Resume download session
    force: false, // Override file if exists
    output: './tmp/', // Dir or filepath to save file
  })
  .then(file => {
    console.log("file", file);
  })
  .catch(err => {
    console.error("err", err);
  });

```

#### Commands line

``` sh
gdrive upload file.txt --share user@gmail.com # upload and share file

gdrive donwload 1eoAgH8xgBkkUDXkTdyPSHSbaJViv33oX # download file

gdrive --help
```

## License

MIT
