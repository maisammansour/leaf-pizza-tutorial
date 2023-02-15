![alt](/logo.png)

## Main user story

I am a user who wants to order pizza.

I would like an API that allows me to order pizza.
I should be able to order at most 1 pizza per order.
A pizza has a type (e.g. margherita, pepperoni, bianca, etc.), a size (e.g. small, medium, large) and an assortment of toppings.
I am able to order as many drinks as I want within my order.
I also must supply a valid US address within the order.

The address I input should be validated against the Google Address Validation API (The owner would prefer that no external Google libraries are used due to licensing issues) that validates addresses. If the address is invalid, the order should be rejected. The documentation can be found [here](https://developers.google.com/maps/documentation/address-validation/requests-validate-address) (The owner will provide an API key for the Google Address Validation API.)

In addition, I should not be able to place an order without ordering at least one drink

I go to the API and order my pizza. Once I have ordered my pizza, I will receive an immediate order confirmation containing the new order's ID, as well as the pizza and any drinks I may have ordered.

I can then go to the API and check the status of my order given the order ID. I and others like me will make a lot of requests, so the API should be cached so that I can **reliably** check the status of my order without having to hit the database every time.

Once the order is in the system, a scheduled job will run every few seconds to fetch all pending orders and process them. **Only one job should run at a time**, to avoid duplicate processing. The restaurant only has a set amount of employees who can make pizzas at any given time and they can also handle the orders concurrently, but only up to the amount of employees. The processing of the order should take between 5-10 seconds.

Once the order has started processing, the status of the order should be updated to "processing". Once the order has finished processing, the status of the order should be updated to "processed".

## Administrator user story

I would like to be able to configure the amount of employees and delivery drivers. 


## Notes

The application should be unit and integration tested.
