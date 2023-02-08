I am a user who wants to order pizza

I go to the API and order my pizza, providing the following fields:

1. I can only order 1 pizza
    a. A pizza can have 1 or more toppings
    b. A pizza has a type (e.g. margherita, pepperoni, bianca, etc.)
    c. A pizza has a size (e.g. small, medium, large, etc.)
    d. An address (Assume US addresses)
2. I can also supplement the order with an array of drinks

The address I input should be validated against the Google Address Validation API (The owner would prefer that no external Google libraries are used due to licensing issues) that validates addresses. If the address is invalid, the order should be rejected. The documentation can be found here: https://developers.google.com/maps/documentation/address-validation/requests-validate-address

The owner will provide an API key for the Google Address Validation API.

Once I have ordered my pizza, I will receive an immediate response with the following fields:

```
Order ID
Order Status (e.g. pending, processing, pending delivery, delivered)
Pizza {
    Type
    Size
    Toppings
}
Drinks [
    "Drink 1",
    "Drink 2",
    "Drink 3"
]
```

I can then go to the API and check the status of my order given the order ID. The API should be cached so that I can check the status of my order without having to hit the database. (Caching should know when to evict the relevant order)

Once the order is in the system, a scheduled job will run every few seconds to fetch all pending orders and process them. Only one job should run at a time, to avoid duplicate processing. The restaurant only has a set amount of employees who can make pizzas at any given time and they can also handle the orders concurrently, but only up to the amount of employees. The processing of the order should take 5-10 seconds.

Once the order has started processing, the status of the order should be updated to "processing". Once the order has finished processing, the status of the order should be updated to "pending delivery".

Once the order has been processed, a scheduled job will run every few seconds to fetch all orders that are pending delivery and deliver them. This scheduled job should be inside of a lock/mutex and only one should run at a time, to avoid duplicate delivery. The delivery company only has a set amount of delivery drivers at any given time and they can also handle the orders concurrently, but only up to the amount of employees. The delivery of the order should take a random amount of time between 5 and 10 seconds.

Allow configuration of the number of employees and the number of delivery drivers.

Once the order has started delivery, the status of the order should be updated to "delivering". Once the order has finished delivery, the status of the order should be updated to "delivered".

The application should be thoroughly unit and integration tested.