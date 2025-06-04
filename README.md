# Transitioning from Computer Vision to Remote Sensing

I noticed that Remote Sensing (RS) Engineers refer to things a little differently which might be confusing for someone coming from Computer Vision (CV) background. This guide helps bridge the gap between CV and RS by comparing key concepts and terminology, mapping CV knowledge to RS applications, highlighting equivalencies, differences, and nuances.

## Computer Vision vs. Remote Sensing Terminology

| **Concept** | **Computer Vision Term** | **Remote Sensing Term** | **Explanation and Notes** |
|-------------|-------------------------|-------------------------|---------------------------|
| **Image** | Image, Frame | Raster, Imagery, Scene | In CV, an "image" is typically a 2D array (e.g., RGB). In RS, a "raster" or "imagery" refers to georeferenced pixel grids, often with multiple spectral bands (e.g., multispectral or hyperspectral). A "scene" is a specific area captured by a sensor. |
| **Pixel** | Pixel | Pixel, Cell | Both fields use "pixel" for the smallest unit in an image/raster. RS may use "cell" in the context of gridded data (e.g., DEMs). RS pixels often carry geospatial metadata (e.g., coordinates). |
| **Color Channels** | Channels (e.g., RGB) | Bands | CV uses "channels" for RGB or grayscale. RS uses "bands" for spectral ranges (e.g., red, NIR, SWIR). RS imagery often has more than three bands (multispectral or hyperspectral). |
| **Image Resolution** | Resolution (e.g., 1920x1080) | Spatial Resolution (e.g., 30 cm/pixel) | CV resolution is pixel dimensions. RS spatial resolution is the ground distance per pixel (e.g., 0.3m for high-res satellite imagery). RS also considers **spectral resolution** (band wavelength range) and **temporal resolution** (revisit frequency). |
| **Feature** | Feature, Object | Feature, Target, Class | In CV, a "feature" is a detectable pattern (e.g., edges, corners) or an object (e.g., car). In RS, a "feature" can be a geographic entity (e.g., building, river) or a spectral signature. "Target" often refers to objects of interest, and "class" is used in land cover classification. |
| **Segmentation** | Semantic Segmentation, Instance Segmentation | Land Cover Classification, Object Extraction | CV’s semantic segmentation assigns a class to each pixel (e.g., person, background). RS’s land cover classification labels pixels as land types (e.g., forest, urban). Instance segmentation in CV is akin to RS’s object extraction (e.g., delineating individual buildings). |
| **Object Detection** | Object Detection | Feature Extraction, Target Detection | CV’s object detection (e.g., bounding boxes around cars) aligns with RS’s feature or target detection (e.g., identifying buildings or vehicles in imagery). RS often emphasizes geospatial context. |
| **Annotation** | Labels, Bounding Boxes, Masks | Ground Truth, Vector Data, Labels | CV uses annotations like bounding boxes or pixel masks. RS uses "ground truth" for validated data (e.g., GeoJSON polygons for buildings) and "vector data" for shapes like points, lines, or polygons. |
| **Coordinate System** | Image Coordinates (x, y) | Geospatial Coordinates, CRS | CV uses pixel-based (x, y) coordinates relative to the image. RS uses geospatial coordinates (e.g., latitude/longitude or UTM) tied to a Coordinate Reference System (CRS, e.g., EPSG:4326). |
| **Preprocessing** | Normalization, Augmentation | Radiometric Correction, Orthorectification | CV preprocessing includes normalization or data augmentation (e.g., rotation). RS preprocessing involves radiometric correction (adjusting for sensor/atmospheric effects) and orthorectification (correcting for terrain distortions). |
| **Region of Interest (ROI)** | ROI, Bounding Box | Area of Interest (AOI) | CV’s ROI is a subregion of an image for processing. RS’s AOI is a georeferenced area (e.g., a city) defined by coordinates or polygons, often tied to real-world boundaries. |
| **Feature Extraction** | Feature Extraction (e.g., SIFT, CNN features) | Spectral Signature, Texture Analysis | CV extracts features like edges or deep learning embeddings. RS extracts spectral signatures (unique band responses, e.g., for vegetation) or texture patterns (e.g., urban layouts). |
| **Classification** | Classification (e.g., ImageNet classes) | Land Use/Land Cover (LULC) Classification | CV classification assigns labels to images or objects (e.g., cat vs. dog). RS’s LULC classification categorizes pixels/areas into classes like forest, water, or urban, often using spectral data. |
| **Training Data** | Dataset (e.g., COCO, ImageNet) | Training Samples, Reference Data | CV uses curated datasets with annotations. RS uses reference data, often combining field measurements, vector labels, or existing maps, with emphasis on geospatial accuracy. |
| **Edge Detection** | Edge Detection (e.g., Canny) | Boundary Delineation | CV’s edge detection finds intensity gradients. RS’s boundary delineation identifies edges of geographic features (e.g., coastlines), often using vectorization or segmentation. |
| **Image Registration** | Image Alignment, Registration | Georeferencing, Coregistration | CV aligns images using feature matching. RS’s georeferencing assigns real-world coordinates, and coregistration aligns multiple images (e.g., multitemporal satellite data) to a common CRS. |
| **Depth Estimation** | Depth Map, Stereo Vision | Digital Elevation Model (DEM), DSM/DTM | CV’s depth estimation infers 3D structure from images. RS uses DEMs (Digital Elevation Models) for terrain height, with DSM (Digital Surface Model) including buildings/trees and DTM (Digital Terrain Model) for bare earth. |
| **Change Detection** | Change Detection | Change Detection, Temporal Analysis | Both fields analyze changes between images. RS emphasizes geospatial changes (e.g., deforestation, urban expansion) using multitemporal imagery, often with spectral indices like NDVI. |
| **Index/Feature** | Index (e.g., HOG features) | Spectral Index (e.g., NDVI, NDWI) | CV uses indices like Histogram of Oriented Gradients for feature description. RS uses spectral indices (e.g., NDVI for vegetation, NDWI for water) derived from band ratios. |
| **Visualization** | Visualization, Heatmap | Thematic Map, Choropleth | CV visualizes results like heatmaps for attention. RS creates thematic maps (e.g., land cover maps) or choropleths to show spatial distributions of classes or indices. |

## Key Observations for Transitioning from CV to RS

1. **Geospatial Context**: RS emphasizes georeferencing and CRS, unlike CV’s focus on pixel coordinates. Use tools like GDAL or Rasterio to handle geospatial metadata.
2. **Spectral Data**: RS often involves multispectral or hyperspectral data (beyond RGB), requiring understanding of bands and spectral signatures. Extend CV’s RGB channel experience to indices like NDVI.
3. **Scale and Resolution**: RS deals with varying spatial resolutions (e.g., 0.3m for commercial satellites, 10m for Sentinel-2). CV’s pixel resolution knowledge applies but consider ground sampling distance.
4. **Data Formats**: RS uses GeoTIFF for rasters and GeoJSON/Shapefiles for vectors, unlike CV’s JPEG/PNG. Use GeoPandas and Rasterio for processing.
5. **Applications**: CV tasks like object detection or segmentation map to RS tasks like feature extraction or land cover classification but involve larger areas and temporal analysis.

## Useful Python Libraries for Remote Sensing

## 1. Rasterio
- **Purpose**: Reading, writing, and manipulating raster data (e.g., satellite imagery in GeoTIFF format).
- **Why it’s useful**: Similar to OpenCV for image I/O in computer vision, Rasterio handles geospatial raster data with support for georeferencing and coordinate transformations.
- **Key Features**:
  - Reads and writes GeoTIFF, NetCDF, and other raster formats.
  - Integrates with NumPy for efficient array-based processing.
  - Supports geospatial metadata like Coordinate Reference Systems (CRS).
- **Example Use Case**: Load a satellite image, apply a computer vision algorithm (e.g., edge detection), and save the result with geospatial metadata intact.

## 2. GeoPandas
- **Purpose**: Working with vector geospatial data (e.g., shapefiles, GeoJSON).
- **Why it’s useful**: For computer vision practitioners, think of GeoPandas as a Pandas extension for handling spatial data like polygons or points, useful for tasks like annotating regions in imagery.
- **Key Features**:
  - Extends Pandas DataFrames with geometry columns.
  - Supports spatial operations like intersections and buffers.
  - Integrates with Matplotlib for visualization.
- **Example Use Case**: Overlay bounding boxes or segmentation masks from computer vision models onto geospatial maps.

## 3. PyTorch or TensorFlow with TorchGeo
- **Purpose**: Deep learning for remote sensing tasks.
- **Why it’s useful**: If you’re familiar with PyTorch or TensorFlow for computer vision, TorchGeo extends these frameworks for geospatial data, providing datasets and models tailored for remote sensing.
- **Key Features**:
  - Pre-built datasets (e.g., Sentinel-2, Landsat) with geospatial metadata.
  - Models for tasks like land cover classification and change detection.
  - Handles multi-band imagery (e.g., RGB + infrared).
- **Example Use Case**: Fine-tune a U-Net for semantic segmentation on multispectral satellite imagery.

## 4. GDAL/OGR
- **Purpose**: Comprehensive geospatial data processing.
- **Why it’s useful**: GDAL is a powerful backend for geospatial data manipulation, akin to a Swiss Army knife for raster and vector data. It’s often used indirectly via Rasterio or GeoPandas but can be leveraged directly for complex tasks.
- **Key Features**:
  - Supports hundreds of geospatial file formats.
  - Provides tools for reprojection, resampling, and mosaicking.
  - Command-line and Python API access.
- **Example Use Case**: Preprocess a large dataset of satellite images by reprojecting them to a common CRS before feeding into a computer vision pipeline.

## 5. Spectral
- **Purpose**: Analyzing hyperspectral and multispectral imagery.
- **Why it’s useful**: Unlike RGB images in computer vision, remote sensing often involves hyperspectral data with many bands. Spectral provides tools to process these high-dimensional datasets.
- **Key Features**:
  - Spectral unmixing and dimensionality reduction.
  - Classification algorithms for hyperspectral data.
  - Visualization of spectral signatures.
- **Example Use Case**: Classify land cover types using hyperspectral data, leveraging techniques like PCA familiar from computer vision.

## 6. OpenCV
- **Purpose**: General-purpose image processing.
- **Why it’s useful**: If you’re from a computer vision background, you’re likely already familiar with OpenCV. It’s still valuable in remote sensing for tasks like filtering, feature extraction, or preprocessing before geospatial analysis.
- **Key Features**:
  - Extensive image processing functions (e.g., thresholding, edge detection).
  - Compatible with NumPy arrays from Rasterio.
  - Fast C++ backend with Python bindings.
- **Example Use Case**: Apply histogram equalization to enhance satellite imagery before feeding it into a deep learning model.

## 7. Shapely
- **Purpose**: Manipulating and analyzing geometric objects.
- **Why it’s useful**: Shapely is ideal for handling geometric operations (e.g., calculating areas, intersections) in vector data, complementing computer vision tasks like object detection or segmentation.
- **Key Features**:
  - Simple API for points, lines, and polygons.
  - Supports spatial relationships (e.g., contains, intersects).
  - Lightweight and integrates with GeoPandas.
- **Example Use Case**: Convert detected objects from a computer vision model into geospatial polygons for mapping.

## 8. Xarray
- **Purpose**: Handling multidimensional labeled arrays.
- **Why it’s useful**: Remote sensing data often comes in multidimensional formats (e.g., time series of satellite images). Xarray extends NumPy for labeled arrays, similar to how you might use tensors in computer vision.
- **Key Features**:
  - Supports NetCDF and other climate data formats.
  - Time series and spatial indexing.
  - Integrates with Dask for large datasets.
- **Example Use Case**: Analyze temporal changes in NDVI (Normalized Difference Vegetation Index) across a series of satellite images.
