Helps you organize and build your shopify theme assets


File structure
-----
Put your files in

``assets_src/vendor/libs`` (javascript libraries, jquery plugins)

``assets_src/less`` (your styles)

``assets_src/coffee`` (your javascript)

``assets_src/img`` (your images)




Building your assets
---------------------

```bash
cd assets_src
make
```



Output
------
Your assets will be compiled and put into the ``assets``folder in the root directory


Some comments
-------------
* The script will compile two versions of the provided js/coffee/less assets. A normal and a minified version.
* all coffeescript files in ``assets_src/coffee``will be joint into a single file called ``application.js``
* The only less file that will be compiled is ``application.less``. You should @import all your other less files there.
* Images can also be organized into subfolders within ``assets_src/img``. The script will simply copy all files (regardless of whether it really is an image or not) over to the assets folder.