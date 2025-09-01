# Ways to install and use

There are many ways to install and use RumbleDB. For example:

* By simply using one of our online sandboxes (Jupyter notebook or simple sandbox page)
* Our newest library: by installing a pip package (pip install jsoniq)
* By running the standalone RumbleDB jar with Java on your laptop
* By installing with homebrew
* By installing Spark yourself on your laptop (for more control on Spark parameters) and use a small RumbleDB jar with spark-submit
* By using our docker image on your laptop (go to the "Run with docker" section on the left menu)
* By uploading the small RumbleDB jar to an existing Spark cluster (such as AWS EMR)
* By running RumbleDB as an HTTP server in the background and connecting to it in a Jupyter notebook with the %%jsoniq magic.
* By installing it manually on your machine.

## Further steps

After installing RumbleDB, further steps could involve:

* Learning JSONiq. More details can be found in the JSONiq section of this documentation and in the [JSONiq specification](https://www.jsoniq.org/docs/JSONiq/webhelp/index.html) and [tutorials](https://colab.research.google.com/github/RumbleDB/rumble/blob/master/RumbleSandbox.ipynb).
* Storing some data on S3, creating a Spark cluster on Amazon EMR (or Azure blob storage and Azure, etc), and querying the data with RumbleDB. More details are found in the cluster section of this documentation.
* Using RumbleDB with Jupyter notebooks. For this, you can run RumbleDB as a server with a simple command, and get started by downloading the [main JSONiq tutorial as a Jupyter notebook](https://raw.githubusercontent.com/RumbleDB/rumble/master/RumbleSandbox.ipynb) and just clicking your way through it. More details are found in the Jupyter notebook section of this documentation. Jupyter notebooks work both locally and on a cluster.
* Write JSONiq code, and share it on the Web, as others can import it from HTTP in just one line from within their queries (no package publication or installation required) or specify an HTTP URL as an input query to RumbleDB!
