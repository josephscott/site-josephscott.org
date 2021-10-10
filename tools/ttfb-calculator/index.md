---
title: TTFB Calculator
---

<style>
@media ( min-width: 600px ) {
	.ttfb-wrapper {
		width: 65%;
	}
}

input {
	width: 75px;
}
</style>

<div class="ttfb-wrapper">

<p>
Adjust the network RTT ( Round Trip Time ), and the server time each step of the HTTP request ( DNS, TCP, TLS, HTTP ).  One round is assumed for each.  All values in are in ms ( milliseconds ).
</p>

<p>
<small>
<code>
ttfb = ( rtt + dns ) + ( rtt + tcp ) + ( rtt + tls ) + ( rtt + http );
</code>
</small>
</p>

<strong>Total TTFB:</strong> <span id="ttfb-total">&nbsp;&nbsp;&nbsp;</span> ms

<div class="form">
    <div>
        <input type="text" class="rtt calc" name="rtt" value="85" autofocus>
        <label for="rtt">Network RTT</label>
    </div>
	<hr />
    <div>
        <input type="text" class="dns calc" name="dns" value="7">
        <label for="dns">DNS Server Time</label>
    </div>
    <div>
        <input type="text" class="tcp calc" name="tcp" value="3">
        <label for="tcp">TCP Server Time</label>
    </div>
    <div>
        <input type="text" class="tls calc" name="tls" value="50">
        <label for="tls">TLS Server Time</label>
    </div>
    <div>
        <input type="text" class="http calc" name="http" value="100">
        <label for="tls">HTTP Server Time</label>
    </div>
</div>

</div>

<script>
document.addEventListener("DOMContentLoaded", function() {
	let $qs = document.querySelector.bind( document );
	let $qsa = document.querySelectorAll.bind( document );

	let change = () => {
		let rtt = parseInt( $qs( '.rtt' ).value );
		let dns = parseInt( $qs( '.dns' ).value );
		let tcp = parseInt( $qs( '.tcp' ).value );
		let tls = parseInt( $qs( '.tls' ).value );
		let http = parseInt( $qs( '.http' ).value );

		let total = ( rtt * 4 ) + dns + tcp + tls + http;
		total = new Intl.NumberFormat( 'en-US' ).format( total );
		$qs( '#ttfb-total' ).innerText = total;
	};
	change();

	$qsa( '.calc' ).forEach( ( f ) => {
		f.oninput = change;
	} );
});
</script>
