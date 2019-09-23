Introduction
-------------------
This dataset contains 18,000,000 computer generated building footprints in Tanzania and Uganda. This data is freely available for download and use.

License
-------------------
This data is licensed by Microsoft under the [Open Data Commons Open Database License (ODbL)](https://opendatacommons.org/licenses/odbl/)

## FAQ
#### What the data include:
18,000,000 building footprint polygon geometries in Tanzania and Uganda in GeoJSON format.

#### What is the GeoJson format?
GeoJSON is a format for encoding a variety of geographic data structures. 
For Intensive Documentation and Tutorials, Refer to [GeoJson Blog](http://geojson.org/)

#### Creation Details:
The building extraction is done in two stages:
1.	Semantic Segmentation – Recognizing building pixels on the aerial image using DNNs
2.	Polygonization – Converting building pixel blobs into polygons
### Semantic Segmentation
![](/images/segmentation.png)


#### DNN architecture
The network backbone is EfficientNet B3 which can be found [here](https://arxiv.org/abs/1905.11946).
The model is fully-convolutional, meaning that the model can be applied to an image of any size (constrained by GPU memory, 4096x4096 in our case).

#### Training details
The training set consists of 1.2 million labeled buildings. Images in the set are with 30 cm/pixel resolution.

#### Metrics
These are the intermediate stage metrics we use to track DNN model improvements and they are pixel based.
Pixel recall/precision = 86.8%/81.8%

### Polygonization
![](/images/polygonization.PNG)

#### Method description
We developed a method that approximates the prediction pixels into polygons making decisions based on the whole prediction feature space. This is very different from standard approaches, e.g. the Douglas-Peucker algorithm, which are greedy in nature. The method tries to impose some of a priori building properties, which is, at the moment, manually defined and automatically tuned. Some of these a priori properties are:
1. The building edge must be of at least some length, both relative and absolute, e.g. 3 meters
2. Consecutive edge angles are likely to be 90 degrees
3. Consecutive angles cannot be very sharp, smaller by some auto-tuned threshold, e.g. 30 degrees
4. Building angles likely have very few dominant angles, meaning all building edges are forming an angle of (dominant angle _&pm;_ n&pi;/2)

In near future, we will be looking to deduce this automatically from existing building information.

#### Metrics
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

On our evaluation set contains ~15k building. The metrics on the set are:
- IoU is 0.68, Shape distance is 0.39, Average rotation error is 4.1 degrees

#### Data Vintage
The vintage of the footprints depends on the vintage of the underlying imagery. Because Bing Imagery is a composite of multiple sources it is difficult to know the exact dates for individual pieces of data.

#### How good are the data?
Our metrics show that in the vast majority of cases the quality is at least as good as data hand digitized buildings in OpenStreetMap. It is not perfect, particularly in dense urban areas but it is still awesome.

### What is the coordinate reference system?
EPSG: 4326

#### Will there be more data coming for other geographies?
Maybe. This is a work in progress.

#### Why is the data being released?
Microsoft has a continued interest in supporting a thriving OpenStreetMap ecosystem.

#### Should we import the data into OpenStreetMap?
Maybe. Never overwrite the hard work of other contributors or blindly import data into OSM without first checking the local quality. While our metrics show that this data meets or exceeds the quality of hand-drawn building footprints, the data does vary in quality from place to place, between rural and urban, mountains and plains, and so on. Inspect quality locally and discuss an import plan with the community. Always follow the [OSM import community guidelines](https://wiki.openstreetmap.org/wiki/Import/Guidelines).

A Vector Tile implementation of the data is hosted by [Esri](https://www.arcgis.com/home/item.html?id=f40326b0dea54330ae39584012807126)


| Country       | Number of Buildings  | Unzipped MB |
| ------------- |:-------------:| -----:|
| [Tanzania and Uganda](https://usbuildingdata.blob.core.windows.net/usbuildings-v1-1/Alabama.zip)|2,460,404|526|
<br>
<br>

# Contributing

This project welcomes contributions and suggestions.  Most contributions require you to agree to a
Contributor License Agreement (CLA) declaring that you have the right to, and actually do, grant us
the rights to use your contribution. For details, visit https://cla.microsoft.com.

When you submit a pull request, a CLA-bot will automatically determine whether you need to provide
a CLA and decorate the PR appropriately (e.g., label, comment). Simply follow the instructions
provided by the bot. You will only need to do this once across all repos using our CLA.

This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/).
For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or
contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.

# Legal Notices

Microsoft, Windows, Microsoft Azure and/or other Microsoft products and services referenced in the documentation
may be either trademarks or registered trademarks of Microsoft in the United States and/or other countries.
The licenses for this project do not grant you rights to use any Microsoft names, logos, or trademarks.
Microsoft's general trademark guidelines can be found at http://go.microsoft.com/fwlink/?LinkID=254653.

Privacy information can be found at https://privacy.microsoft.com/en-us/

Microsoft and any contributors reserve all others rights, whether under their respective copyrights, patents,
or trademarks, whether by implication, estoppel or otherwise.
