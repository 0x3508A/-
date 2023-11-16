# UPI Payment URI and UPI QR Code

## How the URI for UPI Looks like

`upi://pay?pa=nath23@upi&pn=Aorj&am=100&tn=donate&cu=INR`

Here `pa=nath23@upi` is the **UPI ID** `nath23@upi`.
The `pn=Aorj` is the Person / Organization's name whom the ID belongs to.
The Amount for transaction is `am=100`.
This can also be a decimal number.
Finally the Transaction Type `tn=donate` for Donation
Currency is added at the end `cu=INR`.
When the `am` part is not there then it becomes a normal **UPI ID**.

Here is basic QR Code generated using :

<https://upiqr.in/>

```
Name   : Pramod Kumar Mahajan
UPI ID : haritech@upi

URI =
upi://pay?cu=INR&pa=haritech@upi&pn=Pramod%20Kumar%20Mahajan
```

Here is the Basic QR Code.

Now Translating a URI to QR Code using:
<https://www.qr-code-generator.com/>

Using URI:

```
upi://pay?pa=jaijai@upi&pn=Bhim%20Sen&tn=Vegitables&am=200&cu=INR

Name: Bhim Sen
UPI : jaijai@upi
Transaction : Vegitables
Amount : 200
Currency : INR
```

![UPI QR Code Generated](./upi-payment-uri-qr-code/2022-11-15_13-35-QR.png)


----
<!-- Footer Begins Here -->
## Links

- [Back to Misc. Hub](./README.md)
- [Back to Root Document](../README.md)
