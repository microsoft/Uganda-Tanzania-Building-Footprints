## Introduction
Under Microsoft’s AI for Humanitarian Action program, Bing Maps is contributing to an initiative from Humanitarian OpenStreetMap Team that will bring AI Assistance to open map building. More information around the partnership is available on [Bing Maps blog](https://blogs.bing.com/maps/2019-09/microsoft-releases-18M-building-footprints-in-uganda-and-tanzania-to-enable-ai-assisted-mapping).

Bing Maps is releasing country wide open building footprints datasets in Uganda and Tanzania. This dataset contains 17,942,345‬ computer generated building footprints. The data is freely available for download and use under applicable license. 

## License
This data is licensed by Microsoft under the [Open Data Commons Open Database License (ODbL)](https://opendatacommons.org/licenses/odbl/).

## FAQ
### What does the data include?
17,942,345‬ building footprint polygon geometries in Uganda and Tanzania in GeoJSON format. You can download the data here:

| Country       | Number of Buildings  | Unzipped MB |
| ------------- |:-------------:| -----:|
| [Uganda](https://usbuildingdata.blob.core.windows.net/tanzania-uganda-buildings/Uganda_2019-09-16.zip)|6,928,078‬|1339|
| [Tanzania](https://usbuildingdata.blob.core.windows.net/tanzania-uganda-buildings/Tanzania_2019-09-16.zip)|11,014,267‬|2202|

### What is the GeoJson format?
GeoJSON is a format for encoding a variety of geographic data structures. 
For intensive documentation and tutorials, refer to [GeoJson blog](http://geojson.org/).

### Why is the data being released?
Microsoft has a continued interest in supporting a thriving OpenStreetMap ecosystem.

### Should we import the data into OpenStreetMap?
Maybe. Never overwrite the hard work of other contributors or blindly import data into OSM without first checking the local quality. While our metrics show that this data meets or exceeds the quality of hand-drawn building footprints, the data does vary in quality from place to place, between rural and urban, mountains and plains, and so on. Inspect quality locally and discuss an import plan with the community. Always follow the [OSM import community guidelines](https://wiki.openstreetmap.org/wiki/Import/Guidelines).

### Will the data be used or made available in larger OpenStreetMap ecosystem?
Yes. Currently Microsoft Open Buildings dataset is used in ml-enabler for task creation. You can try it out at [AI assisted Tasking Manager](https://tasks-assisted.hotosm.org/). Facebook has also integrated the dataset into experimental version of RapiD editor. Try out this experimental version here [RapiD](https://mapwith.ai/rapid-sotm2019).

### What is the creation process for this data?
The building extraction is done in two stages:
1.	Semantic Segmentation – Recognizing building pixels on the aerial image using DNNs
2.	Polygonization – Converting building pixel blobs into polygons

### Stage1: Semantic Segmentation
![](/images/segmentation.jpg)

#### DNN architecture
The network backbone is EfficientNet B3 which can be found [here](https://arxiv.org/abs/1905.11946).
The model is fully-convolutional, meaning that the model can be applied to an image of any size (constrained by GPU memory, 4096x4096 in our case).

#### Training details
The training set consists of 1.2 million labeled buildings. The data is diverse in terms of geolocation, urbanization and underlying imagery, in order to attain the good corpus representativeness. We also used mixuture of high and low quality labels. Images in the set are with 30 cm/pixel resolution.

#### Pixel Metrics
These are the intermediate stage metrics we use to track DNN model improvements and they are pixel based.
Pixel precision/recall = 86.8%/81.8%.

### Stage 2: Polygonization
![](/images/polygonization.jpg)

#### Method description
We developed a method that approximates the prediction pixels into polygons making decisions based on the whole prediction feature space. This is very different from standard approaches, e.g. Douglas-Peucker algorithm, which are greedy in nature. The method tries to impose some of a priori building properties, which is, at the moment, manually defined and automatically tuned.

#### Polygon Metrics
Building matching metrics:

| Metric | Value |
| --- | :---: |
| Precision | 94.5% |
| Recall | 61.8% |

False positive ratio across the board is 1.6%.

We track various metrics to measure the quality of the output:
1. Intersection over Union – This is the standard metric measuring the overlap quality against the labels
2. Shape distance – With this metric we measure the polygon outline similarity
3. Dominant angle rotation error – This measures the polygon rotation deviation

![](/images/bldgmetrics.JPG)

The evaluation set contains 18.5k building. The metrics on the set are:
- IoU is 0.68, Shape distance is 0.39, Average rotation error is 4.1 degrees

### What is the vintage of this data?
The vintage of the footprints depends on the vintage of the underlying imagery. Bing Imagery is a composite of multiple sources, therefore it is difficult to know the exact dates for individual pieces of data.

### How good is the data?
Our metrics show that in the vast majority of cases the quality is at least as good as data hand digitized buildings in OpenStreetMap. It is not perfect, particularly in dense urban areas but it provides good recall in rural areas. See below for metrics by area type:  
![](/images/polygonmetrics.JPG)

### What is the coordinate reference system?
EPSG: 4326

### Will there be more data coming for other geographies?
Maybe. This is a work in progress.

<br>

## Contributing

This project welcomes contributions and suggestions.  Most contributions require you to agree to a
Contributor License Agreement (CLA) declaring that you have the right to, and actually do, grant us
the rights to use your contribution. For details, visit https://cla.microsoft.com.

When you submit a pull request, a CLA-bot will automatically determine whether you need to provide
a CLA and decorate the PR appropriately (e.g., label, comment). Simply follow the instructions
provided by the bot. You will only need to do this once across all repos using our CLA.

This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/).
For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or
contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.

## Legal Notices

Microsoft, Windows, Microsoft Azure and/or other Microsoft products and services referenced in the documentation
may be either trademarks or registered trademarks of Microsoft in the United States and/or other countries.
The licenses for this project do not grant you rights to use any Microsoft names, logos, or trademarks.
Microsoft's general trademark guidelines can be found [here](http://go.microsoft.com/fwlink/?LinkID=254653).

Privacy information can be found [here](https://privacy.microsoft.com/en-us/)

Microsoft and any contributors reserve all others rights, whether under their respective copyrights, patents,
or trademarks, whether by implication, estoppel or otherwise.
