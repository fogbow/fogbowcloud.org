Title: RESTful API
url: fogbow-api
save_as: fogbow-api.html
section: usage
index: 1

Programmatic interface
==========

Fogbow provides a RESTful API that implements OGF's OCCI standard. This API also extends the OCCI standard to incorporate fucntionalities that are only meaningful in the context of a federation of cloud providers.

### Resources

#### Order: /fogbow_request

Endpoint | Method | Header parameters | Description
------------ | ------------- | ------------ | -------------
/fogbow_request | GET | No fields are required | Fetch the list of user's orders
/fogbow_request/{order_id} | GET | No fields are required | Fetch an order by its ID
/fogbow_request | DELETE | No fields are required | Delete all user's orders
/fogbow_request/{order_id} | DELETE | No fields are required | Delete a specific order by ID
/fogbow_request | POST | *X-OCCI-Attributes:* org.fogbowcloud.request.requirements, org.fogbowcloud.order.resource-kind <br> *Categories:* example1, example2 | Description
