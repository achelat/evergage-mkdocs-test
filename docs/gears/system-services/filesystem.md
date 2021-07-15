---
path: '/gears/system-services/filesystem'
title: 'Filesystem Service'
tags: ['gears', 'services', 'filesystem', 'csv', 'xml']
---
The filesystem service provides read and write access to a virtualized filesystem provided by Interaction Studio. The filesystem contains the SFTP directories where feed files are uploaded. These files can be accessed by any Gear component that includes the filesystem service such as ETL. For documentation on all available methods see the [Filesystem API Typedoc](http://evergage-gears-docs.s3-website-us-east-1.amazonaws.com/core/interfaces/filesystem.html).

## File Writer
Writing files is accomplished by calling `filesystem.writeFile("/path/filename")` which will return a [FileWriter](http://evergage-gears-docs.s3-website-us-east-1.amazonaws.com/core/interfaces/filewriter.html) pointing to the specified file. If the file already exists, it will be overwritten unless the `append` parameter is set to `true` as in `filesystem.writeFile("/path/filename", true)`. 

#### Example
```typescript
export class FileWriterExample implements EntityJob<User> {
    writer: FileWriter;

    begin(context: EntityJobContext) {
        this.writer = context.services.filesystem.writeFile('/users.txt');
        this.writer.writeLine('user ids');
    }

    transform(context: EntityJobContext, user: User) {
        this.writer.writeLine(user.id);
    }
}
```

## File Reader
Similarly, calls to `filesystem.readFile("/path/filename")` will return a [FileLineReader](http://evergage-gears-docs.s3-website-us-east-1.amazonaws.com/core/interfaces/filelinereader.html). Lines can then be iterated over by calling `hasNext()` and `readLine()` on the FileLineReader. Wrappers are provided for parsing CSV and XML file formats.

### CSV
To parse a CSV file and obtain a record iterator use `context.services.csv.readFile("/path/filename")` which will return a [CsvReader](http://evergage-gears-docs.s3-website-us-east-1.amazonaws.com/core/interfaces/csvreader.html). This is especially useful when writing jobs that return a record iterator from their extract phase such as ETL.

### XML
The XML wrapper works in the same way. Calling `context.services.xml.readFile("/path/filename")` returns an [XmlReader](http://evergage-gears-docs.s3-website-us-east-1.amazonaws.com/core/interfaces/xmlreader.html).

#### Example
```typescript
export class ActivityImportETL implements EtlJob<{ [s: string]: any; }> {
    static readonly FILENAME_REGEX = /^userActivity-.+\.csv(\.[a-z]+)?$/;

    getFilesToProcess(context: EtlJobContext): File[] {
        let files = context.services.filesystem.listFiles('/');
        return files
            .filter(file => {
                return ActivityImportETL.FILENAME_REGEX.test(file.path);
            });
    }

    extract(context: EtlJobContext, file: File): EvgIterator<{ [p: string]: any }> {
        return context.services.csv.readFile(file)
            .separator(',')
            .recordIterator();
    }

    transform(context: EtlJobContext, record: { [p: string]: any }): void {
        let statEvent: ExternalStatEvent = {
            userId: record['userId'],
            campaignAction: record['campaignAction'],
            externalCampaignId: record['campaignId'],
            externalCampaignType: "Email",
            statEventDate: record['time']
        };
        context.stageStatEvent(statEvent);
    }
}
```
