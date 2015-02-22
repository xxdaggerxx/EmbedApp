# Polychemy's Embedable App.
Polychemy's Embeddable App Implementation.<br>

<p>
Polychemy's Embeddable apps allows any online retailer to embed Polcyhemy's customizable jewelry display.<br>
Customers can customize thier own products and purchase directly of your site, with out leaving your web site.<br>
The apps can be hooked up to the your shopping cart, ensuring a much better user expirence for your customers.<br>

</p>


<h1> How Does it Work? </h1>
here's a brief overview of the app implimentation.<br>

<ul>
<li>Embed the Polychemy App to your web site, with our simple I-Frame Code, and configure settings.</li>
<li>When the customers presses "Add To Cart", A POST request is send to your shopping cart page, with product details and image. Customization Data is also sent in JSON.</li>
<li>Once your customers completes the check out process on your site and confirms payment. You place the order with a POST request to our API, forwarding the product details and customization data. Multiple products can be sent in one cart.</li>
<li> Finally , once payment is made, we will begin manufacturing.</li>
</ul>

<h1>(Step 1) Embed Iframe App.</h1>
