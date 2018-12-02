---
title: "Integrating Paystack payment system with PHP — Ultimate Guide"
date: 2018-11-28T10:29:28+01:00
draft: false
description: "In this guide, you'll learn how to integrate Paystack payment system on your php website."
---

![Paystack payment confirmation](/uploads/paystack_success.png)

In this guide, you'll learn how to integrate Paystack payment system on your website. 

This guide is comprehensive, so you should go through every information for maximum output.


Paystack is a Modern online and offline payments for Africa.

*Before you can start integrating Paystack, you will need a [Paystack account](https://dashboard.paystack.co/#/signup). Create a [free account](https://dashboard.paystack.co/#/signup) now if you haven't already done so.*


The Paystack API gives you access to pretty much all the features you can use on your account dashboard and lets you extend them for use in your application. It strives to be RESTful and is organized around the main resources you would be interacting with - with a few notable exceptions.

After creating an account, the next thing is to sign in to your new Paystack account. On your dashboard you will find your ***public*** and ***secret*** key. We will make use of the ***public*** and ***secret*** key for this guide.

## Let Dive In

Paystack have three major approach to integrate their API and collect payments on your websites. But this guide explain only two approach. The third approach is with pure JavaScript, which I will discuss entirely on a seperate post. 

1. [Paystack Inline](#inline) 
2. [Paystack Standard](#standard)

Using any one of two depends on what you're trying to do. Each one comes with it own user and developer experience.

## <a id="inline"></a> Paystack Inline

This approach offers a simple, secure and convenient payment flow for web. It can be integrated with a line of code thereby making it the easiest way to start accepting payments. It also makes it possible to start and end the payment flow on the same page, thus combating redirect fatigue.

Now, the code below display a Paystack pay button.

```
<form>
  <script src="https://js.paystack.co/v1/inline.js"></script>
  <button type="button" onclick="payWithPaystack()"> Pay </button> 
</form>

```
The pay button is yet to do what you expect. So if you click on it, nothing will happen. For it to work you need to add the `payWithPaystack()` Javascript function below the form.

Here is the `payWithPaystack` function provided by Paystack. 

```
<!-- place below the html form -->
<script>
  function payWithPaystack(){
    var handler = PaystackPop.setup({
      key: 'paste your key here',
      email: 'customer@email.com',
      amount: 10000,
      ref: ''+Math.floor((Math.random() * 1000000000) + 1), // generates a pseudo-unique reference. Please replace with a reference you generated. Or remove the line entirely so our API will generate one for you
      metadata: {
         custom_fields: [
            {
                display_name: "Mobile Number",
                variable_name: "mobile_number",
                value: "+2348012345678"
            }
         ]
      },
      callback: function(response){
          alert('success. transaction ref is ' + response.reference);
      },
      onClose: function(){
          alert('window closed');
      }
    });
    handler.openIframe();
  }
</script>

```
You need to replace the `key:` value '*paste your key here*' with the public key on your Paystack account settings. If login, you can locate the key [here](https://dashboard.paystack.com/#/settings/developer).

> Please note that the key to use with inline is the public key and not the secret key

If you did it correctly, on the click of the pay button, a nice looking Paystack payment UI will pop out. Since you're testing the payment API, you should use a test card.

> To use a test card,  you should use the ***test public key*** instead. And never forget to replace the ***test public key*** with the ***live public key*** on a live website.

![paystack payment form](/uploads/paystack_ui.png)

When the payment is successfull, your browser will show an alert, indicating a successfull transaction, with a reference key.

![paystack payment form](/uploads/paystack_integration_ui_testcard.png)

With this Inline method, everything works on a single page. Also notice the `callback` and `onClose` object key. The `callback` allows you to control the user experience within a callback function if the payment is successfull. e.g Redirecting the user to a Thank You page.  

## <a id="standard"></a>Paystack Standard

This is the standard approach of collecting payments on your web app. The standard approach is a better and secure way to integrate within your php web app.

Now, for this approach to work on your server you need to confirm that your server can conclude a TLSv1.2 connection. Most up-to-date server have this capability. If you're on a web server, you contact your service provider for guidance if you have any SSL errors.

For this approach, you'll will need to create two new files.

```
   initialize.php
   callback.php
```
## Initialize a transaction

Paste the following code on the `initialize.php`

```
<?php
$curl = curl_init();

$email = "your@email.com";
$amount = 30000;  //the amount in kobo. This value is actually NGN 300

// url to go to after payment
$callback_url = 'myapp.com/pay/callback.php';  

curl_setopt_array($curl, array(
  CURLOPT_URL => "https://api.paystack.co/transaction/initialize",
  CURLOPT_RETURNTRANSFER => true,
  CURLOPT_CUSTOMREQUEST => "POST",
  CURLOPT_POSTFIELDS => json_encode([
    'amount'=>$amount,
    'email'=>$email,
    'callback_url' => $callback_url
  ]),
  CURLOPT_HTTPHEADER => [
    "authorization: Bearer sk_test_36658e3260b1d1668b563e6d8268e46ad6da3273", //replace this with your own test key
    "content-type: application/json",
    "cache-control: no-cache"
  ],
));

$response = curl_exec($curl);
$err = curl_error($curl);

if($err){
  // there was an error contacting the Paystack API
  die('Curl returned error: ' . $err);
}

$tranx = json_decode($response, true);

if(!$tranx->status){
  // there was an error from the API
  print_r('API returned error: ' . $tranx['message']);
}

// comment out this line if you want to redirect the user to the payment page
print_r($tranx);
// redirect to page so User can pay
// uncomment this line to allow the user redirect to the payment page
header('Location: ' . $tranx['data']['authorization_url']);

```

The `initialize.php` will initilize your customer transaction with the paystack API and redirect the user to a Paystack payment page. 

> On a live server, replace the test key with your own live secret key. Look for the line with comment *'replace this with your own test key'* and remove the sk_test_xxxxxxxxx to your secret key.

Note that, the `$email` and `$amount` are the customer's email address and the amount they are to pay while the `$callback_url` is the url the customer will be redirect to after payment. 

Bringing the customers back to your site is an important part of the standard approach, so don't forget to change the `$callback_url` to that of your app.

The email and amount can be collected through forms or whatever way you intended. 

> The `$amount` is in Nigeria Kobo, so always add double zeros on any amount you are charging the customer. e.g 100000 for 1000

*You can use this [money tool](https://walletinvestor.com/converter/ngn/kobocoin/) for accuracy on complicated amount.*

When the customer enters their card details, Paystack will validate and charge the card. When successful, it will then redirect back to your `callback_url` set when initializing the transaction or on your dashboard at: https://dashboard.paystack.co/#/settings/developer .

>  If your `callback_url` is not set, your customers see a "Transaction was successful" message without any redirect. 


### Verify the Transaction
Now, since the callback was specify in your code, you need to set the callback.php. 

Enter the code below inside the `callback.php`

```
<?php

$curl = curl_init();
$reference = isset($_GET['reference']) ? $_GET['reference'] : '';
if(!$reference){
  die('No reference supplied');
}

curl_setopt_array($curl, array(
  CURLOPT_URL => "https://api.paystack.co/transaction/verify/" . rawurlencode($reference),
  CURLOPT_RETURNTRANSFER => true,
  CURLOPT_HTTPHEADER => [
    "accept: application/json",
    "authorization: Bearer sk_test_36658e3260b1d1668b563e6d8268e46ad6da3273",
    "cache-control: no-cache"
  ],
));

$response = curl_exec($curl);
$err = curl_error($curl);

if($err){
	// there was an error contacting the Paystack API
  die('Curl returned error: ' . $err);
}

$tranx = json_decode($response);

if(!$tranx->status){
  // there was an error from the API
  die('API returned error: ' . $tranx->message);
}

if('success' == $tranx->data->status){
  // transaction was successful...
  // please check other things like whether you already gave value for this ref
  // if the email matches the customer who owns the product etc
  // Give value
  echo "<h2>Thank you for making a purchase. Your file has bee sent your email.</h2>"
}

```

If you follow the steps correctly. You will get the following result.

!['paystack standard demo'](/uploads/paystack_demo.gif)

If you run into the error below. 

```
API returned error: Transaction reference not found
```
Then make sure the SECRET_KEY in `callback.php` is same with one used in the `initialize.php` and the callback url should be a live domain.

Congratulation, you just integrated paystack payment into your app.

> Have any problem, you can comment. If it urgent, you can message me on twitter.

## Hints

Go to dashboard > settings > webhook/keys to get your public and secret key for both the live and test.

The live keys are used for production purpose. While the public is for testing purpose.

To enable live mode on paystack, you'll need to submit your business details.

## External readings
Stephen Jude. [How To Verify Paystack Payment Transaction With PHP and jQuery](https://medium.com/@stephenjudesuccess/how-to-verify-paystack-payment-transaction-with-php-and-jquery-96329f69afc8), Medium.com.

Chiwex blog. [How to integrate paystack payment system with PHP](https://chiwex.com/integrate-paystack-payment-system/).

[Paystack documentation. ](https://developers.paystack.co/docs/)