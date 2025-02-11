# SEPA-QR-for-Woocommerce (GDPR-compliant)
Plug-and-Play Plugin for Woocommerce
# Before you start
The plugin comes as is and free. However a real person has put real work into it. So if you use it please do s.th. good. Use your efforts, your time for beneficial projects or whatever!
# Prerequisite
php GD2 extension must be installed as the QR-Code generator by [fellwell15](https://github.com/fellwell5/bezahlcode/) requires this. **I guess this is a standard module and for most people nothing has to be done here!**
# Installation
Nothing special:
* Search for SEPA QR in the WordPress Plugin directory and you will find https://wordpress.org/plugins/mxp-sepa-qr-code-addon-for-woocommerce
* click install and acitvate

## Manual installation
* Download the ZIP-file via "Code --> Download Zip"
* Extract ZIP-File and make a new ZIP-File from the mxp-sepaqr directory
* In Woocommerce via "install" and upload or manually placing mxp-sepaqr.zip into the plugin folder.
# What it does
places an image with the SEPA-QR-Code in the
* thank-you-page after placing an order
* email you get from woocommerce

In the backend:
* the QR code generator creates the QR-code locally. There is **no Google-API nor external server in use**!
* the QR code generator is from [fellwell15](https://github.com/fellwell5/bezahlcode/)
* plugin registers a get-parameter (configurable, default mxp_qr) for testing purposes and, if desired, to create links to the cached QR codes.
* the prefix mxp is used throghout the plugin to avoid collisions with other plugins and functions. mxp stands for www.musicalexperten.de (musical experts). Remember where you've seen it first! ;-)


# Hooking into other plugins
I use a plugin for [PDF-invoices and packaging slips](https://docs.wpovernight.com/home/woocommerce-pdf-invoices-packing-slips/pdf-template-action-hooks/).  Refer to this sample to hook the QR-Code into whatever you like:

```
/wp-content/themes/Your(Child)Theme/functions.php
```

```php
/* QR-Code in invoices */
add_action( 'wpo_wcpdf_after_order_details', 'wpo_wcpdf_qr_code', 10, 2 );
function wpo_wcpdf_qr_code ($document_type, $order) {
	require_once WP_PLUGIN_DIR . '/mxp-sepa-qr-code-addon-for-woocommerce/muxp-sepaqr.php';
    $muxp_order = wc_get_order( $order);
	$order_id  = $order->get_id();
 	if ( !empty($muxp_order->get_total()) && (float)$order->get_total() > 0 ) {
		echo '<h1>QR-Code for your online banking app<h1>';
		echo '<img class="bacs-qrcode" src="' . esc_attr(muxp_get_qrcode($order->get_total(), $order_id)) . '" alt="qr-code"></p>';
	} 
}
```

# Translation
https://translate.wordpress.org/projects/wp-plugins/mxp-sepa-qr-code-addon-for-woocommerce/

# Configuration / if it does not work
The plugin comes with a little fallback: In case the BIC, IBAN, etc. are not shown open the **mxp-sepaqr.php** in the [programing code](https://github.com/Coernel82/SEPA-QR-for-Woocommerce/blob/afbacf58264c7afccd6b7f29e3f3105cb0e95b3b/mxp-sepaqr/mxp-sepaqr.php#L45-L50) you can hardcode some variables and translations. You'll find explanations in the comments.
## Advanced configuration of the qr-code itself
Have a look at [fellwell15](https://github.com/fellwell5/bezahlcode/)

# Testing and troubleshooting
## Simple way
Install the plugin and order s.th. in your shop using BACS (direct bank transfer).
## To test if the QR-Code generator is working
www.yourwebpage.de/?mxp_qr=something  = creates a real QR with dummyvalues 11-11
[Working example](https://www.musicalexperten.de/?mxp_qr=something)
## To find an existing cached QR-Code, query for a valid md5 string. If it does not exist in cache or transients, a sad smiley will appear.
www.yourwebpage.de/?mxp_qr=351436ef4b279e1811a6c68a2dd58b1b 
results in a sad smiley. [Working example](https://www.musicalexperten.de/?mxp_qr=351436ef4b279e1811a6c68a2dd58b1b)

# Remarks
Storing the QR code in cache or transients is only needed if you want to use a link instead of a picture inside the email. Details in the program code.

# Support
The program has been written by a professional programmer - however fully free of charge and without detailed knowledge about WooCommerce. The program comes as is and we cannot give support. I have no clue about it and the programmer can't work for free!

# Full integration in Woocommerce
I am more then happy if someone integrates the code into the Woocommerce core! The topic is discussed here: https://github.com/woocommerce/woocommerce/issues/27661
