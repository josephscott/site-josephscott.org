---
title: Percentiles
---

<p>
Provide a space or new line separated list of numbers.
</p>

<textarea class="numbers" cols="35" rows="10"></textarea>

<p class="results"></p>

<script>
document.addEventListener("DOMContentLoaded", function() {
	let $qs = document.querySelector.bind( document );

	let numbers_el = $qs( '.numbers' );
	let results_el = $qs( '.results' );

	let do_percentiles = ( event ) => {
		let out = '';
		let numbers = event.target.value.trim().split( /\s+/ );

		numbers.forEach( ( num, i ) => {
			let parsed_num = Number( num );
			if ( !isNaN( parsed_num ) ) {
				numbers[i] = parsed_num;
			} else {
				delete numbers[i];
			}
		} );

		numbers.sort( ( a, b ) => { return a - b } );

		out = " --- Ranked ---\n";
		[ 25, 50, 75, 90, 95 ].forEach( ( p ) => {
			p_decimal = p / 100;

			// Ranked method
			let index = p_decimal * numbers.length;
			index = Math.floor( index );
			let ranked = numbers[index];
			ranked = new Intl.NumberFormat( 'en-US', {} ).format( ranked );

			out += `p${p} = ${ranked}\n`;
		} );

		results_el.innerText += out;
	}

	numbers_el.addEventListener( 'input', do_percentiles );
});
</script>
