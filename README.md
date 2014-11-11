gmap-js
=======

google map geojson api


Getting Started

1. Parse your geojson file

# How to use in Java(Android) Application

- in GeoDataHelper

```java
public String getMapParam(String worldID) {

  DBHelper dbHelper = new DBHelper(mContext);

  // input stream to jsonObject
  JsonObject geodata = dbHelper.getGeoData(worldID.toUpperCase());

  if(geodata != null) {
    double cenX = geodata.get("Cen_x").getAsDouble();
    double cenY = geodata.get("Cen_y").getAsDouble();
    double shapeArea = geodata.get("Shape_Area").getAsDouble();

    int zoom = (int) Math.floor((Math.log(shapeArea * 20)));

    if (zoom < 5) {
      zoom += 1;
    } else if (zoom > 5) {
      zoom -= 1;
    }

    if (zoom < 0) {
        zoom = 0;
    }

    String coords = geodata.get("Coords").getAsString();
    coords = coords.replaceAll("\\s+", "");

    Log.d("cen_x", "" + cenX);
    Log.d("cen_y", "" + cenY);

    try {
      
      // !! important
      String params = "?&lat=" + cenY + "&lon=" + cenX + "&zoom=" + zoom + "&polygon=" + URLEncoder.encode(coords, "UTF-8");

      return params;
    } catch (UnsupportedEncodingException e) {
        e.printStackTrace();
    }
  }
  return null;
}
```  
  

- in Application

```
String mapParam = geodataHelper.getMapParam(worldID);

if(mapParam != null) {
    map.loadUrl("file:///android_asset/map.html" + mapParam);
}
```
