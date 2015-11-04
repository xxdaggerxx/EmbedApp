# Polychemy's Embedable App. BETA.
Polychemy's Embeddable App Implementation.<br>

<p>
Polychemy's Embeddable apps allows any online retailer to embed Polcyhemy's customizable jewelry display.<br>
Customers can customize thier own products and purchase directly of your site, with out leaving your web site.<br>
The apps can be hooked up to the your shopping cart, ensuring a much better user expirence for your customers.<br>

</p>

By Using our Apps, you agree to our [Privacy Policy](http://www.polychemy.com/Privacy.php) and [Terms & Conditions] (http://www.polychemy.com/Policies.php)



<h1> How Does it Work? </h1>
here's a brief overview of the app implimentation.<br>

<ul>
<li>Embed the Polychemy App to your web site, with our simple I-Frame Code, and configure settings.</li>
<li>When the customers presses "Add To Cart", A POST request is send to your shopping cart page, with product details and image. Customization Data is also sent in JSON.</li>
<li>Once your customers completes the check out process on your site and confirms payment. You place the order with a POST request to our API, forwarding the product details and customization data. Multiple products can be sent in one cart.</li>
<li> Finally , once payment is made, we will begin manufacturing.</li>
</ul>

<h1>Step 1 - Embed Iframe App.</h1>
Add this Code anywhere in the BODY Tags of your HTML.
```javascript
<!-----POLYCHEMY EMBED CODE - Copy & Paste this code between the Body tages of your HTML	---->
<div id="iframeEmbed"></id>		
<script type="text/javascript">
//Set Up variables For Embedable App.///
var USER = "[ACCESS_CODE]";
//Set default product.
var productName = "Roman Name Ring";
//Do you have an external cart?
//var shoppingcart = "http://localhost/FakeShoppingCart.php";
//if no shopping cart leave shoppingcart variable empty
var shoppingcart = "";
var buyButton = "Add To Cart";
var currency = "USD";
//Dimensions	
var P_height = "1300";
var P_width = "100%";
</script>
<!--END POLYCHEMY EMBED CODE - Polychemy.com-->
<script src="http://polychemy.com/js/polyEmbed.js"></script>

```
Replace the [Access code] with the one issued to you.<br>
<b>productName</b> variable is the name of the default product. You can change this to which ever you prefer. You can also use the product SKU.<br>
<b>shoppingcart</b> variable is most important. This is your shopping cart page that will recieve the product Details. Leave this empty if you prefer to use Polychemy.com's shopping cart (var shoppingcart = "";) .<br>
<b>buyButton</b> - Customize the checkout button. Default is "Add To Cart".<br>
<b>currency</b> - The Default Currency.<br>

<h1>Step 2 - Create Your Shopping Cart.</h1>
After the user has finished customizing thier products on the app, and is ready to check out.<br>
The "Check Out" Button is pressed. Variables will be sent via POST Method to your shopping cart you specified in the previous step.<br>
POST Variables Details.<br>
<ul>

<li><b>Name</b> - Name of the product "Roman Name Ring"</li>
<li><b>Price</b> - Retail Price of Product "119.99"</li>
<li><b>Image</b> - Image address of product "http://polychemy3d.com/ModelDATABASE.php?getfile=JPG&ID=659520651679486100" </li>
<li><b>ProductDATA</b> - A Stringified JSON Data</li>
<li><b>ID</b> - User ID.</li>

</ul>

This data should be stored on your shopping cart database.

<h1>Step 3 - Send the order to Polychemy</h1>
<p>
Once your customers has finsihed the checkout process, and payment has been made. You will have to send us the order, and customization details from the previous step.
</p>

Here is a PHP example of placing an order with our order.php API.

```PHP
<?php
//Set Customer data and shipping details.
//We Will ship the product to this address.
$CustomerData = new stdClass();
$CustomerData->email = "customer@gmail.com";
$CustomerData->street = "Ave 12";
$CustomerData->city = "New York";
$CustomerData->state = "New York";
$CustomerData->zip = "123232";
$CustomerData->country = "United Statesdsf";
$CustomerData->name = "John Doe";
$CustomerData->hpnumber = "383748743";
$CustomerData->occasion = "none";
//Leave these variables blank.
$CustomerData->gender = "none";
$CustomerData->forwho = "none";
$CustomerData->coupon = "";
$CustomerData->cdtoken = "";

//send polcyehmy invoice. if false, no invoice will be sent.
//If oyu want to send your own invoice, then keep this as FALSE.
$CustomerData->sendinvoice = false;

//Identification Variables
$referalData = new stdClass();

///***Replace ACCESS ID with the Access ID you were given.
$referalData->referalID = "[ACCESS ID]";
$referalData->type = "[ACCESS ID]";

///Add Product Data into shopping cart array.
//This product Data is given to you from the previous step. Product data contains the customization settings for each product.
//If there are multiple products in the shopping cart, then add all to the array.
$ShoppingCart = array();
array_push($ShoppingCart, json_decode($_POST["ProductDATA"]));



$customizationData = new stdClass();
$customizationData->customerData = $CustomerData;
$customizationData->ShoppingCart = $ShoppingCart;

//**Set User ID. The user ID is given to you from the previous setp. Any USER ID, from any product in the cart will be fine.
$customizationData->ID = $_POST["ID"];

//get referal information if there's any
$customizationData->referal = $referalData;	
//set payment type.
$customizationData->paymentType = "ExternalCart";

//** This is your Secret Key.
$customizationData->secret = "[SECRET KEY]";


//CURL POST Script.
//set POST variables and URL.
$url = 'https://www.polychemy.com/php/Order.php';
$fields = array(
						'customerData' => urlencode(json_encode($customizationData)),
						'command' => urlencode("addOrder")
				);

//url-ify the data for the POST
$fields_string="";
foreach($fields as $key=>$value) { $fields_string .= $key.'='.$value.'&'; }
rtrim($fields_string, '&');

//open connection
$ch = curl_init();

//set the url, number of POST vars, POST data
curl_setopt($ch,CURLOPT_URL, $url);
curl_setopt($ch,CURLOPT_POST, count($fields));
curl_setopt($ch,CURLOPT_POSTFIELDS, $fields_string);

curl_setopt($ch, CURLOPT_SSL_VERIFYPEER, false);

//execute post
$result = curl_exec($ch);


//close connection
curl_close($ch);
echo $result;
?>
```
Here's a JSON example request.<br>
You will have to send the following JSON request to POST variable 'customerData'.<br>
And a 'addOrder' string to POST variable 'command'.<br>
Please make sure you stringify the data and URL encode it before sending it over to our system.

```json
{
	"customerData":{
		"email":"aaron.issac@gmail.com",
		"street":"Ave 12",
		"city":"New York",
		"state":"New York",
		"zip":"123232",
		"country":"United States",
		"name":"John Doe",
		"hpnumber":"383748743",
		"gender":"none",
		"forwho":"none",
		"occasion":"none",
		"coupon":"",
		"cdtoken":"",
		"sendinvoice":false
	},
	"ShoppingCart":[{ JSON ENCODED PRODUCT DATA }],
	"ID":"641567956",
	"referal":{
		"referalID":"[ACCESS ID]",
		"type":"[ACCESS ID]"
	},
	"paymentType":"ExternalCart",
	"secret":"polychemy"
}

```

Here's another JSON Example. This time with a test product (from productDATA).
This is how your JSON should look when you pass the data to us. Be sure to serealize the JSON request before sending it to us

```Json
{
	"customerData":{
	"email":"aaron.issac@gmail.com",
	"street":"Ave 12",
	"city":"New York",
	"state":"New York",
	"zip":"123232",
	"country":"United States",
	"name":"John Doe",
	"hpnumber":"383748743",
	"gender":"none",
	"forwho":"none",
	"occasion":"none",
	"coupon":"",
	"cdtoken":"",
	"sendinvoice":false
	},
	"ShoppingCart":[
		{
		"type":"Roman Name Ring",
		"sku":"ROMANRING",
		"arg":[
		"small",
		"Sterling_Silver",
		"6"
		],
		"argvalue":[
		"Custom Text",
		"Material",
		"Ring Size"
		],
		"commandlist":[
		"TEXT",
		"MATERIAL",
		"RINGSIZE"
		],
		"RAWcommandlist":[
		"TEXT:7:Love:CHECKCHAR",
		"MATERIAL:ALL",
		"RINGSIZE:ALL"
		],
		"priceDisplay":121.58,
		"folder":"3568471218459308000",
		"itemnum":"3568471218459308000",
		"image":"http:\/\/polychemy3d.com\/ModelDATABASE.php?getfile=JPG&ID=3568471218459308000",
		"volume":"0.3335311903633674",
		"price":129.99
		}
	],
"ID":"641567956",
"referal":{
	"referalID":"[ACCESS ID]",
	"type":"[ACCESS ID]"
}

```


If everyhting is sucessful you will get the following JSON response:<br/>
```json
{"result":"OK"}
```
The order has been placed and we will begin manufacturing, ONLY after payment is made with us.<br>
Payment must be made in USD only.
