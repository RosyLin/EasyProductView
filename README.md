# APP_Retail_EasyProductView
The Retail EasyProductView application is used by the Retail and Outlet stores to view product information in lieu of the Product Information System (PIS). It is designed to be a single-page application which will require fewer clicks for the user to find the information they are looking for. It show sku-level inventory as well as incorporates tag printing functionality from App_Retail_TagPrinting.  

# URL's for the application are below:

PROD:
https://mw-retailprod-lb.llbean.com/EasyProductView/

QA:
https://mw-retailqa-lb.llbean.com/EasyProductView/

DEV:
https://mw-retailtest-lb.llbean.com/EasyProductView/

# How the application works.
The application has a React front-end that calls a number of different JAX-RS endpoints on the WAS-101 server. The application shares some endpoints with the App_Retail_TagPrinting application as well as consumes an in-house API called objectify-product-information. The objectify-product-information API was specifically written to support the EasyProductView application by scraping from PIS only the essential information.

When the application starts, the user is presented with a blank screen and a search bar. The user may enter a sku, an itemID, or a freetext search term. If a sku is entered, the user will be brought directly to the information page for that product with the correct size and color selected to reflect the sku. This would be the case in a returns situation with a sku present or if an item on the floor is missing a tag and the one next to it has one. The search box supports scanning in skus in from barcodes using usb scanners or the new mobile Zebra devices. If an itemID is entered, the user will be taken directly to the product page. This would be the case in a returns situation or a user on the floor when the user only has the item's tag with itemID.

This application replaced the Product Information System (PIS) tool accessed via the llweb homepage. 

# Building the project
To build, pull down source from GitHub and add EAR to existing WAS server. There are two URL resources that must be added in the developer console of the WAS server in order to run the application. They are as follows:

First:

Name: ApigeeURL

JNDI name: url/ApigeeProductsAPI

Specification: https://test.api.llbean.com/v1/product/info

Second:

Name: TagPrintingURL

JNDI name: url/TagPrintingURL

Specification: https://mw-retailqa-lb.llbean.com/TagPrintingApp/

Once this is complete, start the WAS server before continuing.
Node will have to be installed in order to run this application. Navigate to \APP_Retail_EasyProductView\EasyProductView\EasyProductView\WebContentDev at a unix command prompt and run the command "npm start". This will start up the front end. At this point, the application should be accessible locally at http://localhost:3000/.

# Troubleshooting
Troubleshooting Instructions

A troubleshooting document will be added at a later date.
