# Code_NDVI
//Filter by data (your selection, geometry and clouds, claculate the median of all bands)
var collection = imageCollection. filterBounds(geometry)
.filterDate('2022-01-01', '2022-02-27')
.filterMetadata('CLOUD_COVER' , 'less_than' ,10)
print(collection);

// select first image that meets criteria
var image = collection.first();

// Step2: Calculate NDVI

//Calculate NDVI
var ndvi = image.normalizedDifference(['B5', 'B4']);

// Display NDVI
Map.addLayer(ndvi, {min: -1, max: 1, palette: ['blue', 'white', 'green']}, 'NDVI');

// Step3 calculate EVI
//...................
//Calculate EVI

var evi = image. expression(
  '2.5 * ((NIR - RED) / (NIR + 6 * RED - 7.5 * BLUE + 1))', {
    'NIR' : image.select('B5'),
    'RED' : image.select('B4'),
    'BLUE' : image.select('B2')
  });
  //Display EVI
  Map.addLayer(evi, {min: -1, max: 1, palette: ['blue', 'white', 'green']}, 'EVI')
  
  //Step4 compare NDVI and EVI
  //Subtract NDVI from EVI
  
  var differenceEVINDVI= evi.subtract(ndvi);
  
  // displayresult
  Map.addLayer(differenceEVINDVI, {min: -1, max: 1, palette: ['blue', 'white', 'green']}, 'EVI minus NDVI');
  
