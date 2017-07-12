# Travel Sample Application

Couchbase Travel is a sample web application that demonstrates how to interact with the Couchbase query services via the SDKs

Each SDK comes with its own implementation of the backend for the application. You can download the complete source code and then build and run the app. While the app runs, you can get a peek of what is happening in the backend via the "Narration (a console like UI element that can display N1QL queries for instance). It is only visible in components that communicate with the server (but can also be collapsed).

![](./assets/travel-app-narration.png)

The documentation for the travel app illustrates the data model and walks through the N1QL and FTS queries used to select flights and search for hotels.

## Generic set up

You'll need:

- Your favorite editor or EID.
- Your SDK of choice and its specific dependencies
- A local Couchbase 4.5 (or greater) installation (make sure that the travel-sample bucket has been loaded from 4.5 and that there is, at least, one node with data, query, index and full text search services in the cluster)
- That's it!

To start, it is easiest if you run Couchbase Server and the travel sample app on the same machine. It's not required to run your development environment this way, and advanced MDS configurations are supported. It's just easier to start a development environment with components running locally.

Download [Couchbase Server 4.6](https://www.couchbase.com/downloads) and install it. As you follow the download instructions and setup wizard, make sure you keep all the services (`data`, `query`, and `index`) selected. Make sure also to install the sample bucket named `travel-sample` (introduced in Couchbase Server 4.0) because it contains the data used in this tutorial.

![](assets/cb-server-services.png)

![](assets/cb-server-travel-sample.png)

If you already have Couchbase Server 4.5 or later installed but did not install the `travel-sample` bucket (or you installed it from a 4.0 version), open the Couchbase Web Console and select **Settings > Sample Buckets**. Select the `travel-sample` checkbox, and then click Create. A notification box in the upper-right corner disappears when the bucket is ready to use.

Finally, in order to be able to search for `hotels`, an FTS index called hotels must be created on the travel-sample bucket. Go to `Indexes > Full Text > New Full Text Index` and create the hotels index:

![](./assets/travel-app-fts-index.png)

Note that the index mapping could be better tuned, but a generic mapping will also work fine for this example.

## Using the Travel App

Before running the application, make sure that the Couchbase Server instance is running.

### Installation

```bash
$ git clone https://github.com/couchbaselabs/try-cb-java.git
cd try-cb-java
mvn spring-boot:run
```

Open `127.0.0.1:8080` in your browser.

To find a flight:

1. Open a browser and navigate to the login URL that was displayed when you started the app.
2. Sign in to Couchbase Travel by providing your credentials, or create a new account (which will expire after some time depending on the backend used, usually 1 hour):

	![](assets/travel-app-login.png)

3. Find a flight:

![](assets/travel-app-screen-flight.png)

In the Airport or City section, enter an airport code or city name in the From and To fields. In the Travel Dates section, enter Leave and Return dates in MM/DD/YYYY format. Click Find Flights. The app displays the available flights for the outbound and return legs of the trip. Select one or more flights to add to your cart by clicking the choose button next to each flight.

![](assets/travel-app-screen-flight-choose.png)

Now you have flights stored locally in your cart. Go to the cart page and review your flights:

1. Click on the cart icon button in the navigation bar, top right
2. See that the number of flights in the cart and the total cost are displayed in said navigation bar.
3. Review the flights in your cart, and book one by clicking on the book button next to it.

![](assets/travel-app-screen-cart.png)

4. The flight is now booked, as confirmed by a message on the page

The flights that you've booked are stored in the backend. You can see which flights you've booked by navigating to the "user" page. The backend uses authentication to control access to the endpoints for booking and listing flights.

![](assets/travel-app-screen-user.png)

To search for an hotel, navigate to the hotel page:

Click on the "earth" icon in the navigation bar (rightmost icon). Click on the Find Hotels button if you want to have a look at any first 100 hotels. Refine your search using the input boxes, either by keyword, geographically or both.

![](assets/travel-app-screen-hotels.png)

> Warning: You should make sure that you have installed Couchbase 4.5 with FTS enabled. The API backend that is used by the hotels page makes use of an FTS index that you have to create, named hotels.

## Using CBQ

You can browse and access documents in Couchbase using browser and command line tools without writing any code. This can be helpful to you if you simply wish to inspect cluster data without writing code. You can access individual documents using the command line and web console.

### Using the command-line query shell (cbq)

You can use the cbq program as a command line query shell to issue N1QL queries in couchbase. cbq is available on all cluster nodes and does not require a separate installation. Simply run cbq from the Couchbase installation directory ( /opt/couchbase/bin/cbq on Linux and "/Applications/Couchbase Server.app/Contents/Resources/couchbase-core/bin/cbq"). Once in the shell, you should be greeted by a prompt and ready to issue any N1QL query.

```bash
$ ./cbq
cbq> select airportname FROM `travel-sample` WHERE airportname IS NOT NULL LIMIT 1;
	// TODO
```

### Web Console Document Access

You can use the Web Console to view, edit, and create JSON documents up to 2.5KB in size. To access documents using the Web Console:

1. Navigate your browser to a cluster node. Type the address of the cluster with the admin port (8091). Use your username (usually Administrator). The password is the password you used when setting up the cluster.
2. Once logged in, go to the Data Buckets section

	![](assets/webui-buckets.png)

3. Click on the Documents button of the default bucket:

	![](assets/webui-documents.png)
	
4. To retrieve a document, type in the document ID and click on Lookup ID. This will then open the document edit page:

	![](assets/webui-lookup.png)
	
	![](assets/webui-editor-existing.png)
	
5. To create a new document, return to the Documents section and click on Create Document

	![](assets/webui-create-document.png)

6. You’ll be prompted to enter a document ID

	![](assets/webui-newdoc-prompt.png)

7. Once created, you can now edit your document. Don't forget to Save when done

	![](assets/webui-editor-new.png)