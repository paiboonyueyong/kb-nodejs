
# How to process big file 
## read file from text file and insert to mongodb

```javascript
var MongoClient = require('mongodb').MongoClient;
var url = "mongodb://localhost:27017/";

function insertData(objArr){
    MongoClient.connect(url, function (err, db) {
        if (err) throw err;
        var dbo = db.db("mydb");
        dbo.collection("SplitterL2Report").insertMany(objArr, function (err, res) {
            if (err) throw err;
            console.log("Number of documents inserted: " + res.insertedCount);
            db.close();
        });
    });
}

function processBigFile(inputFile){
    var fs = require('fs'),
        readline = require('readline'),
        instream = fs.createReadStream(inputFile),
        outstream = new (require('stream'))(),
        rl = readline.createInterface(instream, outstream);

    var row = 0;
    var fields;
    var objArr = [];
    var total = 0;

    rl.on("line", function (line) { this.emit("pause", line); });

    rl.on("pause", function (line) {
        //console.log("pause");

        if (line!=null){
            var obj = {};
            var tabs = line.split('\t');

            if (row == 0) {
                fields = tabs;
            }

            if (row >= 1) {

                for (tab = 0; tab < tabs.length; tab++) {
                    obj[fields[tab]] = tabs[tab];
                }

                objArr.push(obj);
                total = total + 1;

                if (total == 10000)
                {
                    insertData(objArr);
                    console.log("row=" + row);
                    total = 0;
                    objArr = [];
                }
            }

            row = row + 1;
            this.emit("resume");
        }
        else
        {
            console.log("Line is null");
            if (total > 0)
            {
                insertData(objArr);
                total = 0;
                objArr = [];
            }
        }
      
    });

    rl.on("resume", function () {
        //console.log("resume");
    });

    rl.on("close", function () {
        console.log("close");
    });

}

if (process.argv.length < 3) {
    console.log('Usage: node ' + process.argv[1] + ' FILENAME');
    process.exit(1);
}

var filename = process.argv[2];
console.log("filename=" + filename);
processBigFile(filename);

```
