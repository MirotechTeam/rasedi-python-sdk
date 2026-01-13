# miropay-python-sdk #
A python SDK for Miropay payment services.

### Installation ###
You can install it using `pip install miropay-payment-sdk`

### How to use ###
First of all you will need `API_KEY` and `API_SECRET`. These credentials are provided inside of your panel.

### Note ###
If your key starts with `live_...` it means you are using live mode and if it starts with `test_...` it means you are using sand-box mode.

## Create payment client ##
Once you provided the `API_KEY` and `API_SECRET` now you can create your payment client simply using `PaymentClient` class provided in the sdk.

For example:
`from MiropayPaymentSDK import PaymentClient`

`client = PaymentClient(api_key="YOUR_KEY", api_secret="YOUR_SECRET")`

Now you are all set to use our payment system.

## Create payment link ##
Now that you have created the `PaymentClient` you can use it to create payment links.

You can use `ICreatePayment` provided to create a payment link.
It includes the fields bellow:
  - `amount : str` which will declare the amount of the payment.It should be a stringified number.
  - `title : str` which is the title of your payment link so you can keep track of it in your panel and customers will see the title once paying.
  - `description : str` which is the description of your payment link. customers will see the description once paying.
  - `gateways : list[GATEWAY]` which it the selection of the `GATEWAY`s you want your customers use to pay. You can use `GATEWAY` enum provided in the sdk and use the gateways you desire.`from MiropayPaymentSDK import GATEWAY` 
  - `collectFeeFromCustomer : bool` with this set as `True` all service fees will be collected from your customer paying the payment link. This means service fees will be added to the `amount` you provided once paying.
  - `collectCustomerEmail : bool` with this set as `True` your customers should enter their email address once paying the payment link.
  - `collectCustomerPhoneNumber: bool` with this set as `True` your customers should enter their phone number once paying the payment link.
  - `redirectUrl : str` The url you want your customer to be returned after payment being `PAID` or `FAILED`.

  Now with every parameter explianed, here is an example of create payment link:

  `response = await client.create_payment(`
    
    payload=ICreatePayment(
            amount="10200",
            title="Test Payment",
            description="This is a test payment",
            gateways=[GATEWAY.FIB,GATEWAY.ZAIN],
            redirectUrl="https://google.com",
            collectFeeFromCustomer=True,
            collectCustomerEmail=True,
            collectCustomerPhoneNumber=False
            )
  `)`

  After this request you will get the response as an instance of `ICreatePaymentResponse`.
  
  
  # All responses from our end includes: 
  - `statusCode : int` Which is the http status code
  - `headers : dict[str, str]` Which is the headers
  - `body` The type of body is differ based on the method you are using.


  #
  
  Now with that being said lets explian the response `body` of create payment link:
  It with be an instance of `ICreatePaymentResponseBody`:
  - `referenceCode : str` which is a unique id for your payment link, you can use this to check the your payment which will be explained in next step.

  - `amount : str` the amount of payment with fees included (or excluded) based on the `collectFeeFromCustomer` field.

  - `paidVia : Optional[str]`
  
  - `paidAt : Optional[str]`
  - `redirectUrl :str`
  - `status : PAYMENT_STATUS`
  - `payoutAmount : Optional[str]`
