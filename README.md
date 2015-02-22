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

<h1>Step 1 - Embed Iframe App.</h1>
Add this Code anywhere in the <BODY></BODY> Tags of your HTML.
```javascript
<iframe src="" width="1000" height="1200" scrolling="no" id="iframeEmbed"></iframe>
<script type="text/javascript">
//Set Up variables For Embedable App.///
var USER = "[ACCESS CODE]";
var productName = "Roman Name Ring";
var shoppingcart = "http://localhost/FakeShoppingCart.php";
////////////////////////////////////////////////////////////
var Poly_embed = document.getElementById("iframeEmbed");
Poly_embed.src = "Jewelry.php?name="+productName+"&embed=true&User="+USER+"&shoppingCart="+encodeURIComponent(shoppingcart);
</script>
```
Replace the [Access code] with the one issued to you.<br>
<b>productName variable</b> is the name of the default product. You can change this to which ever you prefer. You can also use the product SKU.<br>
<b>shoppingcart variable</b> is most important. This is your shopping cart page that will recieve the product Details. Leave this empty if you prefer to use Polychemy.com's shopping cart.

<h1>Step 2 - Create Your Shopping Cart.</h1>
After the user has finished customizing thier products on the app, and is ready to check out.<br>
The "Check Out" Button is pressed. Variables will be sent with the POST Method to the shopping cart you specified in the previous step.<br>
POST Variables.<br>

<ul>

<li></li>

</ul>
