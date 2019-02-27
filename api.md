**Hazards**
-----------

* **URL**

	`/api/:hazard/`

* **Method:**

	`GET`


* **Success Response:**

```
  {
      "hazard_id" : {
          "name" : String,
          "location": {
              "north": Double,
              "east": Double,
              "south": Double,
              "west": Double
          },
          "last_updated" : "mmddyyyy"
  }
```

* **Notes:**

	TODO



**Hazard Data**
----------

* **URL**

	`/api/:hazard/:hazard_id`

* **Method**

	`GET`

* **URL Params**

	*Required*

	* `"id"=Integer`
	* `"image_type"=List[Backscatter, TempCoh, ...]`

	*Optional*

  * `"satellites"=[<satellite_id>]`
  * `"start_date"="mmddyyyy"`
  * `"end_date"="mmddyyyy"`
  * `"max_num_images"=Integer`
  * `"last_n_days"=Integer`
<br/><br/>

* **Success Response:**

```
{
    "<hazard_id>": {
          "hazard_name": String,
          "last_updated": "mmddyyyy",

          "location": {
              "north": Double,
              "east": Double,
              "south": Double,
              "west": Double
          },
          "images": {
                "satellite_id1": {
                    "dataType1": [
                        <url_to_image_1>,
                        <url_to_image_2>,
                        ...
                    ],
                    "dataType2": [
                        <url_to_image_1>,
                        <url_to_image_2>,
                        ...
                    ],
                    ...
                },
                "satellite_id2": {
                    "dataType1": [
                        ...
                    ],
                    ...
                },
          }
    }
}
```


* **Notes:**
  * Should we include any metadata with each image?
    - Assuming the image date is "in" the generated image, do we need to include the image date?
  * The `last_n_days` parameter can only be used if neither the `start_date` and `end_date` parameters are used.
  * `max_num_images` will use the most recent images first.
  * Images in the response content will be ordered chronologically with newest images first.
  * TODO

**Hazard Download**
----------

* **URL**

	`/api/:hazard/download/:hazard_id`

* **Method:**

	`GET`

* **URL Params**

  *Required*

  * `"hazard_id"=Integer`

  *Optional*

  * `"satellites"=[satellite_id]`
  * `"start_date"="mmddyyyy"`
  * `"end_date"="mmddyyyy"`
  * `"max_num_images"=Integer`
  * `"last_n_days"=Integer`
  * `"dataType"=OneOf["GEOTIFF", "KMZ", "raw", "GIF"]`
<br/><br/>
* **Success Response:**

	**Content:** link to file for download
