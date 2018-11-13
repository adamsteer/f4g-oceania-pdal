# Entwine - massive point cloud organisation



## creating an entwine index

`docker run -it -v $(pwd):/opt/data connormanning/entwine -i dataset.laz -o dataset-entwine`





## viewing an Entwine datasource


Once you've created an Entwine index, start a webserver in the directory you're working in:


`python -m http.server 9001`

...and navigate to http://potree.entwine.io. Modify the URL to something like:

http://potree.entwine.io/custom.html?r=\"http://localhost:9001/dataset-entwine/\"
