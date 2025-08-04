# Writing queries directly in Jupyter notebook cells

The Python edition of RumbleDB comes out of the box with a JSONiq magic.

If you are in a Jupyter notebook and have installed the jsoniq pip package, you can activate the jsoniq magic with:

```
%load_ext jsoniqmagic
```

Then, you can run JSONiq in standalone cells and see the results:

```
%%jsoniq
{"foobar":1} 
```

Of course, you can still continue to use rumble.jsoniq() calls and process the outputs as you see fit.

An example of the magic in action is available in our [upgraded online sandbox](https://colab.research.google.com/github/RumbleDB/rumble/blob/master/RumbleSandbox.ipynb).

Note: This is a different magic than the magic that works with the RumbleDB HTTP server. It is more modern and running a server is no longer needed with this different magic. It suffices to install the jsoniq Python package.&#x20;
