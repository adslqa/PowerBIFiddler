# PowerBIFiddler
PowerBIFiddler is a custom [Fiddler](http://www.telerik.com/fiddler) inspector extension that allows you to view Power BI metadata and tile refresh HTTP request payloads in plain text.

By default, Power BI sends tile data compressed and base64 encoded. It is difficult to debug - especially when you have a real-time dashboard and data is quicking streaming in.  Without PowerBIFiddler, you'd have to copy that data and manually decode and decompress it using a custom built tool. This extension automatically decodes and decompresses it for you and displays it in a new Fiddler Inspector response tab.

![](images/inspector.png)

The encoded and compressed version looks like this:
```
tileDataBinaryBase64Encoded=H4sIAAAAAAAEAE2OPQuDQAyG/0tmh2tLl1t1qLRCQehSOoS7gAfnKbk4iPjfG6UFyfR+POFdwFN2HEYZGOwCLUVyAva9wD0kD/ZcwAvjRGChMbB+1gJ6EvQoqH1VPu9gpUbb4Uh5h2tFoWoNFPDk0CPPt0CM7Lr5kDdbXqcsmNwPLDG6KaKEIR0+7cX/jpO5mstj26KnfC6HfowkmglPpO76BbPzFb3XAAAA
```

The new and improved decoded and decompressed version looks like this:

```
tileData={"descriptor":{"Select":[{"Kind":2,"Value":"M0"}]},"metadata":{},"dsr":{"DataShapes":[{"Id":"DS0","PrimaryHierarchy":[{"Id":"DM0","Instances":[{"Calculations":[{"Id":"M0","Value":"10503L"}]}]}],"IsComplete":true}]}}
```

## How to Install
### MSI
Run the PowerBIFiddler.msi found on the [releases page](https://github.com/jonbgallant/PowerBIFiddler/releases). It will copy the ```PowerBIFiddler.dll``` and ```Newtonsoft.Json.dll``` files to your local ```C:\Users\{user}\Documents\Fiddler2\Inspectors``` directory.

> If you use Edge or IE, it may tell you that the MSI is corrupt or invalid because I haven't signed it with a valid cert.  You can choose to accept the warning and install or use the Zip install option below.

### Zip
Unzip the PowerBIFidder.7z file found on the [releases page](https://github.com/jonbgallant/PowerBIFiddler/releases) to your local ```C:\Users\{user}\Documents\Fiddler2\Inspectors``` directory.

Make sure you have [.NET 4.6.1](https://www.microsoft.com/en-us/download/details.aspx?id=49981) installed.

## How to Use
PowerBIFiddler supports two types of requests: metadata and tile refresh.

### Metadata
When you first hit [PowerBI.com](https://app.powerbi.com) it will make a metadata request to get all the metadata required to render Power BI based on your preferences and previous state. This request also contains the tile data from the dashboard you had open when you last closed Power BI. That tile data is also compressed and base64 encoded.

The metadata url is ```/powerbi/metadata/app?dashboardObjectId={guid}```

![](images/metadata.png)

When you navigate that JSON payload with PowerBIFiddler you will see a child "dashboards" node and the default dashboard will have a "tiles" node, which contains the new decompressed and decoded tileData property.

![](images/metadatatiles.png)

### Tile Refresh
When you setup a real-time dashboard, Power BI will send a "subscribe" HTTP request and will get a "tiles" data response.

![](images/fiddlersessions.png)

When you click on the "tiles" response you will see the new tileData property in the new "Power BI" inspector tab.
![](images/inspector.png)


## Development
 - If you want to build compile the installer project, you'll need to install the [Visual Studio 2015 Installer Project Plugin](https://visualstudiogallery.msdn.microsoft.com/f1cc3f3e-c300-40a7-8797-c509fb8933b9).  If you don't want to compile the installer, then just ignore the unsupported project type warning when you load the solution.
 - The Fiddler.exe and Standard.dll references might not be mapped in VS correctly. If that is the case, then remove and re-add the references from ```C:\Program Files (x86)\Fiddler2\Fiddler.exe``` and ```C:\Program Files (x86)\Fiddler2\Inspectors\Standard.dll```  