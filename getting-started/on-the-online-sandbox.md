# Trying on the online sandbox

If you really want to start writing queries right now, there is a public sandbox [here](https://colab.research.google.com/github/RumbleDB/rumble/blob/master/RumbleSandbox.ipynb) that will just work and guide you. You only need to have a Google account to be able to execute them, as this exposes our Jupyter notebook via the Colab environment. You are also free to download and use this notebook with any other provider or even your own local Jupyter and it will work just the same: the queries are all shipped to our own, small public backend no matter what.

If you do not have a Google account, you can also use our simpler sandbox page without Jupyter, [here](http://public.rumbledb.org:9090/public.html) where you can type small queries and see the results.

With the sandboxes above, you can only inline your data in the query or access a dataset with an HTTP URL.

Once you want to take it to the next level and query your own data on your laptop, you will find instructions below to use RumbleDB on your own computer manually, which among others will allow you to query any files stored on your local disk. And then, you can take a leap of faith and use RumbleDB on a large cluster (Amazon EMR, your company's cluster, etc).
