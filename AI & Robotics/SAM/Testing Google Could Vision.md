2024-05-23


Asked on Perplexity and got directed to this blog: https://journal.code4lib.org/articles/17186

Found out that google cloud is not that expensive: https://cloud.google.com/vision/pricing/
![[Pasted image 20240523230005.png]]


Got account setup and trying out Google Cloud Vision. Kind of annoyed it already needs credit card info for a trial.
https://cloud.google.com/python/docs/reference/vision/latest

```
py -m venv <your-env>
.\<your-env>\Scripts\activate
pip install google-cloud-vision
```

23:03 Installed successfully.
Image annotation: https://cloud.google.com/python/docs/reference/vision/latest/google.cloud.vision_v1.services.image_annotator

Seems not so hard: https://www.youtube.com/watch?v=BN8aO0LULyw&t=1s&ab_channel=GoogleCloudTech
- the video guide is from this doc: https://cloud.google.com/vision/docs/detect-labels-image-api#make_a_request_to_the_cloud_vision_api_service

23:18 Created a cloud storage bucket.
- change bucket access from uniform to fine-grained. Damn it works.
- 

23:29 It worked:
```
{
  "responses": [
    {
      "labelAnnotations": [
        {
          "mid": "/m/0838f",
          "description": "Water",
          "score": 0.9835823,
          "topicality": 0.9835823
        },
        {
          "mid": "/m/01bqvp",
          "description": "Sky",
          "score": 0.95418584,
          "topicality": 0.95418584
        },
        {
          "mid": "/m/09686",
          "description": "Vertebrate",
          "score": 0.9219572,
          "topicality": 0.9219572
        },
        {
          "mid": "/m/03ktm1",
          "description": "Body of water",
          "score": 0.86342466,
          "topicality": 0.86342466
        },
        {
          "mid": "/m/04h4w",
          "description": "Lake",
          "score": 0.83913785,
          "topicality": 0.83913785
        },
        {
          "mid": "/m/02c9_23",
          "description": "Coastal and oceanic landforms",
          "score": 0.8332006,
          "topicality": 0.8332006
        },
        {
          "mid": "/m/0bp3j53",
          "description": "Boats and boating--Equipment and supplies",
          "score": 0.7970429,
          "topicality": 0.7970429
        },
        {
          "mid": "/m/017m8l",
          "description": "Waterway",
          "score": 0.78091794,
          "topicality": 0.78091794
        },
        {
          "mid": "/m/017rgb",
          "description": "Ferris wheel",
          "score": 0.7090119,
          "topicality": 0.7090119
        },
        {
          "mid": "/m/083wq",
          "description": "Wheel",
          "score": 0.75200224,
          "topicality": 0.75200224
        }
      ]
    }
  ]
}

```