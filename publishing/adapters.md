# Adapters

Extensions, rules, and data elements are building blocks. When you want to make your application do something, these building blocks are added to libraries and then a library is "built" into a build. Those builds are delivered to a hosted location.

Adapters define available destinations where the builds can be delivered. They come in two types:

1. Managed by Adobe
2. SFTP

The same adapter can be used for multiple environments within a property.

## Managed by Adobe

This adapter typr is the default selection and is simplest to manage.

If you choose to have Launch manage the hosting for your build, Launch delivers the build to a 3rd party CDN. These CDNs operate independently from Adobe, so even when Launch has maintenance or downtime, the tags deployed to your sites and applications continue to function as normal.  The embed code references the main library file on the CDN so a browser can retrieve the files ar run-time.

When you create a new property through the UI, one of these will be created for you.

### Create Managed by Adobe adapter

1. Open the Adapters tab.
2. Create the new adapter.
3. Name the adapter.
4. Select Managed by Adobe as the adapter type.
5. Click Save.

### Why choose Adobe-managed hosting for library files?

When you choose the Adobe-managed option, it means that your Launch library files are served to your visitors from the 3rd party CDNs that Adobe has contracted with.  Today, the primary CDN provide is Akamai.

Akamai is robust when serving content to a global, high-volume audience of Web visitors. Akamai runs redundant networks of load-balanced, geo-optimized nodes to serve content as quickly as possible to visitors wherever they are located throughout the world.

Specifically, Akamai runs more than 137,000 servers in 87 countries within more than 1,150 networks. In terms of redundancy, Akamai does not just route from one server to another, Akamai routes from one node of servers to another node of servers as-needed. In other words, each node consists of multiple servers for redundancy within a node, so a box going down is not an issue because he other boxes in the node take over. If a node goes down, Akamai serves from the next closest one, with the same cached content. Nodes are dynamically selected based on visitor location, traffic load, and other factors so content is consistently served from the best local node for each visitor.

### Can I avoid errors in case of CDN unavailability?

No. Launch can do nothing from the client side if the library is unavailable. However, these CDNs are designed with multiple redundancies and have made a business around making assets available all the time.

If you feel that you need more control over the library and it's location, we recommend you look into self-hosting the library files.

### CDN cache control headers

Cache control headers are automatically set for libraries hosted using the Adobe-managed adapters.

* Production builds: Cache control headers are set to 60 minutes
* Staging builds with "-staging" in the filename: Cache control headers are set to 0 minutes
* Dev builds with "-development" in the filename: Cache control headers are set to 0 minutes

Note: It is up to browsers to receive and respect the cache control headers. Some browsers might ignore them.

Important: Cache control headers were added in Spring of 2018. Environments that do not have "-development" or "-staging" in their Environment embed codes were created prior to that time.  These environments will need to be recreated if you want to take advantage of these new cache control headers.  If you don't re-create the environments, you'll have the same 60-minute cache control as the production libraries.

## Self-managed adapter

There are a number of reasons to choose to host your own build files.

* Organizations have a number of security and legal requirements that might make cloud-hosting a less-desirable or impossible option.
* You can reduce the number of required DNS lookups by hosting your own files.
* You might prefer to have more control over edge locations, caching, and so on.

To meet these requirements, Launch allows you to push your completed builds to an SFTP destination.

Launch connects to an SFTP site using an encrypted key. To use this connection type:

* Your SFTP server must generate a key
* You must provide the username and encrypted private key during the adapter setup process

For more information about envruptying your private key, please check the [Encrypting Values](https://developer.adobelaunch.com/guides/api/encrypting_values/) guide in the developer docs.  

If you select an SFTP adapter for your environment, there is an additional path variable that you'll need to provide to the environment. This path field is the HTTP path to where your build files are stored relative to your web site. This field is required because the different files in the build reference one another. Launch needs to know where they will be so the files can reference each other properly.

### Create an SFTP adapter

1. Open the Adapters tab.
2. Create the new Adapter.
3. Name your adapter.
4. Select SFTP as the adapter type.
5. Enter the host, path, port, username, and encrypted private key.
6. Click Save.

